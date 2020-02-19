# 1. 截取
StringUtils中常用的截取字符串的方法如下：
```
substring(String str,int start)
substring(String str,int start, int end)
substringAfter(String str,String separator)
substringAfterLast(String str,String separator)
substringBefore(String str,String separator)
substringBeforeLast(String str,String separator)
substringBetween(String str,String tag)
```
 需要注意的是，截取字符串时，若被截取的字符串为null或""，则截取之后的返回的字符串也为null和""。

## 1.1. 根据位置截取
根据指定位置截取字符串，当指定的截取位置为非负数时，则从左往右开始截取，第一位为0，后面依次类推，但当索引值为负数时，则从右往左截取，注意此时右侧第一位为-1

- 只指定了起始位置，则截取至字符串末尾：

```
StringUtils.substring(null, 2); // "" null和""截取后都返回null和""
StringUtils.substring(null, 2); // null
StringUtils.substring("china", 0); // china 指定的起始截取位置为0，则从第一位开始截取，也就是不截取
StringUtils.substring("china", 2); // ina 指定的截取位置为2，则从左往右第三位开始截取
StringUtils.substring("china", -2); // na 指定的截取位置为-2，则从右往左第二位开始截取
```

- 指定了起始位置和结束位置，则从起始位置开始截取到结束位置（但不包含结束位置）：

```
StringUtils.substring(null, 2, 4); // null null和""截取后都返回null和""
StringUtils.substring("", 2, 4); // ""
StringUtils.substring("china", 0, 0); // ""
StringUtils.substring("china", 2, 4); // in
StringUtils.substring("china", -2, -4); // in
StringUtils.substring("china", 2, -3); // ""
StringUtils.substring("china", 2, -1); // in
```
## 1.2. 根据指定的分隔符 
根据指定的分隔符进行截取（不包含该分隔符）

- 从分隔符第一次出现的位置向后截取：
```
StringUtils.substringAfter("china", "i"); // na 从第一次出现"i"的位置向后截取，不包含第一次出现的"i"
StringUtils.substringAfter("china", "hi"); // na
StringUtils.substringAfter("chinachina","h")); // inachina
StringUtils.substringAfter("china", "a"); // ""
StringUtils.substringAfter("china", "d"); // "" 分隔符在要截取的字符串中不存在，则返回""
StringUtils.substringAfter("china", "")); // china 分隔符为""，则返回原字符串
Stringtils.substringAfter("china", null); // "" 分隔符为null，则返回""
```
- 从分隔符最后一次出现的位置向后截取：

```
StringUtils.substringAfterLast("china", "i"); // na
StringUtils.substringAfterLast("chinachina", "i"); // na "i"最后出现的位置向后截取
```

- 从分隔符第一次出现的位置向前截取：
```
StringUtils.substringBefore("china", "i"); // ch 
StringUtils.substringBefore("chinachina", "i"); // ch 从"i"第一次出现的位置向前截取
```
- 从分隔符最后一次出现的位置向前截取：

```
StringUtils.substringBeforeLast("china", "i");
StringUtils.substringBeforeLast("chinachina", "i"); // chinach
```

- 截取指定标记字符串之间的字符序列：

```
StringUtils.substringBetween(null, "ch") // null
StringUtils.substringBetween("", "") // ""
StringUtils.substringBetween("tagabctag", "") // "" 标记字符串为""，则截取后返回""
StringUtils.substringBetween("", "tag") // null // 注意此处返回的是null
StringUtils.substringBetween("tagabctag", null) // null 标记字符串为null，则截取后返回null
StringUtils.substringBetween("tagabctag", "tag") // "abc"
```
# 2. 去除空白
  去除字符串中的空白符是我们在处理字符串时经常遇到的问题，StringUtils中也封装了一些非常好用的方法来帮助我们解决这个问题：

```
trim(String str)
trimToEmpty(String str)
trimToNull(String str)
strip(String str)
stripToEmpty(String str)
stripToNull(String str)
deleteWhitespace(String str)
```
## 2.1. 去除字符串首尾的控制符
（char ≤ 32）
- trim(String str)：如果被去除的字符串的为null或""，则返回null和"":

```
StringUtils.trim(null); // null
StringUtils.trim(""); // ""
StringUtils.trim("     ");// ""
StringUtils.trim("abc"); // abc
StringUtils.trim("    abc    "); // abc
StringUtils.trim(" a b c "); // "a b c" 注意此处字符串内部的控制符是不去除的
```

- trimToEmpty(String str)：如果被去除的字符串的为null或""，则都返回"":

