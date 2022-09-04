# 开始

基于promise可以用于浏览器和node.js的网络请求库

**axios 是什么?**

axios 是一个基于ES6 `promise` 网络请求库，作用于node.js 和浏览器中。 它是 isomorphic 的(即同一套代码可以运行在浏览器和node.js中)。在服务端它使用原生 node.js 中 `http` 模块, 而在客户端(浏览器)则使用 `XMLHttpRequests`。

# 特性

- 从浏览器创建 XMLHttpRequests
- 从 node.js 创建 http 请求
- 支持 Promise API
- 拦截请求和响应
- 转换请求和响应数据
- 取消请求
- 自动转换JSON数据
- 🆕自动序列化数据对象成 multipart/form-data 或 x-www-form-urlencoded 编码格式
- 客户端支持防御XSRF

# 安装

使用 npm:

```bash
$ npm install axios
```

使用 bower:

```bash
$ bower install axios
```

使用 yarn:

```bash
$ yarn add axios
```

使用 pnpm:

```bash
$ pnpm add axios
```

使用 jsDelivr CDN:

```html
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```

使用 unpkg CDN:

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```



# 演示	

**注意: CommonJS 用法**

为了获得TypeScript类型推断（智能感知/自动完成），在CommonJS中使用 `require()` 导入时请使用以下方法：

```js
const axios = require('axios').default;

// axios.<method> 能够提供自动完成和参数类型推断功能
```



演示一个 `GET` 请求

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
  .then(function () {
    // 总是会执行（详情请了解Promise相关知识）
  });

// 上述请求也可以按以下方式完成（可选）
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  })
  .then(function () {
    // 总是会执行
  });  

// 想要使用async/await用法吗? 只需要添加async关键字到你的方法上
async function getUser() {
  try {
    const response = await axios.get('/user?ID=12345');
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
```

注意: 由于'async/await' 是ECMAScript 2017中的一部分，而且在IE和一些旧的浏览器中不支持，所以使用时务必要小心。



演示一个 `POST` 请求

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



演示多个并发请求

```js
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

Promise.all([getUserAccount(), getUserPermissions()])
  .then(function (results) {
    const acct = results[0];
    const perm = results[1];
  });
```



# axios API

可以向 `axios` 传递相关配置来创建请求

**axios(config)**

```js
// 发起一个 POST 请求
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});
// 在 node.js 用GET请求获取远程图片
axios({
  method: 'get',
  url: 'http://bit.ly/2mTM3nY',
  responseType: 'stream'
})
  .then(function (response) {
    response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
  });
```

**axios(url[, config])**

```js
// 发起一个 GET 请求 (默认请求方式)
axios('/user/12345');
```

## 请求方式的别名

为了方便起见，已经为所有常见的请求方式提供了别名。

**axios.request(config)**

**axios.get(url[, config])**

**axios.delete(url[, config])**

**axios.head(url[, config])**

**axios.options(url[, config])**

**axios.post(url[, data[, config]])**

**axios.put(url[, data[, config]])**

**axios.patch(url[, data[, config]])**

**注意**

在使用别名方法时， `url`、`method`、`data` 这些属性都不必在配置中指定。

# 并发（已过时）

请使用 `Promise.all` 方法来替代下面这些方法。

处理并发请求的辅助方法：

axios.all(iterator) 和 axios.spread(callback)

# 创建实例

你可以使用自定义配置新建一个实例。

**axios.create([config])**

```js
const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});
```

## **实例方法**

以下是可用的实例方法。指定的配置将与实例的配置合并。

**axios#request(config)**

**axios#get(url[, config])**

**axios#delete(url[, config])**

**axios#head(url[, config])**

**axios#options(url[, config])**

**axios#post(url[, data[, config]])**

**axios#put(url[, data[, config]])**

**axios#patch(url[, data[, config]])**

**axios#getUri([config])**



# 请求配置

这些是创建请求时可以用的配置选项。只有 `url` 是必需的。如果没有指定 `method`，请求将默认使用 'GET' 方法。

