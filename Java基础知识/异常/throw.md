# 1. 概念
- throw是在程序中明确引发异常
- throw是语句抛出一个异常。
语法：throw (异常对象);
      如：
      `  throw e;`

一般会用于程序出现某种逻辑时程序员主动抛出某种特定类型的异常


```
public static void main(String[] args) {
		String s = "abc";
		if(s.equals("abc")) {
			throw new NumberFormatException();
		} else {
			System.out.println(s);
		}
		//function();
}
```
会抛出异常：

```
Exception in thread "main" java.lang.NumberFormatException

at test.ExceptionTest.main(ExceptionTest.java:67)

```