```
StringUtils.trimToEmpty(null); // "" 此处返回的是""
StringUtils.trimToEmpty(""); // ""
StringUtils.trimToEmpty("     ");// ""
StringUtils.trimToEmpty("abc"); // abc
StringUtils.trimToEmpty("    abc    "); // abc
StringUtils.trimToEmpty(" a b c "); // a b c
```

- trimToNull(String str)：如果被去除的字符串的为null或""，则都返回null:
```
StringUtils.trimToNull(null); // null
StringUtils.trimToNull(""); // null
StringUtils.trimToNull("     ");// null
StringUtils.trimToNull("abc"); // abc
StringUtils.trimToNull(" \t\r\nabc    "); // abc
StringUtils.trimToNull(" a b c "); // "a b c" 
```
## 2.2. 去除字符串首尾的空白符
(空白符主要包括' '，'\t'，'\r'，'\n'等等，具体的空白符可以参考Java API中Character类中isWhiteSpace()方法中的描述)：
- trim(String str)：如果被去除的字符串的为null或""，则返回null和"":

```
StringUtils.strip(null); // null
StringUtils.strip(""); // ""
StringUtils.strip("     ");// ""
StringUtils.strip("abc"); // abc
StringUtils.strip(" \t\r\n abc    "); // abc
StringUtils.strip(" a b c "); // a b c
```

- trimToEmpty(String str)：如果被去除的字符串的为null或""，则都返回"":

```
StringUtils.stripToEmpty(null); // null
StringUtils.stripToEmpty(""); // nulld
StringUtils.stripToEmpty("     ");// null
StringUtils.stripToEmpty("abc"); // abc
StringUtils.stripToEmpty(" \t\r\n abc    "); // abc
StringUtils.stripToEmpty(" a b c "); // "a b c"
```

- trimToNull(String str)：如果被去除的字符串的为null或""，则都返回null:

```
StringUtils.stripToNull(null); // null
StringUtils.stripToNull(""); // nulld
StringUtils.stripToNull("     ");// null
StringUtils.stripToNull("abc"); // abc
StringUtils.stripToNull(" \t\r\n abc    "); // abc
StringUtils.stripToNull(" a b c "); // "a b c"
```
## 2.3. 去除字符串中所有的空白符：

```
StringUtils.deleteWhitespace(null); // null
StringUtils.deleteWhitespace(""); // ""
StringUtils.deleteWhitespace("abc"); // "abc"
StringUtils.deleteWhitespace("   ab  c  "); // "abc"
```
# 3. 包含
StringUtils中判断是否包含的方法主要有：

```
contains(CharSequence seq, int searchChar)
contains(CharSequence seq, CharSequence searchSeq)
containsIgnoreCase(CharSequence str, CharSequence searchStr)
containsAny(CharSequence cs, char... searchChars)
containsAny(CharSequence cs, CharSequence searchChars)
containsOnly(CharSequence cs,char… valid)
containsOnly(CharSequence cs, String validChars)
containsNone(CharSequence cs,char… searchChars)
containsNone(CharSequence cs, String invalidChars)
startsWith(CharSequence str,CharSequence prefix)
startsWithIgnoreCase(CharSequence str,CharSequence prefix)
```
startsWithAny(CharSequence string,CharSequence… searchStrings)
## 3.1. 判断字符串中是否包含指定的字符或字符序列：
- 区分大小写：
```
StringUtils.contains(null, 'a'); // false
StringUtils.contains("china", null); // false
StringUtils.contains("", 'a'); // false
StringUtils.contains("china", 'a');// true
StringUtils.contains("china", 'z');//false
StringUtils.contains(null, "a"); // false
StringUtils.contains("china", null); // false
StringUtils.contains("", ""); // true
StringUtils.contains("abc", "");// true
StringUtils.contains("china", "na");// true
StringUtils.contains("abc", "z"); // false
```
- 不区分大小写：

