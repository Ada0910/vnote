# 1. 通过kaptcha.xml配置
## 1.1. 用idea新建一个spring Initializr
## 1.2. - 添加kaptcha的依赖:
```
<!-- kaptcha验证码 -->
        <dependency>
        <groupId>com.github.penggle</groupId>
            <artifactId>kaptcha</artifactId>
            <version>2.3.2</version>
        </dependency>
```

## 1.3. 在resources下面新建kaptcha.xml
内容如下:
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 生成kaptcha的bean-->
    <bean id="captchaProducer" class="com.google.code.kaptcha.impl.DefaultKaptcha">
        <property name="config">
            <bean class="com.google.code.kaptcha.util.Config">
                <constructor-arg type="java.util.Properties">
                    <!--设置kaptcha属性 -->
                    <props>
                        <prop key = "kaptcha.border ">yes</prop>
                        <prop key="kaptcha.border.color">105,179,90</prop>
                        <prop key="kaptcha.textproducer.font.color">blue</prop>
                        <prop key="kaptcha.image.width">100</prop>
                        <prop key="kaptcha.image.height">50</prop>
                        <prop key="kaptcha.textproducer.font.size">27</prop>
                        <prop key="kaptcha.session.key">code</prop>
                        <prop key="kaptcha.textproducer.char.length">4</prop>
                        <prop key="kaptcha.textproducer.font.names">宋体,楷体,微软雅黑</prop>
                        <prop key="kaptcha.textproducer.char.string">0123456789ABCEFGHIJKLMNOPQRSTUVWXYZ</prop>
                        <prop key="kaptcha.obscurificator.impl">com.google.code.kaptcha.impl.WaterRipple</prop>
                        <prop key="kaptcha.noise.color">black</prop>
                        <prop key="kaptcha.noise.impl">com.google.code.kaptcha.impl.DefaultNoise</prop>
                        <prop key="kaptcha.background.clear.from">185,56,213</prop>
                        <prop key="kaptcha.background.clear.to">white</prop>
                        <prop key="kaptcha.textproducer.char.space">3</prop>
                    </props>
                </constructor-arg>
            </bean>
        </property>
    </bean>
</beans>

```
注:kaptcha.xml中的内容其实就是和spring 整合kaptcha时spring-kaptcha.xml中内容一样，就是将kaptcha交给spring容器管理，设置一些属性，然后要用的时候直接注入即可。

## 1.4. 加载kaptcha.xml:
在springboot启动类上加上@ImportResource(locations = {"classpath:kaptcha/kaptcha.xml"})，加了这个注解，springboot就会去加载kaptcha.xml文件。注意kaptcha.xml的路径不要写错，classpath在此处是resources目录。
## 1.5. 编写controller:
```
CodeController.java
@Controller
public class CodeController {
    @Autowired
    private Producer captchaProducer = null;
    @RequestMapping("/kaptcha")
    public void getKaptchaImage(HttpServletRequest request, HttpServletResponse response) throws Exception {
        HttpSession session = request.getSession();
        response.setDateHeader("Expires", 0);
        response.setHeader("Cache-Control", "no-store, no-cache, must-revalidate");
        response.addHeader("Cache-Control", "post-check=0, pre-check=0");
        response.setHeader("Pragma", "no-cache");
        response.setContentType("image/jpeg");
        //生成验证码
        String capText = captchaProducer.createText();
        session.setAttribute(Constants.KAPTCHA_SESSION_KEY, capText);
        //向客户端写出
        BufferedImage bi = captchaProducer.createImage(capText);
        ServletOutputStream out = response.getOutputStream();
        ImageIO.write(bi, "jpg", out);
        try {
            out.flush();
        } finally {
            out.close();
        }
    }
}
```

注:在这个controller径注入刚刚kaptcha.xml中配置的那个bean，然后就可以使用它生成验证码，以及向客户端输出验证码;记住这个类的路由，前端页面验证码的src需要指向这个路由。
## 1.6. 新建验证码比对工具类:
CodeUtil.java
```
public class CodeUtil {
    /**
     * 将获取到的前端参数转为string类型
     * @param request
     * @param key
     * @return
     */
    public static String getString(HttpServletRequest request, String key) {
        try {
            String result = request.getParameter(key);
            if(result != null) {
                result = result.trim();
            }
            if("".equals(result)) {
                result = null;
            }
            return result;
        }catch(Exception e) {
            return null;
        }
    }
    /**
     * 验证码校验
     * @param request
     * @return
     */
    public static boolean checkVerifyCode(HttpServletRequest request) {
        //获取生成的验证码
        String verifyCodeExpected = (String) request.getSession().getAttribute(com.google.code.kaptcha.Constants.KAPTCHA_SESSION_KEY);
        //获取用户输入的验证码
        String verifyCodeActual = CodeUtil.getString(request, "verifyCodeActual");
        if(verifyCodeActual == null ||!verifyCodeActual.equals(verifyCodeExpected)) {
            return false;
        }
        return true;
    }
}

