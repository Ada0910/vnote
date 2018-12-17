# 1. 题目描述
编写一个函数来验证输入的字符串是否是有效的 IPv4 或 IPv6 地址。

IPv4 地址由十进制数和点来表示，每个地址包含4个十进制数，其范围为 0 - 255， 用(".")分割。比如，172.16.254.1；

同时，IPv4 地址内的数不会以 0 开头。比如，地址 172.16.254.01 是不合法的。

IPv6 地址由8组16进制的数字来表示，每组表示 16 比特。这些组数字通过 (":")分割。比如,  2001:0db8:85a3:0000:0000:8a2e:0370:7334 是一个有效的地址。而且，我们可以加入一些以 0 开头的数字，字母可以使用大写，也可以是小写。所以， 2001:db8:85a3:0:0:8A2E:0370:7334 也是一个有效的 IPv6 address地址 (即，忽略 0 开头，忽略大小写)。

然而，我们不能因为某个组的值为 0，而使用一个空的组，以至于出现 (::) 的情况。 比如， 2001:0db8:85a3::8A2E:0370:7334 是无效的 IPv6 地址。

同时，在 IPv6 地址中，多余的 0 也是不被允许的。比如， 02001:0db8:85a3:0000:0000:8a2e:0370:7334 是无效的。

说明: 你可以认为给定的字符串里没有空格或者其他特殊字符。

示例 1:

输入: "172.16.254.1"

输出: "IPv4"

解释: 这是一个有效的 IPv4 地址, 所以返回 "IPv4"。
示例 2:

输入: "2001:0db8:85a3:0:0:8A2E:0370:7334"

输出: "IPv6"

解释: 这是一个有效的 IPv6 地址, 所以返回 "IPv6"。
示例 3:

输入: "256.256.256.256"

输出: "Neither"

解释: 这个地址既不是 IPv4 也不是 IPv6 地址。
# 2. Solution
1.先判断是何种类型类型的IP地址，进入相应的判别函数，如果都不是就直接返回Neither。

2.构建split()函数，根据分割符个数N将字符串分割为N+1份（空字符串也为一份）。

3.若先找到'.'（IPv4分隔符）符号的：

```
1）先判断IP在'.'分割符下分割后的段数，若不为4则直接返回Neither。

2）判断分割后每段的字符串长度，若为空串或串长大于3，不合法。

3）判断每段字符串是否有前导0，有则不合法。

4）判断字符串中是否有非数字字符，有则不合法。

5）判断字符串的大小，若大于255则也不合法。

6）若各段字符串均无问题，返回"IPv4"。
```

4.若先找到':'（IPv6分隔符）符号的：
```
1）先判断IP在':'分割符下分割后的段数，若不为8则直接返回Neither。

2）判断分割后每段的字符串长度，若为空串或串长大于4，不合法。

3）判断字符串中是否有除[0~9a~fA~F]外的字符，有则不合法。

4）若各段字符串均无问题，返回"IPv6"。
```

# 3. Code
```
class Solution {
    public String validIPAddress(String IP) {
        if (IP.length() <= 0) {
            return "Neither";
        }
        else
        //ipv4
        if(isIPv4(IP)){
            return "IPv4";
        }else if(isIPv6(IP)){//IPv6
            return "IPv6";
        }else{
            return "Neither";
        }
    }
        
          //判断是否为ipv4
        private boolean isIPv4(String ip){
            if(ip.charAt(ip.length()-1)=='.')
                return false;
            
            //以下标.分割ip地址
            String[] result = ip.split("\\.");
            if(result.length !=4)//长度不符合
                return false;
            
            for(String val:result){
                if("".equals(val) || val.length() > 3|| (val.length() > 1 && val.charAt(0)=='0'))
                   return false;
                  
                   for(int i=0;i<val.length();i++){
                       if(!(val.charAt(i)>='0'&&val.charAt(i)<='9')){
                           return false;
                       }
                   }
                   
                   if(Integer.parseInt(val)>255)
                   return false;      
        
         
    }
                   return true;
        }                 
                   
                   //判断是否为ipv6
         private boolean isIPv6(String ip){
            if(ip.charAt(ip.length()-1)==':')
                return false;
             
             String[] res= ip.toLowerCase().split("\\:");
             if(res.length !=8)
                 return false;
             
             for(String t : res){
                 if(t.length()>4 ||t.length()<=0)
                     return false;
                 
                 for(int j = 0;j<t.length();j++){
                     char c = t.charAt(j);
                        if (c < '0' || ( c > '9' && c < 'a') || c > 'f') {
                    return false;
                }
                } 
             }
            return true;
        }
      
            
}
```