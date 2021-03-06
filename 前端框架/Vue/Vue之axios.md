# 1. 初级使用
## 1.1. 安装axios
```
npm install axios --save-dev
```
## 1.2. 在main.js中引用axios
```
import axios from 'axios';
Vue.prototype.$axios = axios //全局注册，使用方法为:this.$axios
```
## 1.3. 复制代码3、就可以在代码中使用axios发送请求了
```
this.$axios({
      method: "post",
      url: "apiURL", // 接口地址
      data: {
        keyword: "1"   // 传接口参数
      }
    })
      .then(response => {
        console.log(response, "success");   // 成功的返回      
      })
      .catch(error => console.log(error, "error")); // 失败的返回
```
复制代码log输出返回内容，所有的内容都在
![](_v_images/20191125150545002_2372.png)


# 2. 进阶使用
但是在实际使用中，我们往往还需要加入各种请求头，以及固定接口地址，所以为了方便使用，我们要重写封装axios以便添加拦截器
## 2.1. 新建文件，用来写封装axios的代码
位于 src/axios/index.js
```
import axios from 'axios';
import Qs from 'qs'; // 用来处理参数，可不使用，若要使用，npm安装： npm install qs
axios.defaults.baseURL = 'apiURL'; // 请求的默认域名
// 添加一个请求拦截器
axios.interceptors.request.use(config => {
    config.headers.languagetype = 'CN'; // 举例，加上一个公共头部
    config.data = Qs.stringify(config.data); // 处理数据，可不写
    return config;
  },
  err => {
    return Promise.reject(err);
  });

//添加一个响应拦截器

axios.interceptors.response.use(res => {
  //在这里对返回的数据进行处理
  console.log(res.data, '网络正常');
  return res.data;
}, err => {
  console.log('网络开了小差！请重试...');
  return Promise.reject(err);
})

export default axios 
```

## 2.2. main.js中引用
```
import axios from './axios'
Vue.prototype.$axios = axios //全局注册，使用方法为:this.$axios
```

## 2.3. 复制代码3、代码中使用依旧是
```
this.$axios({
      method: "post",
      url: "apiURL", // 接口地址
      data: {
        keyword: "1"   // 传接口参数
      }
    })
      .then(response => {
        console.log(response, "success");   // 成功的返回      
      })
      .catch(error => console.log(error, "error")); // 失败的返回
```
复制代码因为返回的结果进行了处理，所以只返回了接口的结果

![](_v_images/20191125150736859_24315.png)
查看network
![](_v_images/20191125150753441_8610.png =829x)
