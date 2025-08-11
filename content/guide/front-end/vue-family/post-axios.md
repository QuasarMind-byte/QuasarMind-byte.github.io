---
title: "Axios基础知识"
---

Axios 是一个**基于 Promise 的、功能强大且流行的 HTTP 客户端库**，用于在浏览器和 Node.js 环境中发起 HTTP 请求。它因其简洁的 API、强大的功能和良好的兼容性而成为前端和后端开发的首选。

## 一、核心介绍 (What & Why Axios?)

1.  **核心特性:**
    *   **基于 Promise:** 使用 ES6 Promise API，避免了回调地狱，支持 `async/await` 语法，使异步代码更清晰易读。
    *   **浏览器 & Node.js 支持:** 同一套 API 在两个环境中无缝工作。
    *   **请求/响应拦截器:** 在请求发出前或响应返回后对其进行全局或局部的拦截、转换或添加逻辑（如添加认证头、处理错误）。
    *   **自动转换 JSON 数据:** 请求数据 (`data`) 可自动序列化为 JSON，响应数据 (`data`) 可自动解析为 JavaScript 对象。
    *   **取消请求:** 提供机制 (`CancelToken` 或较新的 `AbortController`) 来取消正在进行的请求。
    *   **客户端 XSRF 防护:** 内置支持防御跨站请求伪造 (XSRF/CSRF)。
    *   **并发请求:** 提供 `axios.all` 和 `axios.spread` (现在更常用 `Promise.all`) 辅助函数处理并发请求。
    *   **进度跟踪:** 在浏览器中支持上传和下载的进度监控。
    *   **广泛的浏览器兼容性:** 支持现代浏览器和较旧的浏览器（如 IE11）。
    *   **类型支持:** 提供良好的 TypeScript 类型定义。

2.  **主要优势:**
    *   **简洁直观的 API:** `axios.get(url)`, `axios.post(url, data)` 等方法命名清晰。
    *   **丰富的配置:** 提供全局默认配置 (`axios.defaults`) 和每个请求的特定配置。
    *   **强大的错误处理:** 统一捕获 HTTP 状态码错误和网络错误。
    *   **高度可扩展:** 通过拦截器和适配器可以灵活定制行为。
    *   **活跃的社区和生态:** 文档完善，社区支持好，与各种框架（Vue, React, Angular 等）集成方便。

3.  **与 `fetch()` 的比较:**
    *   **更简洁的 API:** `fetch()` 需要手动处理响应状态码、转换 JSON 等，Axios 封装得更好。
    *   **更完善的错误处理:** Axios 默认将非 2xx 状态码视为错误（可通过 `validateStatus` 配置），而 `fetch()` 只在网络故障时 reject Promise。
    *   **拦截器:** `fetch()` 原生不支持，需要额外封装。
    *   **取消请求:** Axios 提供内置机制，`fetch()` 需要使用 `AbortController`。
    *   **进度:** Axios 浏览器端内置支持，`fetch()` 需要监听 `ReadableStream`。
    *   **浏览器兼容性:** Axios 对旧浏览器的兼容性更好（尤其 IE11）。

## 二、基本使用 (How to Use)

1.  **安装:**
    ```bash
    npm install axios
    # 或
    yarn add axios
    # 或在浏览器中通过 CDN 引入
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    ```

2.  **发起请求 (核心方法):**
    *   **`axios(config)`** - 通用方法
        ```javascript
        axios({
          method: 'get', // 默认
          url: '/api/user/12345',
          params: { id: 12345 }, // URL 参数 (GET)
          data: { name: 'John' }, // 请求体数据 (POST, PUT, PATCH)
          headers: { 'X-Custom-Header': 'value' },
          timeout: 1000, // 超时时间 (毫秒)
          responseType: 'json', // 默认 (其他: 'arraybuffer', 'blob', 'document', 'text', 'stream')
        })
        .then(response => {
          console.log(response.data); // 服务器响应的数据 (自动转换)
          console.log(response.status); // HTTP 状态码 (200)
          console.log(response.statusText); // HTTP 状态信息 ('OK')
          console.log(response.headers); // 响应头信息
          console.log(response.config); // 请求的配置信息
        })
        .catch(error => {
          if (error.response) {
            // 请求发出且服务器响应了状态码 (非2xx)
            console.log(error.response.data);
            console.log(error.response.status);
            console.log(error.response.headers);
          } else if (error.request) {
            // 请求已发出但无响应 (网络错误、目标服务器无响应)
            console.log(error.request); // 在浏览器中是 XMLHttpRequest 实例
          } else {
            // 设置请求时出错或触发拦截器中的错误
            console.log('Error', error.message);
          }
          console.log(error.config);
        });
        ```
    *   **别名方法 (更常用):**
        ```javascript
        axios.get(url[, config])
        axios.delete(url[, config])
        axios.head(url[, config])
        axios.options(url[, config])
        axios.post(url[, data[, config]])
        axios.put(url[, data[, config]])
        axios.patch(url[, data[, config]])
        ```
        *示例 (GET):*
        ```javascript
        axios.get('/api/users', {
          params: { page: 2, limit: 10 }
        })
        .then(response => console.log(response.data))
        .catch(error => console.error(error));
        ```
        *示例 (POST):*
        ```javascript
        axios.post('/api/users', {
          firstName: 'Fred',
          lastName: 'Flintstone'
        })
        .then(response => console.log('User created:', response.data))
        .catch(error => console.error('Creation failed:', error));
        ```