```js
{
  // 'url' 是用于请求的服务器 URL
  url: '/user',

  // 'method' 是创建请求时使用的方式
  method: 'get', // 默认值

  // 'baseURL' 将自动添加在 'url' 前面，除非 'url' 是一个绝对 URL
  // 为axios实例设置 'baseURL' 可以方便后续将相对 URL 传递给该实例的方法
  baseURL: 'https://some-domain.com/api/',

  // 'transformRequest' 允许在将请求数据发送给服务器前，修改请求数据
  // 它只适用于 'PUT', 'POST', 'PATCH' 和 'DELETE' 请求方式
  // 数组中最后一个函数必须返回一个字符串，或者一个关于Buffer, ArrayBuffer, FormData 或 Stream 的实例
  // 你可以修改请求头
  transformRequest: [function (data, headers) {
    // 对发送的 data 进行任意转换处理

    return data;
  }],

  // 'transformResponse' 允许在将响应数据传递给 then/catch 方法前，修改响应数据
  transformResponse: [function (data) {
    // 对接收的 data 进行任意转换处理

    return data;
  }],

  // 'headers' 可以自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // 'params' 是与请求一起发送的 URL 参数
  // 必须是一个简单对象或 URLSearchParams 对象
  params: {
    ID: 12345
  },

  // 'paramsSerializer' 是可选方法，主要用于序列化 'params'
  paramsSerializer: {
    indexes: null // 数组索引格式(null 代表无括号, false 代表空括号, true 表示带索引的括号)
  },

  // 'data' 是作为请求体被发送的数据
  // 仅适用 'PUT', 'POST', 'DELETE 和 'PATCH' 请求方式
  // 在没有设置 'transformRequest' 时，则必须是以下类型之一:
  // - string, 简单对象, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属: FormData, File, Blob
  // - Node 专属: Stream, Buffer
  data: {
    firstName: 'Fred'
  },
  
  // 发送请求体数据的可选语法
  // 请求方式 post
  // 只有 value 会被发送，key 则不会
  data: 'Country=Brasil&City=Belo Horizonte',

  // 'timeout' 指定请求超时的毫秒数
  // 如果请求时间超过 'timeout' 的值，则请求会被中断
  timeout: 1000, // 默认值是 '0' (永不超时)

  // 'withCredentials' 表示跨域请求时是否需要使用凭证(例如，cookie)
  withCredentials: false, // default

  // 'adapter' 允许自定义处理请求, 这使测试更加容易
  // 返回一个 promise 并提供一个有效的响应
  adapter: function (config) {
    /* ... */
  },

  // 'auth' 表示使用 HTTP Basic Auth 提供凭证
  // 这将设置一个 'Authorization' 请求头，它会覆盖 'headers' 中已存在的自定义 'Authorization' 请求头
  // 请注意，只有HTTP Basic Auth可以通过此参数配置
  // 对于Bearer tokens等令牌，请使用自定义 'Authorization' 中信息来替代
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // 'responseType' 表示服务器将要响应的数据类型
  // 选项包括: 'arraybuffer', 'document', 'json', 'text', 'stream'
  // 浏览器专属：'blob'
  responseType: 'json', // 默认值

  // 'responseEncoding' 表示用于解码响应的编码 (Node.js 专属)
  // 注意：忽略 'responseType' 为 'stream' 的响应或客户端请求
  responseEncoding: 'utf8', // 默认值

  // 'xsrfCookieName' 是 xsrf token 的值，被用作 cookie 的名称
  xsrfCookieName: 'XSRF-TOKEN', // 默认值

  // 'xsrfHeaderName' 是带有 xsrf token 值的 http 请求头名称
  xsrfHeaderName: 'X-XSRF-TOKEN', // 默认值

  // 'onUploadProgress' 允许处理上传事件
  // 浏览器专属
  onUploadProgress: function (progressEvent) {
    // 处理原生进度事件
  },

  // 'onDownloadProgress' 允许处理下载事件
  // 浏览器专属
  onDownloadProgress: function (progressEvent) {
    // 处理原生进度事件
  },

  // 'maxContentLength' 定义了node.js中允许的HTTP响应内容的最大字节数
  maxContentLength: 2000,

  // 'maxBodyLength' 定义了node.js中允许的http请求内容的最大字节数
  maxBodyLength: 2000,

  // 'validateStatus' 定义了对于给定的HTTP状态码是 resolve promise 还是 reject promise。
  // 如果 'validateStatus' 返回 'true' (或者设置为 'null' 或 'undefined'),
  // 则 Promise 将会是 resolved; 否则，会是 rejected
  validateStatus: function (status) {
    return status >= 200 && status < 300; // 默认值
  },

  // 'maxRedirects' 定义了在node.js中要遵循的最大重定向数
  // 如果设置为0，则不会进行重定向
  maxRedirects: 5, // 默认值
  
  // 'beforeRedirect' 是在重定向前要执行的函数
  // 可以使用它在重定向前调整请求选项, 检查最新的响应头, 或者通过抛出错误来取消请求
  // 如果 'maxRedirects' 被设置为0, 'beforeRedirect' 将不会被使用
  beforeRedirect: (options, { headers }) => {
    if (options.hostname === "example.com") {
      options.auth = "user:password";
    }
  },

  // 'socketPath' 定义了在node.js中使用的UNIX套接字
  // 例如 '/var/run/docker.sock' 发送请求到 docker 守护进程
  // 只能指定 'socketPath' 或 'proxy' 
  // 若都指定，将使用 'socketPath' 
  socketPath: null, // default

  // 'httpAgent' 和 'httpsAgent' 分别定义了在node.js中执行http或https请求时要使用的自定义代理
  // 这允许添加在默认情况下未启用的选项，例如 'keepAlive'
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 'proxy' 定义了代理服务器的主机名, 端口和协议
  // 你可以使用常规的 'http_proxy' 和 'https_proxy' 环境变量来定义自己的代理
  // 如果你在代理配置中使用环境变量, 你还可以定义 'no_proxy' 环境变量来表示不应被代理的域,
  // 以逗号分隔的列表形式呈现
  // 使用 'false' 可以禁用代理功能, 同时环境变量也会被忽略
  // 'auth' 表示应使用HTTP Basic Auth连接到代理, 并且提供凭证
  // 这将设置一个 'Proxy-Authorization' 请求头，
  // 它会覆盖 'headers' 中已存在的自定义 'Proxy-Authorization' 请求头
  // 如果代理服务器使用 HTTPS，则必须设置 protocol 为'https'
  proxy: {
    protocol: 'https',
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // 'cancelToken' 指定可以用于取消请求的取消令牌
  cancelToken: new CancelToken(function (cancel) {
  }),

  // AbortController——另一种取消axios请求的方法
  signal: new AbortController().signal,
  
  // 'decompress' 表示是否应该自动的解压响应体 
  // 如果被设置为true, 将会从已经解压的响应对象中删除 'content-encoding' 响应头
  // 仅结点(XHR无法关闭解压)
  decompress: true // 默认值
    
  // 以下为新内容
    
  // 'insecureHTTPParser' 布尔值
  // Indicates where to use an insecure HTTP parser that accepts invalid HTTP headers.
  // This may allow interoperability with non-conformant HTTP implementations.
  // 应避免使用不安全的解析器
  insecureHTTPParser: undefined // 默认值
    
  transitional: {
    // silent JSON parsing mode
    // `true`  - ignore JSON parsing errors and set response.data to null if parsing failed (old behaviour)
    // `false` - throw SyntaxError if JSON parsing failed (Note: responseType must be set to 'json')
    silentJSONParsing: true, // default value for the current Axios version

    // try to parse the response string as JSON even if `responseType` is not 'json'
    forcedJSONParsing: true,
    
    // throw ETIMEDOUT error instead of generic ECONNABORTED on request timeouts
    clarifyTimeoutError: false,
  },

  env: {
    // The FormData class to be used to automatically serialize the payload into a FormData object
    FormData: window?.FormData || global?.FormData
  },

  formSerializer: {
      visitor: (value, key, path, helpers)=> {}; // custom visitor funaction to serrialize form values
      dots: boolean; // use dots instead of brackets format
      metaTokens: boolean; // keep special endings like {} in parameter key 
      indexes: boolean; // array indexes format null - no brackets, false - empty brackets, true - brackets with indexes
  }
}
```



