# 1. Integer类
```
public class Test03 {
 
    public static void main(String[] args) {
        Integer f1 = 100, f2 = 100, f3 = 150, f4 = 150;
 
        System.out.println(f1 == f2);
        System.out.println(f3 == f4);
    }
}
```
简单的说，如果整型字面量的值在-128到127之间，那么不会new新的Integer对象，而是直接引用常量池中的Integer对象，
所以上面的面试题中f1==f2的结果是true，而f3==f4的结果是false。

```
 Integer ac = new Integer(128);
        Integer bc = 128;                  // 将3自动装箱成Integer类型
        int cc = 128;
        System.out.println(ac == bc);     // false 两个引用没有引用同一对象
        System.out.println(ac == cc);     // true a自动拆箱成int类型再和c比较
        System.out.println(cc==bc);     //true
```