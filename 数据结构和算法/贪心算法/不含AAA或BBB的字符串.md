# 1. 题目
给定两个整数 A 和 B，返回任意字符串 S，要求满足：

S 的长度为 A + B，且正好包含 A 个 'a' 字母与 B 个 'b' 字母；
子串 'aaa' 没有出现在 S 中；
子串 'bbb' 没有出现在 S 中。
 

示例 1：

输入：A = 1, B = 2
输出："abb"
解释："abb", "bab" 和 "bba" 都是正确答案。
示例 2：

输入：A = 4, B = 1
输出："aabaa"
 

提示：

0 <= A <= 100
0 <= B <= 100
对于给定的 A 和 B，保证存在满足要求的 S。
# 2. Solution
# 3. Code
```
class Solution {
    public String strWithout3a3b(int A, int B) {
        
        StringBuilder result  = new  StringBuilder();
        int size = A +B ;
        int a=0 ,b=0;
        for(int i = 0 ;i<size;i++){
            if((A>B&&a!=2)||b==2){
                result.append("a");
                b=0;
                a++;
                A--;
                
            }else if((B>=A&&b!=2)||a==2){
                result.append("b");
                a=0;
                b++;
                B--;
            }
        }
        
        return result.toString();
    }
}
```