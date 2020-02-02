# 1. SpringMVC的工作流程(图)
![](_v_images/20200202173507158_12029.png =1872x)
# 2. 工作流程描述
- 1.用户向服务器发送请求，请求被Spring 前端控制Servelt DispatcherServlet捕获；
- 2. DispatcherServlet对请求URL进行解析，得到请求资源标识符（URI）。然后根据该URI，调用HandlerMapping获得该Handler配置的所有相关的对象（包括Handler对象以及Handler对象对应的拦截器），最后以HandlerExecutionChain对象的形式返回；
-3. DispatcherServlet 根据获得的Handler，选择一个合适的HandlerAdapter。（附注：如果成功获得HandlerAdapter后，此时将开始执行拦截器的preHandler(...)方法）
- 4.  提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)。 在填充Handler的入参过程中，根据你的配置，Spring将帮你做一些额外的工作：
   **HttpMessageConveter**： 将请求消息（如Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息
   **数据转换**：对请求消息进行数据转换。如String转换成Integer、Double等
   **数据根式化**：对请求消息进行数据格式化。 如将字符串转换成格式化数字或格式化日期等
   **数据验证**： 验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中
 - 5.  Handler执行完成后，向DispatcherServlet 返回一个ModelAndView对象；
-  6.  根据返回的ModelAndView，选择一个适合的ViewResolver（必须是已经注册到Spring容器中的ViewResolver)返回给DispatcherServlet ；
-  7. ViewResolver 结合Model和View，来渲染视图
- 8. 将渲染结果返回给客户端
# 3. Handler、HandlerMapping和HandlerAdapter
## 3.1. Handler
- Handler是对请求进行处理的最小单位，一个最典型的Handler即Controller，Handler用于对Request进行处理，同时将最终的处理结果写入Response。Controller中有方法叫handleRequest()，在这里就可以对Request进行处理并返回一个ModelAndView
## 3.2. HandlerMapping
- HandlerMapping中保存请求的URL和handler的关系，同时其中包含了很多的方法用来添加url和handler的键值对或者是根据url的名字检索出相应的handler
- 举例：
DefaultAnnotationHandlerMapping：将扫描当前所有已经注册的spring beans中的@requestmapping标注以找出url 和 handler method处理函数的关系并予以关联
## 3.3. HandlerAdapter
- 通过HandlerAdapter来实际调用处理函数
- 举例:
AnnotationMethodHandlerAdapter：DispatcherServlet中根据handlermapping找到对应的handler method后，首先检查当前工程中注册的所有可用的handlerAdapter,根据handlerAdapter中的supports方法找到可以使用的handlerAdapter。通过调用handlerAdapter中的handle方法来处理及准备handler method中的参数及annotation(这就是spring mvc如何将reqeust中的参数变成handle method中的输入参数的地方)，最终调用实际的handle method
# 4. 相关问题
## 4.1. 例如
Spring为什么要结合使用HandlerMapping以及HandlerAdapter来处理Handler?
    - 符合面向对象中的单一职责原则，代码架构清晰，便于维护，最重要的是代码可复用性高。如HandlerAdapter可能会被用于处理多种Handler