# 响应结构

一个请求的响应包含以下信息：

- data
- status
- statusText
- headers
- config
- request

```js
{
  // `data` 由服务器提供的响应
  data: {},

  // `status` 来自服务器响应的 HTTP 状态码
  status: 200,

  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',

  // `headers` 是服务器响应头
  // 所有的 header 名称都是小写，而且可以使用方括号语法访问
  // 例如: `response.headers['content-type']`
  headers: {},

  // `config` 是 `axios` 请求的配置信息
  config: {},

  // `request` 是生成此响应的请求
  // 在node.js中它是最后一个ClientRequest实例 (in redirects)，
  // 在浏览器中则是 XMLHttpRequest 实例
  request: {}
}
```

当使用 `then` 时，您将接收如下响应:

```js
axios.get('/user/12345')
  .then(function (response) {
    console.log(response.data);
    console.log(response.status);
    console.log(response.statusText);
    console.log(response.headers);
    console.log(response.config);
  });

// 相同效果
axios.get('/user/12345')
  .then(function (response) {
    console.log(response['data']);
    console.log(response['status']);
    console.log(response['statusText']);
    console.log(response['headers']);
    console.log(response['config']);
    console.log(response['request']);
  });
```

当使用 `catch`，或者传递一个rejection callback作为 `then` 的第二个参数时，响应可以通过 `error` 对象被使用，正如下文中*错误处理*部分解释的那样。