```

注:这个类用来比对生成的验证码与用户输入的验证码。生成的验证码会自动加到session中，用户输入的通getParameter获得。注意getParameter的key值要与页面中验证码的name值一致。
## 1.7. 使用验证码:
①Controller
HelloWorld.java
```
@RestController
public class HelloWorld {
    @RequestMapping("/hello")
    public String hello(HttpServletRequest request) {
        if (!CodeUtil.checkVerifyCode(request)) {
            return "验证码有误！";
        } else {
            return "hello,world";
        }
    }
}
```

②页面
```
hello.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        function refresh() {
            document.getElementById('captcha_img').src="/kaptcha?"+Math.random();
        }
    </script>
</head>
<body>
<form action="/hello" method="post">
    验证码:  <input type="text" placeholder="请输入验证码" name="verifyCodeActual">
    <div class="item-input">
        <img id="captcha_img" alt="点击更换" title="点击更换"
             onclick="refresh()" src="/kaptcha" />
    </div>
    <input type="submit" value="提交" />
</form>

</body>
</html>
```

注意:验证码本质是一张图片，所以用<img >标签，然后通过src = "/kaptcha"指向生成验证码的那个controller的路由即可;通过onclick = “refresh()”调用js代码实现点击切换功能;`<input name = "verifyCodeActual ">`中要注意name的值，在CodeUtil中通过request的getParameter()方法获取用户输入的验证码时传入的key值就应该和这里的name值一致。

# 2. 通过配置类来配置kaptcha
## 2.1. 配置kaptcha
相比于方式一，一增二减。
减:

- 把kaptcha.xml删掉
- 把启动类上的@ImportResource(locations = {"classpath:kaptcha/kaptcha.xml"})注解删掉
增:
增加KaptchaConfig配置类
## 2.2. 新建KaptchaConfig配置类
内容如下:
KaptchaConfig.java

```
package com.ada.web.config;

import com.google.code.kaptcha.impl.DefaultKaptcha;
import com.google.code.kaptcha.util.Config;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;

import java.util.Properties;

/**
 * @author Ada
 * @ClassName :KaptchaConfig
 * @date 2019/6/10 22:47
 * @Description:
 */
/*该注解是：是该类可以注入到spring容器中*/
@Component
public class KaptchaConfig {

    /**
     * @return com.google.code.kaptcha.impl.DefaultKaptcha
     * @Author Ada
     * @Date 22:55 2019/6/10
     * @Param []
     * @Description 将Kaptcha注入到IOC容器中，return一个设置好属性的实例
     **/
    @Bean
    public DefaultKaptcha getDefaultKaptcha() {
        DefaultKaptcha defaultKaptcha = new DefaultKaptcha();
        Properties properties = new Properties();
        properties.put("kaptcha.border", "no");
        properties.put("kaptcha.textproducer.font.color", "black");
        properties.put("kaptcha.image.width", "150");
        properties.put("kaptcha.image.height", "40");
        properties.put("kaptcha.textproducer.font.size", "30");
        properties.put("kaptcha.session.key", "verifyCode");
        properties.put("kaptcha.textproducer.char.space", "5");
        Config config = new Config(properties);
        defaultKaptcha.setConfig(config);
        return defaultKaptcha;
    }
}

```
这个类用来配置Kaptcha，就相当于方式一的kaptcha.xml，把kaptcha加入IOC容器，然后return 回一个设置好属性的实例，最后注入到CodeController中，在CodeController中就可以使用它生成验证码。要特别注意return captchaProducer;与private Producer captchaProducer = null;中captchaProducer名字要一样，不然就加载不到这个bean

## 2.3. 测试