```
StringUtils.containsIgnoreCase("china", 'a');// true
StringUtils.containsIgnoreCase("china", 'A');// true
StringUtils.containsIgnoreCase("china", 'Z');//false
StringUtils.containsIgnoreCase(null, "A"); // false
StringUtils.containsIgnoreCase("china", null); // false
StringUtils.containsIgnoreCase("", ""); // true
StringUtils.containsIgnoreCase("abc", "");// true         
StringUtils.containsIgnoreCase("china", "na");// true
StringUtils.containsIgnoreCase("china", "Na");// true
StringUtils.containsIgnoreCase("abc", "Z"); // false
```
## 3.2. 判断字符串中是否包含指定字符集合中或指定字符串中任一字符，区分大小写：
```

StringUtils.containsAny(null, 'a', 'b');// false
StringUtils.containsAny("", 'a', 'b');// false
StringUtils.containsAny("abc", 'a', 'z');// true
StringUtils.containsAny("abc", 'x', 'y');// false
StringUtils.containsAny("abc", 'A', 'z');// false
StringUtils.containsAny(null, "a");// false
StringUtils.containsAny("", "a");// false
StringUtils.containsAny("abc", "ab");// true
StringUtils.containsAny("abc", "ax");// true
StringUtils.containsAny("abc", "xy");// false
StringUtils.containsAny("abc", "Ax");// false
```
## 3.3. 判断字符串中是否不包含指定的字符或指定的字符串中的字符，区分大小写：

```
StringUtils.containsNone(null, 'a'); // true
StringUtils.containsNone("", 'a'); // true 注意这里，空串总是返回true
StringUtils.containsNone("china", ' '); // true 注意包含空白符为true
StringUtils.containsNone("china", '\t'); // true
StringUtils.containsNone("china", '\r'); // true
StringUtils.containsNone("china", 'x', 'y', 'z'); // true
StringUtils.containsNone("china", 'c', 'y', 'z'); // false
StringUtils.containsNone("china", 'C', 'y', 'z'); // true
StringUtils.containsNone(null, "a"); // true
StringUtils.containsNone("", "a"); // true
StringUtils.containsNone("china", ""); // true
StringUtils.containsNone("china", "xyz"); // true
StringUtils.containsNone("china", "cyz"); // false
StringUtils.containsNone("china", "Cyz"); // true
```
## 3.4. 判断字符串中的字符是否都是出自所指定的字符数组或字符串，区分大小写：

```
StringUtils.containsOnly(null, 'a');// false
StringUtils.containsOnly("", "a");// true
StringUtils.containsOnly("ab", ' ');// false
StringUtils.containsOnly("abab", 'a', 'b', 'c');// true
StringUtils.containsOnly("abcd", 'a', 'b', 'c');// false
StringUtils.containsOnly("Abab", 'a', 'b', 'c');// false
StringUtils.containsOnly(null, "a");// false
StringUtils.containsOnly("", "a"); // true
StringUtils.containsOnly("abab", "abc));// true
StringUtils.containsOnly("abcd", "abc"); // false
StringUtils.containsOnly("Abab", "abc");// false
```
## 3.5. 判断字符串是否以指定的字符序列开头：
- 区分大小写：

```
StringUtils.startsWith(null, null); // true
StringUtils.startsWith(null, "abc"); // false
StringUtils.startsWith("abcdef", null); // false
StringUtils.startsWith("abcdef", "abc"); // true
StringUtils.startsWith("ABCDEF", "abc"); // false
```

- 不区分大小写：

```
StringUtils.startsWithIgnoreCase(null, null);// true
StringUtils.startsWithIgnoreCase(null, "abc");// false
StringUtils.startsWithIgnoreCase("abcdef", null);// false
StringUtils.startsWithIgnoreCase("abcdef", "abc");// true
StringUtils.startsWithIgnoreCase("ABCDEF", "abc");// true
```
## 3.6. 判断字符串是否以指定的字符序列数组中任意一个开头，区分大小写：

```
StringUtils.startsWithAny(null, null);// false
StringUtils.startsWithAny(null, new String[] { "abc" });// false
StringUtils.startsWithAny("abcxyz", null);// false
StringUtils.startsWithAny("abcxyz", new String[] { "" });// true
StringUtils.startsWithAny("abcxyz", new String[] { "abc" });// true
StringUtils.startsWithAny("abcxyz", new String[] { null, "xyz", "abc" });// true
StringUtils.startsWithAny("abcxyz", null, "xyz", "ABCX");// false
StringUtils.startsWithAny("ABCXYZ", null, "xyz", "abc");// false
```
# 4. 查询索引
StringUtils中获取字符或字符序列在字符串中出现的索引下标的方法主要有：

```
indexOf(CharSequence seq, int searchChar)
indexOf(CharSequence seq,CharSequence searchSeq)
indexOfIgnoreCase
indexOf(CharSequence seq,CharSequence searchSeq,int startPos)
lastIndexOf(CharSequence seq,int searchChar)
lastIndexOfIgnoreCase(CharSequence str,CharSequence searchStr)
```
## 4.1. 获取指定字符
获取指定字符或字符序列在字符串中第一次出现的索引，若字符串中不包含该字符或字符序列，则返回-1，若字符串或字符序列为""或null，也返回-1（（但字符串和字符序列都为""的情况下，则返回0））：

