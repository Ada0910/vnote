# 1. 网关过滤器 GatewayFilter
GatewayFilter 网关过滤器用于拦截并链式处理web请求，可以实现横切的与应用无关的需求，比如：安全、访问超时的设置等。

GatewayFilter

从类图中可以看到，GatewayFilter 有三个实现类：

OrderedGatewayFilter 是一个有序的网关过滤器

GatewayFilterAdapter 是一个适配器类，是web处理器（FilteringWebHandler）中的内部类

ModifyResponseGatewayFilter 是一个内部类，用于修改响应体

本文就分别介绍一下网关过滤器的实现类


# 2. 有序的网关过滤器 OrderedGatewayFilter
过滤器大多都是有优先级的，因此有序的网关过滤器的使用场景会很多。在实现过滤器接口的同时，有序网关过滤器也实现了 Ordered 接口，构造函数中传入需要代理的网关过滤器以及优先级就可以构造一个有序的网关过滤器。具体的过滤功能的实现在被代理的过滤器中实现的，因此在此只需要调用代理的过滤器即可。

```
public class OrderedGatewayFilter implements GatewayFilter, Ordered {

	private final GatewayFilter delegate;
	private final int order;

	public OrderedGatewayFilter(GatewayFilter delegate, int order) {
		this.delegate = delegate;
		this.order = order;
	}
	--------------------------------省略-------------------------------
}

```
# 3. 网关过滤器适配器 GatewayFilterAdapter
在网关过滤器链 GatewayFilterChain 中会使用 GatewayFilter 过滤请求，GatewayFilterAdapter的作用就是将全局过滤器 GlobalFilter 适配成 网关过滤器 GatewayFilter。

// FilteringWebHandler.java

```
private static class GatewayFilterAdapter implements GatewayFilter {

		private final GlobalFilter delegate;

		public GatewayFilterAdapter(GlobalFilter delegate) {
			this.delegate = delegate;
		}

		@Override
		public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
			return this.delegate.filter(exchange, chain);
		}

		@Override
		public String toString() {
			final StringBuilder sb = new StringBuilder("GatewayFilterAdapter{");
			sb.append("delegate=").append(delegate);
			sb.append('}');
			return sb.toString();
		}
	}
```
# 4. ModifyResponseGatewayFilter
ModifyResponseGatewayFilter 是 ModifyResponseBodyGatewayFilterFactory 的内部类，用于修改响应体的信息
```
// ModifyResponseBodyGatewayFilterFactory.java

	public class ModifyResponseGatewayFilter implements GatewayFilter, Ordered {
		private final Config config;

		public ModifyResponseGatewayFilter(Config config) {
			this.config = config;
		}

		@Override
		@SuppressWarnings("unchecked")
		public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {

			ServerHttpResponseDecorator responseDecorator = new ServerHttpResponseDecorator(exchange.getResponse()) {

				@Override
				public Mono<Void> writeWith(Publisher<? extends DataBuffer> body) {

					Class inClass = config.getInClass();
					Class outClass = config.getOutClass();

					String originalResponseContentType = exchange.getAttribute(ORIGINAL_RESPONSE_CONTENT_TYPE_ATTR);
					HttpHeaders httpHeaders = new HttpHeaders();
					//explicitly add it in this way instead of 'httpHeaders.setContentType(originalResponseContentType)'
					//this will prevent exception in case of using non-standard media types like "Content-Type: image"
					httpHeaders.add(HttpHeaders.CONTENT_TYPE, originalResponseContentType);
					ResponseAdapter responseAdapter = new ResponseAdapter(body, httpHeaders);
					DefaultClientResponse clientResponse = new DefaultClientResponse(responseAdapter, ExchangeStrategies.withDefaults());

					//TODO: flux or mono
					Mono modifiedBody = clientResponse.bodyToMono(inClass)
							.flatMap(originalBody -> config.rewriteFunction.apply(exchange, originalBody));

					BodyInserter bodyInserter = BodyInserters.fromPublisher(modifiedBody, outClass);
					CachedBodyOutputMessage outputMessage = new CachedBodyOutputMessage(exchange, exchange.getResponse().getHeaders());
					return bodyInserter.insert(outputMessage, new BodyInserterContext())
							.then(Mono.defer(() -> {
								long contentLength1 = getDelegate().getHeaders().getContentLength();
								Flux<DataBuffer> messageBody = outputMessage.getBody();
								//TODO: if (inputStream instanceof Mono) {
									HttpHeaders headers = getDelegate().getHeaders();
									if (/*headers.getContentLength() < 0 &&*/ !headers.containsKey(HttpHeaders.TRANSFER_ENCODING)) {
										messageBody = messageBody.doOnNext(data -> headers.setContentLength(data.readableByteCount()));
									}
								// }
								//TODO: use isStreamingMediaType?
								return getDelegate().writeWith(messageBody);
							}));
				}

				@Override
				public Mono<Void> writeAndFlushWith(Publisher<? extends Publisher<? extends DataBuffer>> body) {
					return writeWith(Flux.from(body)
							.flatMapSequential(p -> p));
				}
			};

			return chain.filter(exchange.mutate().response(responseDecorator).build());
		}

		@Override
		public int getOrder() {
			return NettyWriteResponseFilter.WRITE_RESPONSE_FILTER_ORDER - 1;
		}

	}
```