3.  **创建实例 (推荐):**
    可以创建一个具有自定义默认配置的 Axios 实例，用于特定 API 端点。
    ```javascript
    const apiClient = axios.create({
      baseURL: 'https://api.yourdomain.com/v1', // 基础 URL
      timeout: 5000,
      headers: { 'Content-Type': 'application/json' }
    });

    // 使用实例
    apiClient.get('/users').then(...);
    apiClient.post('/posts', { title: 'Hello' }).then(...);
    ```

4.  **配置默认值:**
    *   **全局默认值:** 影响所有请求。
        ```javascript
        axios.defaults.baseURL = 'https://api.example.com';
        axios.defaults.headers.common['Authorization'] = AUTH_TOKEN; // 所有请求添加公共头
        axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'; // 特定方法头
        ```
    *   **实例默认值:** 在创建实例时设置 (如上例)。
    *   **请求级配置:** 优先级最高，在调用请求方法时传入的 `config` 对象中设置。

5.  **拦截器 (Interceptors):**
    在请求或响应被 `then` 或 `catch` 处理前拦截它们。
    *   **请求拦截器:**
        ```javascript
        axios.interceptors.request.use(
          config => {
            // 在发送请求前做些什么 (例如: 添加认证 token)
            config.headers.Authorization = `Bearer ${getAuthToken()}`;
            return config; // 必须返回 config 对象
          },
          error => {
            // 对请求错误做些什么 (通常返回一个 rejected 的 Promise)
            return Promise.reject(error);
          }
        );
        ```
    *   **响应拦截器:**
        ```javascript
        axios.interceptors.response.use(
          response => {
            // 对响应数据做点什么 (例如: 统一处理特定业务状态码)
            if (response.data.code !== 200) { // 假设业务状态码 200 表示成功
              return Promise.reject(new Error(response.data.message || '业务错误'));
            }
            return response.data; // 通常直接返回 response.data 简化后续处理
          },
          error => {
            // 对响应错误做点什么 (统一处理 HTTP 错误)
            if (error.response && error.response.status === 401) {
              // 处理未授权 (跳转登录等)
              router.push('/login');
            }
            // ... 处理其他状态码或网络错误
            return Promise.reject(error); // 继续传递错误
          }
        );
        ```
        *实例级拦截器:* 用法相同，但作用于创建的实例 (`apiClient.interceptors.request.use(...)`)。

6.  **取消请求 (Cancellation):**
    *   **使用 `AbortController` (推荐):**
        ```javascript
        const controller = new AbortController();

        axios.get('/api/slow', {
          signal: controller.signal // 将 signal 传入配置
        })
        .then(response => {...})
        .catch(error => {
          if (axios.isCancel(error)) {
            console.log('请求被取消:', error.message);
          } else {
            // 处理其他错误
          }
        });

        // 在需要取消请求的地方调用
        controller.abort('用户取消了操作');
        ```
    *   **使用 `CancelToken` (旧版 API，不推荐新项目):**
        ```javascript
        const source = axios.CancelToken.source();

        axios.get('/api/slow', {
          cancelToken: source.token
        }).then(...).catch(thrown => {
          if (axios.isCancel(thrown)) { ... }
        });

        // 取消请求
        source.cancel('操作被用户取消');
        ```

## 三、工程实践经验 (Best Practices & Tips)

1.  **封装 Axios 实例:**
    *   创建多个实例：针对不同的后端服务 (`const authApi = axios.create({ baseURL: '/auth' }); const dataApi = axios.create({ baseURL: '/data' });`)。
    *   在实例上设置通用配置 (`baseURL`, `timeout`, `Content-Type`, 认证头)。
    *   在实例上添加拦截器，实现服务或模块级别的统一逻辑（如错误处理、重试、日志）。

2.  **利用拦截器实现通用逻辑:**
    *   **请求拦截器:**
        *   自动添加认证 Token (`Authorization: Bearer ...`)。
        *   设置公共请求头 (`Accept-Language`, `X-Requested-With`)。
        *   添加请求时间戳或唯一标识符 (用于日志或追踪)。
        *   对请求数据进行特定序列化 (如 `qs.stringify` 处理表单)。
    *   **响应拦截器:**
        *   **统一错误处理:** 根据 HTTP 状态码或业务状态码进行全局错误提示、跳转登录页、记录日志等。**这是最重要的实践之一！**
        *   统一解析响应数据 (直接返回 `response.data.data` 或业务需要的格式)。
        *   处理特定的业务状态码逻辑。
        *   转换响应数据格式。
        *   实现全局的请求耗时监控或日志记录。

