# 1. X509EncodedKeySpec概述
## 1.1. 类结构
```
java.lang.Object
  java.security.spec.EncodedKeySpec
      java.security.spec.X509EncodedKeySpec
```
- 声明：
```
public class X509EncodedKeySpec extends EncodedKeySpec
```
# 2. 方法
## 2.1. 构造方法
```
public X509EncodedKeySpec(byte[] encodedKey)  
```
根据给定的编码密钥创建一个新的 X509EncodedKeySpec。
参数：encodedKey - 假设按照 X.509 标准对其进行编码的密钥。复制数组的内容，以防随后进行修改。
抛出：NullPointerException - 如果 encodedKey 为 null。
## 2.2. 方法详细
```
public byte[] getEncoded()  
```
返回按照 X.509 标准进行编码的密钥的字节。
覆盖：类 EncodedKeySpec 中的 getEncoded
返回：密钥的 X.509 编码。每次调用此方法时，都返回一个新数组。


```
public final String getFormat() 
```
返回与此密钥规范关联的编码格式的名称。
指定者：类 EncodedKeySpec 中的 getFormat
返回：字符串 "X.509"。
# 3. KeyFactory(密钥工厂)