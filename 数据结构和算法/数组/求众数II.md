给定一个大小为 n 的数组，找出其中所有出现超过 ⌊ n/3 ⌋ 次的元素。

说明: 要求算法的时间复杂度为 O(n)，空间复杂度为 O(1)。

示例 1:

输入: [3,2,3]
输出: [3]
示例 2:

输入: [1,1,1,3,3,2,2,2]
输出: [1,2]
```
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        
     int count1= 0;
        int count2= 0;
        int num1 = 0;
        int num2 = 1;
          for(int num: nums) {
            if (count1 == 0) {
                num1 = num;
                count1 = 1;
            } else if (num1 == num) {
                count1 ++;
            } else if (count2 == 0) {
                num2 = num;
                count2 = 1;
            } else if (num2 == num) {
                count2 ++;
            } else {
                count1 --;
                count2 --;
                if (count1 == 0 && count2 > 0) {
                    num1 = num2;
                    count1 = count2;
                    num2 = 0;
                    count2 = 0;
                }
            }
        }
        if(count1>0){
            count1=0;
            for(int num:nums){
                if(num==num1)
                    count1++;
            }
        }
        
          if(count2>0){
            count2=0;
            for(int num:nums){
                if(num==num2)
                    count2++;
            }
        }
        
        List<Integer> result =new ArrayList<Integer>();
        if(count1*3>nums.length) result.add(num1);
          if(count2*3>nums.length) result.add(num2);
        return result;
        
        
    }
}
```