- 区分大小写：

```
StringUtils.indexOf(null, 'a');// -1
StringUtils.indexOf("", 'a');// -1
StringUtils.indexOf("abca", 'a');// 0
StringUtils.indexOf("abca", 'b');// 1
StringUtils.indexOf("abca", 'A');// -1
StringUtils.indexOf(null, "a"); // -1
StringUtils.indexOf("abc", null); // -1
StringUtils.indexOf("", ""); // 0
StringUtils.indexOf("", "a"); // -1  注意这里第二个参数为""时则为0
StringUtils.indexOf("abc", "a"); // 0
StringUtils.indexOf("abc", "b"); // 1
StringUtils.indexOf("abc", "ab"); // 0
StringUtils.indexOf("abc", ""); // 0
```

- 不区分大小写：

```
StringUtils.indexOfIgnoreCase(null, "a"); // -1
StringUtils.indexOfIgnoreCase("abc", null); // -1
StringUtils.indexOfIgnoreCase("", ""); // 0
StringUtils.indexOfIgnoreCase("", "a");// -1
StringUtils.indexOfIgnoreCase("abc", "b));// 1
StringUtils.indexOfIgnoreCase("abc", "B"); // 1
```
## 4.2. 获取字符序列在字符串中指定位置之后第一次出现的索引
若字符串中指定位置之后不包含该字符序列，则返回-1，若字符串或字符序列为""或null，也返回-1（但字符串和字符序列都为""的情况下，结果就有点怪异，有时返回0，有时返回1，有时返回-1，根据指定的起始位置会有变化）：

- 区分大小写：

```
StringUtils.indexOf(null, "a", 2); // -1
StringUtils.indexOf("abc", null, 2); // -1
StringUtils.indexOf("", "", 0); // 0 注意此处和下一行都返回0，对比忽略大小写的情形，就有点不一样
StringUtils.indexOf("", "", 1); // 0
StringUtils.indexOf("", "", 2); // 0 
StringUtils.indexOf("", "a", 0); // -1 不包括第二个参数为""的情况
StringUtils.indexOf("abac", "a", 1); // 2
StringUtils.indexOf("abcab", "ab", 2); // 3
StringUtils.indexOf("abc", "a", -1); // 0 -1被当作是0
StringUtils.indexOf("abc", "a", 2); // -1
```

- 不区分大小写：

```
StringUtils.indexOfIgnoreCase("", "", 0)); // 0
StringUtils.indexOfIgnoreCase("", "", 0)); // 1 与不忽略大小写的情况不同，下面也是
StringUtils.indexOfIgnoreCase("", "", 0)); //-1 
StringUtils.indexOfIgnoreCase("abac", "A", 1)); // 2
StringUtils.indexOfIgnoreCase("abcab", "AB", 2)); // 3
StringUtils.indexOfIgnoreCase("abc", "B", -1)); // 1 -1被当作是0
```
## 4.3. 获取指定字符或字符序列在字符串中最后一次出现的索引
若字符串中不包含该字符序列，则返回-1，若字符串或字符序列为""或null，也返回-1（但字符串和字符序列都为""的情况下，返回0）：

- 区分大小写：

```
StringUtils.lastIndexOf(null, 'a'));// -1
StringUtils.lastIndexOf("", 'a'));// -1
StringUtils.lastIndexOf("abccba", 'a'));// 5
StringUtils.lastIndexOf("abccba", 'z'));// -1
StringUtils.lastIndexOf(null, "a"));// -1
StringUtils.lastIndexOf("abc", null));// -1
StringUtils.lastIndexOf("", ""));// 0
StringUtils.lastIndexOf("abc", "b"));// 1
StringUtils.lastIndexOf("abc", "ab"));// 0
StringUtils.lastIndexOf("abc", ""));// 3 返回字符串的长度
```
- 不区分大小写：
```
StringUtils.lastIndexOfIgnoreCase(null, "a");// -1
StringUtils.lastIndexOfIgnoreCase("abc", null);// -1
StringUtils.lastIndexOfIgnoreCase("", "");// 0
StringUtils.lastIndexOfIgnoreCase("abc", "B");// 1
StringUtils.lastIndexOfIgnoreCase("abc", "AB");// 0
StringUtils.lastIndexOfIgnoreCase("abc", "");// 3  返回字符串的长度
```