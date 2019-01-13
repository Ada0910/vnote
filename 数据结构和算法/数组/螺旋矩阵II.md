# 1. 题目
给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

示例:

输入: 3
输出:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
# 2. Solution
# 3. Code
```
class Solution {
    public int[][] generateMatrix(int n) {
        
        int[][] result = new int[n][n];
        int up = 0;
        int down = n-1;
        int left = 0;
        int right = n-1;
        int count = 1;
        while (left<=right &&up<=down){
            
            //右
            for(int i = left;i<=right ;i++ ){
                result[up][i] = count++;
            }
            up++;
            
            //下
            for(int i= up ;i<=down  ;i++){
                result[i][right] = count++;
            }
            right--;
            //左
            for(int i = right ;i>=left ;i--){
                result[down][i] = count++;
            }
            down --;
            //上
            for(int i=down ;i>=up;i-- ){
                result[i][left] = count++;
            }
            left++;
            
        }
        return result ;
        
    }
}
```