> then() 最多需要两个参数，分别为：Promise的成功(resolved/fulfilled)和失败(rejected)情况的回调函数，即fulfillment_callback和rejection_callback。



# 默认配置

您可以指定默认配置，它将作用于每个请求。

**全局 axios 默认值**

```js
axios.defaults.baseURL = 'https://api.example.com';

// 重要提示: 如果该axios在多个域中使用, 则 AUTH_TOKEN 会被发送给所有的域
// 请参阅下面的示例，使用自定义默认值
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;

axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

**自定义实例默认值**

```js
// 创建实例时配置默认值
const instance = axios.create({
  baseURL: 'https://api.example.com'
});

// 创建实例后修改默认值
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
```

**配置的优先级**

配置将会按优先级进行合并。它的顺序是：首先在**lib/defaults/index.js**中找到的库默认值，然后是实例的 `defaults` 属性，最后是请求的 `config` 参数。后面的优先级要高于前面的。下面有一个例子。

```js
// 使用库提供的默认配置创建实例
// 此时超时配置的默认值是 `0`
const instance = axios.create();

// 重写库的超时默认值
// 现在，所有使用此实例的请求都将等待2.5秒，然后才会超时
instance.defaults.timeout = 2500;

// 重写此请求的超时时间，因为该请求需要很长时间
instance.get('/longRequest', {
  timeout: 5000
});
```



# 拦截器

在请求或响应被 `then` 或 `catch` 处理前拦截它们。

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 位于 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

如果你稍后需要移除拦截器，可以这样：

```js
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

你也可以移除所有关于请求或响应的拦截器。

```js
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
instance.interceptors.request.clear(); // 移除请求的拦截器
instance.interceptors.response.use(function () {/*...*/});
instance.interceptors.response.clear(); // 移除响应的拦截器
```

可以给自定义的 axios 实例添加拦截器。

```js
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/})
```

添加请求拦截器时，默认情况下假定它们是异步的。

当主线程被阻塞时，这可能会导致axios请求的执行延迟（在拦截器的后台创建了一个promise，并且您的请求被放在调用堆栈的底部）。

如果您的请求拦截器是同步的，您可以向options对象添加一个标志，该标志将告诉axios同步运行代码，并避免请求执行中的任何延迟。

```js
axios.interceptors.request.use(function (config) {
  config.headers.test = 'I am only a header!';
  return config;
}, null, { synchronous: true });
```

如果希望基于运行时检查执行特定的拦截器，可以向options对象添加runWhen函数。

当且仅当runWhen返回为false时，将不执行拦截器。

该函数将使用config对象调用（不要忘记，您也可以将自己的参数绑定给它）。

当您有一个只需要在特定时间运行的异步请求拦截器时，这非常方便。