3.  **优雅的错误处理:**
    *   在响应拦截器中统一处理所有可能的错误 (`error.response`, `error.request`, `error.message`)。
    *   根据错误类型（网络错误、HTTP 错误、业务逻辑错误）进行不同的用户提示或操作。
    *   避免在每个请求的 `.catch` 中重复写相同的错误处理逻辑。
    *   使用自定义错误类（如 `HttpError`, `BusinessError`, `NetworkError`）包装原始错误，携带更多上下文信息。

4.  **处理文件上传/下载:**
    *   **上传:** 使用 `FormData` 和 `multipart/form-data` 类型。
        ```javascript
        const formData = new FormData();
        formData.append('file', fileInputElement.files[0]);
        formData.append('name', 'file_name');

        axios.post('/upload', formData, {
          headers: { 'Content-Type': 'multipart/form-data' }
        });
        ```
    *   **下载:**
        *   设置 `responseType: 'blob'`。
        *   处理响应，利用 `URL.createObjectURL` 创建链接供用户下载。
        ```javascript
        axios.get('/download/file.pdf', { responseType: 'blob' })
          .then(response => {
            const url = window.URL.createObjectURL(new Blob([response.data]));
            const link = document.createElement('a');
            link.href = url;
            link.setAttribute('download', 'filename.pdf');
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
          });
        ```

5.  **TypeScript 集成:**
    *   Axios 内置了良好的类型定义。
    *   为你的 API 响应定义接口 (`interface UserResponse { id: number; name: string; }`)。
    *   在使用 `axios.get/post` 等方法时指定响应数据类型：
        ```typescript
        interface User {
          id: number;
          name: string;
          email: string;
        }
        // 请求时指定响应数据类型
        axios.get<User>('/api/users/1')
          .then(response => {
            const user: User = response.data;
            console.log(user.name);
          });

        // 在拦截器中返回特定类型 (如果拦截器直接返回了 response.data)
        apiClient.interceptors.response.use(response => response.data as User);
        ```

6.  **处理并发请求:**
    *   使用 `Promise.all` (更现代和通用):
        ```javascript
        Promise.all([
          axios.get('/api/users'),
          axios.get('/api/posts')
        ])
        .then(([usersResponse, postsResponse]) => {
          console.log('Users:', usersResponse.data);
          console.log('Posts:', postsResponse.data);
        })
        .catch(error => console.error('One or more requests failed', error));
        ```
    *   (旧方式) 使用 `axios.all` + `axios.spread`:
        ```javascript
        axios.all([axios.get('/url1'), axios.get('/url2')])
          .then(axios.spread((resp1, resp2) => { ... }));
        ```

7.  **CSRF/XSRF 防护:**
    *   如果后端框架（如 Laravel, Django）使用基于 Cookie 的 CSRF 防护：
        *   确保 Axios 能发送 Cookie (`withCredentials: true`)。
        *   后端通常会在 Cookie 中设置一个 `XSRF-TOKEN`。
        *   Axios **默认会自动**在请求头中添加 `X-XSRF-TOKEN`（从 `xsrfCookieName` 指定的 Cookie 中读取值）。通常无需额外配置。
    *   检查 `axios.defaults.xsrfCookieName` (默认 `'XSRF-TOKEN'`) 和 `axios.defaults.xsrfHeaderName` (默认 `'X-XSRF-TOKEN'`) 是否与后端匹配。

8.  **性能与优化:**
    *   **取消不必要的请求:** 在组件卸载或用户导航离开时，使用 `AbortController` 取消未完成的请求，避免内存泄漏和无效更新。
    *   **节流/防抖:** 对于频繁触发的搜索等操作，在调用 Axios 请求前使用节流 (throttle) 或防抖 (debounce) 函数优化性能。
    *   **缓存:** 对于不常变化的数据，考虑在客户端实现简单的缓存策略（如内存缓存、localStorage），避免重复请求。注意缓存失效策略。

9.  **测试:**
    *   使用 Jest 等测试框架。
    *   利用 `axios-mock-adapter` 或 `msw` (Mock Service Worker) 等库轻松模拟 Axios 请求，测试不同响应场景（成功、失败、超时）下的组件或函数逻辑。
    *   测试拦截器的逻辑是否正确。

10. **安全注意事项:**
    *   永远不要在客户端存储敏感信息（如永久 Token 密钥）。
    *   使用 `withCredentials: true` 时，确保后端正确配置了 CORS (Access-Control-Allow-Origin, Access-Control-Allow-Credentials) 且来源可信。
    *   验证和清理从 API 接收的所有数据，防止 XSS 攻击。
    *   对用户输入进行编码或使用参数化查询防止构造恶意 URL 参数。

**总结:**

Axios 是一个强大、灵活且易于使用的 HTTP 客户端。掌握其核心 API 是基础，但在实际工程实践中，**通过创建实例、合理利用拦截器进行统一配置、错误处理和逻辑封装，以及遵循安全最佳实践**，才能真正发挥其价值，构建出健壮、可维护的网络请求层。将其与 TypeScript 结合使用能进一步提升开发体验和代码质量。