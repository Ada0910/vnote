# 1. 概念：
- throws是方法可能抛出异常的声明。(用在声明方法时，表示该方法可能要抛出异常)
语法：
```
[(修饰符)](返回值类型)(方法名)([参数列表])[throws(异常类)]{......}
      如：      
      public void function() throws Exception{......}
```

- 当某个方法可能会抛出某种异常时用于throws 声明可能抛出的异常，然后交给上层调用它的方法程序处理。如：

```

public static void function() throws NumberFormatException{
		String s = "abc";
		System.out.println(Double.parseDouble(s));
	}
	
	public static void main(String[] args) {
		try {
			function();
		} catch (NumberFormatException e) {
			System.err.println("非数据类型不能转换。");
			//e.printStackTrace();
		}
}

```


处理结果如下：
非数据类型不能转换。
