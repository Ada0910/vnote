# 1. Math.round()
```(四舍五入)
将括号内的数+0.5之后，向下取值，

比如：round(3.4)就是3.4+0.5=3.9，向下取值是3，所以round(3.4)=3; 

round(-10.5)就是-10.5+0.5=-10，向下取值就是-10，所以round(-10.5)=-10

所以，Math.round(11.5)=12;

现在再来看，Math.round(11.5)，Math.round(-11.5)你应该知道等于多少了吧，掌握了方法就好解决问题了。
```
# 2. Math.ceil()
求最小的整数,但不小于本身.  
ceil的英文意义是天花板，该方法就表示向上取整

```
/**   
* @see  求最小的整数,但不小于本身  
* @param double   
* @return double   
*/  
System.out.println(Math.ceil(-1.1));  //-1.0
System.out.println(Math.ceil(-1.9));  //-1.0
System.out.println(Math.ceil(1.1));   //2.0
System.out.println(Math.ceil(1.9));   //2.0

```
# 3. Math.floor()
floor的英文意义是地板，该方法就表示向下取整，

所以，Math.floor(11.6)的结果为11,Math.floor(-11.6)的结果是-12；

```
/** 
* @see 求最大的整数,但不大于本身  
* @param double 
* @return double 
*/  
System.out.println(Math.floor(-1.1));  
System.out.println(Math.floor(-1.9));  
System.out.println(Math.floor(1.1));  
System.out.println(Math.floor(1.9));  

-2.0  
-2.0  
1.0  
1.0  
```