```js
function onGetCall(config) {
  return config.method === 'get';
}
axios.interceptors.request.use(function (config) {
  config.headers.test = 'special get headers';
  return config;
}, null, { runWhen: onGetCall });
```

**多个拦截器**

Given you add multiple response interceptors and when the response was fulfilled

- then each interceptor is executed
- then they are executed in the order they were added
- then only the last interceptor's result is returned
- then every interceptor receives the result of its predecessor
- and when the fulfillment-interceptor throws
  - then the following fulfillment-interceptor is not called
  - then the following rejection-interceptor is called
  - once caught, another following fulfill-interceptor is called again (just like in a promise chain).

Read **the interceptor tests** for seeing all this in code.



# 错误处理

```js
axios.get('/user/12345')
  .catch(function (error) {
    if (error.response) {
      // 请求成功发出且服务器也响应了状态码，但状态代码超出了 2xx 的范围
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // 请求已经成功发起，但没有收到响应
      // `error.request` 在浏览器中是 XMLHttpRequest 的实例，
      // 而在node.js中是 http.ClientRequest 的实例
      console.log(error.request);
    } else {
      // 发送请求时出了点问题
      console.log('Error', error.message);
    }
    console.log(error.config);
  });
```

使用 `validateStatus` 配置选项，可以自定义应该抛出错误的HTTP状态码。

```js
axios.get('/user/12345', {
  validateStatus: function (status) {
    return status < 500; // 只处理状态码小于500的情况
  }
})
```

使用 `toJSON` 可以获取更多关于HTTP错误的信息。

```js
axios.get('/user/12345')
  .catch(function (error) {
    console.log(error.toJSON());
  });
```



# 取消请求

**AbortController**

从 `v0.22.0` 开始，Axios 支持以 fetch API 方式—— `AbortController`取消请求：

```js
const controller = new AbortController();

axios.get('/foo/bar', {
   signal: controller.signal
}).then(function(response) {
   //...
});
// 取消请求
controller.abort()
```

**CancelToken (已过时)**

您还可以使用 *cancel token* 取消一个请求。

Axios 的 cancel token API 是基于被撤销 **cancelable promises proposal**。

此 API 从 `v0.22.0` 开始已被弃用，不应在新项目中使用。

可以使用 `CancelToken.source` 工厂方法创建一个 cancel token ，如下所示：

```js
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios.get('/user/12345', {
  cancelToken: source.token
}).catch(function (thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
    // 处理错误
  }
});

axios.post('/user/12345', {
  name: 'new name'
}, {
  cancelToken: source.token
})

// 取消请求（message 参数是可选的）
source.cancel('Operation canceled by the user.');
```

也可以通过传递一个 executor 函数到 `CancelToken` 的构造函数来创建一个 cancel token：

```js
const CancelToken = axios.CancelToken;
let cancel;

axios.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    // executor 函数接收一个 cancel 函数作为参数
    cancel = c;
  })
});

// 取消请求
cancel();
```

> 注意: 您可以使用同一个 cancel token 或 abort controller 取消多个请求。如果取消令牌在启动axios请求时已被取消，则该请求将立即取消，而不会尝试发出真正的请求。

在过渡期间，您可以使用这两种取消 API，即使是针对同一个请求：

```js
const controller = new AbortController();

const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios.get('/user/12345', {
  cancelToken: source.token,
  signal: controller.signal
}).catch(function (thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
    // 处理错误
  }
});

axios.post('/user/12345', {
  name: 'new name'
}, {
  cancelToken: source.token
})

// 取消请求 (message 参数是可选的)
source.cancel('Operation canceled by the user.');
// 或
controller.abort(); // 不支持 message 参数
```



# 请求体编码

## 使用`application/x-www-form-urlencoded`格式

**URLSearchParams**

默认情况下，axios将 JavaScript 对象序列化为 `JSON` 。 要以`application/x-www-form-urlencoded`格式发送数据，您可以使用`URLSearchParams`API，此API被大多数主流浏览器和v10版本的Node.js支持。

```js
const params = new URLSearchParams({ foo: 'bar' });
params.append('extraparam', 'value');
axios.post('/foo', params);

//
const url = require('url');
const params = new url.URLSearchParams({ foo: 'bar' });
params.append('extraparam', 'value');
axios.post('/foo', params.toString());
```

