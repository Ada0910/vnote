# 1. BASE64
## 1.1. 定义
按照RFC2045的定义，Base64被定义为

- Base64内容传送编码被设计用来把任意序列的8位字节描述为一种不易被人直接识别的形式。
- The Base64 Content-Transfer-Encoding is designed to represent arbitrary sequences of octets in a form that need not be humanly readable.
- 常见于邮件、http加密，截取http信息，你就会发现登录操作的用户名、密码字段通过BASE64加密的
## 1.2. 测试
```
   /**测试BASE64编码*/
        String inputStr = "加密牛逼";
        System.out.println("原文:" + inputStr);
        byte[] inputData = inputStr.getBytes();
        String code = Coder.encryptBASE64(inputData);
        System.out.println("BASE64加密后:\n" + code);
        byte[] output = Coder.decryptBASE64(code);
        String outputStr = new String(output);
        System.out.println("BASE64解密后:\n" + outputStr);
        // 验证BASE64加密解密一致性
        assertEquals(inputStr, outputStr);
```
## 1.3. 结果
![](_v_images/20191018113927944_29492.png)
# 2. MD5
## 2.1. 定义
- message-digest algorithm 5 
- （信息-摘要算法）缩写，广泛用于加密和解密技术，常用于文件校验。

不管文件多大，经过MD5后都能生成唯一的MD5值。
怎么用？当然是把ISO经过MD5后产生MD5的值。一般下载linux-ISO的朋友都见过下载链接旁边放着MD5的串。就是用来验证文件是否一致的
## 2.2. 测试代码
```
/**MD5*/
        BigInteger md5 = new BigInteger(Coder.encryptMD5(inputData));
        System.out.println("MD5:\n" + md5.toString(16));

```
## 2.3. 测试结果
![](_v_images/20191018141724641_10256.png)
# 3. SHA
## 3.1. 定义
SHA(Secure Hash Algorithm，安全散列算法），数字签名等密码学应用中重要的工具，被广泛地应用于电子商务等信息安全领域。虽然，SHA与MD5通过碰撞法都被破解了， 但是SHA仍然是公认的安全加密算法，较之MD5更为安全。
## 3.2. 测试代码
```
    BigInteger sha = new BigInteger(Coder.encryptSHA(inputData));  
    System.err.println("SHA:\n" + sha.toString(32));
```
## 3.3. 结果
![](_v_images/20191018142829434_28868.png)
# 4. HMAC
## 4.1. 定义
HMAC(Hash Message Authentication Code，散列消息鉴别码，基于密钥的Hash算法的认证协议。消息鉴别码实现鉴别的原理是，用公开函数和密钥产生一个固定长度的值作为认证标识，用这个标识鉴别消息的完整性。使用一个密钥生成一个固定大小的小数据块，即MAC，并将其加入到消息中，然后传输。接收方利用与发送方共享的密钥进行鉴别认证等。
## 4.2. 测试代码
```
  /**HMAC*/
        BigInteger mac = new BigInteger(Coder.encryptHMAC(inputData, inputStr));
        System.err.println("HMAC:\n" + mac.toString(16));
```
## 4.3. 结果
![](_v_images/20191018144337305_20285.png)
# 5. 基础类
```
package com.example.demo.security;

import sun.misc.BASE64Decoder;
import sun.misc.BASE64Encoder;

import javax.crypto.KeyGenerator;
import javax.crypto.Mac;
import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import java.security.MessageDigest;

/**
 * @ClassName:Coder
 * @author: Ada
 * @date 2019/10/18 10:45
 * @Description:
 */
public abstract class Coder {
    public static final String KEY_SHA = "SHA";
    public static final String KEY_MD5 = "MD5";

    /**
     * MAC算法可选以下多种算法
     *
     * <pre>
     * HmacMD5
     * HmacSHA1
     * HmacSHA256
     * HmacSHA384
     * HmacSHA512
     * </pre>
     */
    public static final String KEY_MAC = "HmacMD5";

    /**
     * BASE64解密
     *
     * @param key
     * @return
     * @throws Exception
     */
    public static byte[] decryptBASE64(String key) throws Exception {
        return (new BASE64Decoder()).decodeBuffer(key);
    }

    /**
     * BASE64加密
     *
     * @param key
     * @return
     * @throws Exception
     */
    public static String encryptBASE64(byte[] key) throws Exception {
        return (new BASE64Encoder()).encodeBuffer(key);
    }

    /**
     * MD5加密
     *
     * @param data
     * @return
     * @throws Exception
     */
    public static byte[] encryptMD5(byte[] data) throws Exception {

        MessageDigest md5 = MessageDigest.getInstance(KEY_MD5);
        md5.update(data);

        return md5.digest();

    }

    /**
     * SHA加密
     *
     * @param data
     * @return
     * @throws Exception
     */
    public static byte[] encryptSHA(byte[] data) throws Exception {

        MessageDigest sha = MessageDigest.getInstance(KEY_SHA);
        sha.update(data);

        return sha.digest();

    }

    /**
     * 初始化HMAC密钥
     *
     * @return
     * @throws Exception
     */
    public static String initMacKey() throws Exception {
        KeyGenerator keyGenerator = KeyGenerator.getInstance(KEY_MAC);

        SecretKey secretKey = keyGenerator.generateKey();
        return encryptBASE64(secretKey.getEncoded());
    }

    /**
     * HMAC加密
     *
     * @param data
     * @param key
     * @return
     * @throws Exception
     */
    public static byte[] encryptHMAC(byte[] data, String key) throws Exception {

        SecretKey secretKey = new SecretKeySpec(decryptBASE64(key), KEY_MAC);
        Mac mac = Mac.getInstance(secretKey.getAlgorithm());
        mac.init(secretKey);

        return mac.doFinal(data);

    }
}

```
