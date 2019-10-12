# 1. ResponseEntity
ResponseEntity继承了HttpEntity，是HttpEntity的子类且可以添加HttpStatus状态码（推测HttpEntity不能添加HttpStatus状态码）。被用于RestTemplate和Controller层方法

## 1.1. 区分
- ResponseEntity ：标识整个http相应：状态码、头部信息、响应体内容(spring)
- @ResponseBody：加在请求处理方法上，能够处理方法结果值作为http响应体（springmvc）
- @ResponseStatus：加在方法上、返回自定义http状态码(spring)

