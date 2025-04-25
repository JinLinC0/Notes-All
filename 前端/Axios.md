## `Axios`

### `axios`请求工具

`axios`是通过`promise`实现对`ajax`技术的封装；`ajax`技术实现了网页的局部数据刷新，`axios`实现了对`ajax`的封装，[`axios`官方文档](https://axios-http.com/zh/docs/intro)

安装：`npm install axios --save`

`axios`支持多种请求方式：

- `axios(config)`
- `ios.request(config)`
- `axios.get(url[, config])`
- `axios.delete(url[, config])`
- `axios.head(url[, config])`
- `axios.post(url[, data[, config]])`
- `axios.put(url[, data[, config]])`
- `axios.patch(url[, data[, config]])`

具体形式：

```js
axios({
    url: '/getUsers',
    method: 'get',
    responseType: 'json', // 默认的
    data: {
        //'a': 1,
        //'b': 2,
    }
}).then(function (response) {
    console.log(response);
    console.log(response.data);
}).catch(function (error) {
    console.log(error);
    })
```

#### 在`Vue3`项目使用`Axios`

首先需要在`main.js`或者`main.ts`文件中加载`axios`组件和请求`request`

```js
import { Request } from '@/utils/request';
import VueAxios from 'vue-axios'

app.use(VueAxios, Request.init())
```

`request`请求的基本形式如下：

```ts
import axios, {AxiosInstance, AxiosRequestConfig, AxiosResponse} from 'axios';
import {message, notification} from 'ant-design-vue';
 
export class Request {
    public static axiosInstance: AxiosInstance;
 
    // constructor() {
    //     // 创建axios实例
    //     this.axiosInstance = axios.create({timeout: 1000 * 12});
    //     // 初始化拦截器
    //     this.initInterceptors();
    // }
 
    public static init() {
        // 创建axios实例
        this.axiosInstance = axios.create({
            baseURL: '/api',
            timeout: 6000
        });
        // 初始化拦截器
        this.initInterceptors();
        return axios;
    }
 
    // 为了让http.ts中获取初始化好的axios实例
    // public getInterceptors() {
    //     return this.axiosInstance;
    // }
 
 
 
    // 初始化拦截器
    public static initInterceptors() {
        // 设置post请求头
        this.axiosInstance.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
        /**
         * 请求拦截器
         * 每次请求前，如果存在token则在请求头中携带token
         */
        this.axiosInstance.interceptors.request.use(
            (config: AxiosRequestConfig) => {
 
                // const token = Vue.ls.get(ACCESS_TOKEN)
                // if (token) {
                //     config.headers['Authorization'] = 'Bearer ' + token
                // }
 
                // 登录流程控制中，根据本地是否存在token判断用户的登录情况
                // 但是即使token存在，也有可能token是过期的，所以在每次的请求头中携带token
                // 后台根据携带的token判断用户的登录情况，并返回给我们对应的状态码
                // if (config.headers.isJwt) {
                    const token = localStorage.getItem('ACCESS_TOKEN');
                    if (token) {
                        config.headers.Authorization = 'Bearer ' + token;
                    }
                // }
                return config;
            },
            (error: any) => {
                console.log(error);
            },
        );
 
 
        // 响应拦截器
        this.axiosInstance.interceptors.response.use(
            // 请求成功
            (response: AxiosResponse) => {
                // if (res.headers.authorization) {
                //     localStorage.setItem('id_token', res.headers.authorization);
                // } else {
                //     if (res.data && res.data.token) {
                //         localStorage.setItem('id_token', res.data.token);
                //     }
                // }
 
                if (response.status === 200) {
                    // return Promise.resolve(response.data);
                    return response;
                } else {
                    Request.errorHandle(response);
                    // return Promise.reject(response.data);
                    return response;
                }
            },
            // 请求失败
            (error: any) => {
                const {response} = error;
                if (response) {
                    // 请求已发出，但是不在2xx的范围
                    Request.errorHandle(response);
                    return Promise.reject(response.data);
                } else {
                    // 处理断网的情况
                    // eg:请求超时或断网时，更新state的network状态
                    // network状态在app.vue中控制着一个全局的断网提示组件的显示隐藏
                    // 关于断网组件中的刷新重新获取数据，会在断网组件中说明
                    message.warn('网络连接异常,请稍后再试!');
                }
            });
    }
 
 
    /**
     * http握手错误
     * @param res 响应回调,根据不同响应进行不同操作
     */
    private static errorHandle(res: any) {
        // 状态码判断
        switch (res.status) {
            case 401:
                break;
            case 403:
                break;
            case 404:
                message.warn('请求的资源不存在');
                break;
            default:
                message.warn('连接错误');
        }
    }
}
```

编写接口：

```js
import { Request } from '@/utils/request';
 
export function login (parameter: any)  {
    return Request.axiosInstance({
        url: '/cxLogin',
        method: 'post',
        data: parameter
    })
}
```

##### `Axios` 请求

发起一个 `GET` 请求：

```js
const axios = require('axios');

// 向给定ID的用户发起请求
axios.get('/user?ID=12345')
  .then(function (response) {
    // 处理成功情况
    console.log(response);
  })
  .catch(function (error) {
    // 处理错误情况
    console.log(error);
  })
  .finally(function () {
    // 总是会执行
  });
```

发起一个 `POST` 请求：

```js
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

##### `axios` 封装

项目中会有很多的模块都需要发送网络请求，常见的比如登录模块，首页模块等，如果我们项目中直接使用诸如`axios.get()`, `axios.post()`，会存在很多弊端，所以我们需要对`axios`进行封装
