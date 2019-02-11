# 1. 题目
给定两个字符串s1, s2，找到使两个字符串相等所需删除字符的ASCII值的最小和。

示例 1:

输入: s1 = "sea", s2 = "eat"
输出: 231
解释: 在 "sea" 中删除 "s" 并将 "s" 的值(115)加入总和。
在 "eat" 中删除 "t" 并将 116 加入总和。
结束时，两个字符串相等，115 + 116 = 231 就是符合条件的最小和。
示例 2:

输入: s1 = "delete", s2 = "leet"
输出: 403
解释: 在 "delete" 中删除 "dee" 字符串变成 "let"，
将 100[d]+101[e]+101[e] 加入总和。在 "leet" 中删除 "e" 将 101[e] 加入总和。
结束时，两个字符串都等于 "let"，结果即为 100+101+101+101 = 403 。
如果改为将两个字符串转换为 "lee" 或 "eet"，我们会得到 433 或 417 的结果，比答案更大。
注意:

0 < s1.length, s2.length <= 1000。
所有字符串中的字符ASCII值在[97, 122]之间。
# 2. Solution
利用动归的思想，使用二维辅助数组 dp[i][j]，其含义代表字符串 s1 的子串 s1.sub(0,i-1) 和 s2 的子串 s2.sub(0,j-1) 需删除字符的ASCII值的最小和。 
（因为字符串的对于每个字符的位置是从基数 0 开始的）

根据上述说明的 dp[i][j] 的含义，其中，s1 的子串 s1.sub(0,i-1) 可以看作是 s1.sub(0,i-2)+s1(i-1) 组合而成，s2 的子串 s2.sub(0,j-1) 可以看作是 s2.sub(0,j-2)+s2(j-1) 组合而成。(s(x) 表示字符串 s 的下标为 x 的字符)。

# 3. Code
```
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
         int lena = s1.length();
         int lenb = s2.length();
        
        int[][] dp  = new int[lena+1][lenb+1];
        dp[0][0] = 0;
        for(int i =1 ;i<lenb+1;i++){
            dp[0][i] = dp[0][i-1]+s2.charAt(i-1);
        }
        
        for(int i = 1;i<lena+1;i++){
            dp[i][0] = dp[i-1][0] +s1.charAt(i-1);
            for(int j = 1;j<lenb+1;j++){
                if(s1.charAt(i-1)==s2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1] ;
                }else{
                    dp[i][j] = Math.min(dp[i-1][j]+s1.charAt(i-1),dp[i][j-1]+s2.charAt(j-1));
                }
            }
        }
        
        return dp[lena][lenb];
        
    }
}
```