**Query String**

为了兼容旧版本的浏览器，可以使用polyfill（确保polyfill 全局环境）。

或者， 您可以使用[`qs`](https://github.com/ljharb/qs) 库编码数据:

```js
const qs = require('qs');
axios.post('/foo', qs.stringify({ 'bar': 123 }));
```

或者，用另一种方式 (ES6),

```js
import qs from 'qs';
const data = { 'bar': 123 };
const options = {
  method: 'POST',
  headers: { 'content-type': 'application/x-www-form-urlencoded' },
  data: qs.stringify(data),
  url,
};
axios(options);
```

**旧版本Node.js**

在旧版本 Node.js 引擎中， 可以使用 `querystring`模块，如下所示:

```js
const querystring = require('querystring');
axios.post('http://something.com/', querystring.stringify({ foo: 'bar' }));
```

您也可以使用 `qs` 库。

注意：如果需要对嵌套对象进行字符串化处理，则最好使用 `qs` 库，因为 querystring 方法在嵌套对象字符串化处理时存在问题。

**🆕 Automatic serialization to URLSearchParams**

如果'content-type'请求头被设为'application/x-www-form-urlencoded'，axios会自动地将数据对象序列化为urlencoded格式。

```js
const data = {
  x: 1,
  arr: [1, 2, 3],
  arr2: [1, [2], 3],
  users: [{name: 'Peter', surname: 'Griffin'}, {name: 'Thomas', surname: 'Anderson'}],
};

await axios.postForm('https://postman-echo.com/post', data,
  {headers: {'content-type': 'application/x-www-form-urlencoded'}}
);
```

服务器将其处理为：

```
  {
    x: '1',
    'arr[]': [ '1', '2', '3' ],
    'arr2[0]': '1',
    'arr2[1][0]': '2',
    'arr2[2]': '3',
    'arr3[]': [ '1', '2', '3' ],
    'users[0][name]': 'Peter',
    'users[0][surname]': 'griffin',
    'users[1][name]': 'Thomas',
    'users[1][surname]': 'Anderson'
  }
```

If your backend body-parser (like `body-parser` of `express.js`) supports nested objects decoding, you will get the same object on the server-side automatically

```
  var app = express();
  
  app.use(bodyParser.urlencoded({ extended: true })); // support encoded bodies
  
  app.post('/', function (req, res, next) {
     // echo body as JSON
     res.send(JSON.stringify(req.body));
  });

  server = app.listen(3000);
```

## 使用`multipart/form-data`格式

**Form data**

To send the data as a `multipart/formdata` you need to pass a formData instance as a payload. Setting the `Content-Type` header is not required as Axios guesses it based on the payload type.

```js
const formData = new FormData();
formData.append('foo', 'bar');

axios.post('https://httpbin.org/post', formData);
```

In node.js, you can use the [`form-data`](https://github.com/form-data/form-data) library as follows:

```js
const FormData = require('form-data');
 
const form = new FormData();
form.append('my_field', 'my value');
form.append('my_buffer', new Buffer(10));
form.append('my_file', fs.createReadStream('/foo/bar.jpg'));

axios.post('https://example.com', form)
```

**🆕 Automatic serialization to FormData**

Starting from `v0.27.0`, Axios supports automatic object serialization to a FormData object if the request `Content-Type` header is set to `multipart/form-data`.

The following request will submit the data in a FormData format (Browser & Node.js):

```js
import axios from 'axios';

axios.post('https://httpbin.org/post', {x: 1}, {
  headers: {
    'Content-Type': 'multipart/form-data'
  }
}).then(({data})=> console.log(data));
```

In the `node.js` build, the ([`form-data`](https://github.com/form-data/form-data)) polyfill is used by default.

You can overload the FormData class by setting the `env.FormData` config variable, but you probably won't need it in most cases:

```js
const axios= require('axios');
var FormData = require('form-data');

axios.post('https://httpbin.org/post', {x: 1, buf: new Buffer(10)}, {
  headers: {
    'Content-Type': 'multipart/form-data'
  }
}).then(({data})=> console.log(data));
```

Axios FormData serializer supports some special endings to perform the following operations:

- `{}` - serialize the value with JSON.stringify
- `[]` - unwrap the array-like object as separate fields with the same key

> NOTE: unwrap/expand operation will be used by default on arrays and FileList objects

FormData serializer supports additional options via `config.formSerializer: object` property to handle rare cases:

- `visitor: Function` - user-defined visitor function that will be called recursively to serialize the data object to a `FormData` object by following custom rules.
- `dots: boolean = false` - use dot notation instead of brackets to serialize arrays and objects;
- `metaTokens: boolean = true` - add the special ending (e.g `user{}: '{"name": "John"}'`) in the FormData key. The back-end body-parser could potentially use this meta-information to automatically parse the value as JSON.
- `indexes: null|false|true = false` - controls how indexes will be added to unwrapped keys of `flat` array-like objects
  - `null` - don't add brackets (`arr: 1`, `arr: 2`, `arr: 3`)
  - `false`(default) - add empty brackets (`arr[]: 1`, `arr[]: 2`, `arr[]: 3`)
  - `true` - add brackets with indexes (`arr[0]: 1`, `arr[1]: 2`, `arr[2]: 3`)

Let's say we have an object like this one:

```js
const obj = {
  x: 1,
  arr: [1, 2, 3],
  arr2: [1, [2], 3],
  users: [{name: 'Peter', surname: 'Griffin'}, {name: 'Thomas', surname: 'Anderson'}],
  'obj2{}': [{x:1}]
};
```

The following steps will be executed by the Axios serializer internally:

```js
const formData= new FormData();
formData.append('x', '1');
formData.append('arr[]', '1');
formData.append('arr[]', '2');
formData.append('arr[]', '3');
formData.append('arr2[0]', '1');
formData.append('arr2[1][0]', '2');
formData.append('arr2[2]', '3');
formData.append('users[0][name]', 'Peter');
formData.append('users[0][surname]', 'Griffin');
formData.append('users[1][name]', 'Thomas');
formData.append('users[1][surname]', 'Anderson');
formData.append('obj2{}', '[{"x":1}]');
```

Axios supports the following shortcut methods: `postForm`, `putForm`, `patchForm` which are just the corresponding http methods with the `Content-Type` header preset to `multipart/form-data`.

# 文件传输

You can easily sumbit a single file

```
await axios.postForm('https://httpbin.org/post', {
  'myVar' : 'foo',
  'file': document.querySelector('#fileInput').files[0] 
});
```

or multiple files as `multipart/form-data`.

```
await axios.postForm('https://httpbin.org/post', {
  'files[]': document.querySelector('#fileInput').files 
});
```

`FileList` object can be passed directly:

```
await axios.postForm('https://httpbin.org/post', document.querySelector('#fileInput').files)
```

All files will be sent with the same field names: `files[]`.

# 🆕 HTML Form Posting (browser)

Pass HTML Form element as a payload to submit it as `multipart/form-data` content.

```
await axios.postForm('https://httpbin.org/post', document.querySelector('#htmlForm'));
```

`FormData` and `HTMLForm` objects can also be posted as `JSON` by explicitly setting the `Content-Type` header to `application/json`:

```
await axios.post('https://httpbin.org/post', document.querySelector('#htmlForm'), {
  headers: {
    'Content-Type': 'application/json'
  }
})
```

For example, the Form

```
<form id="form">
  <input type="text" name="foo" value="1">
  <input type="text" name="deep.prop" value="2">
  <input type="text" name="deep prop spaced" value="3">
  <input type="text" name="baz" value="4">
  <input type="text" name="baz" value="5">

  <select name="user.age">
    <option value="value1">Value 1</option>
    <option value="value2" selected>Value 2</option>
    <option value="value3">Value 3</option>
  </select>

  <input type="submit" value="Save">
</form>
```

will be submitted as the following JSON object:

```
{
  "foo": "1",
  "deep": {
    "prop": {
      "spaced": "3"
    }
  },
  "baz": [
    "4",
    "5"
  ],
  "user": {
    "age": "value2"
  }
}
```

Sending `Blobs`/`Files` as JSON (`base64`) is not currently supported.