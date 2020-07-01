 如果是使用 `@Vue/cli` v3 创建的项目，可以在根目录下创建 `.env.*` 文件 来定义一些环境变量。比如 `API_HOST` 注意需要以 `VUE_APP_` 开始命令，可以参考[环境变量和模式](https://cli.vuejs.org/zh/guide/mode-and-env.html#模式) 如果是 `v2` 版本，可以修改 `/config/*.env.js` 文件实现类似的需求 数据请求可以创建多个 axios 示例，把实例导出，使用实例的方法去请求数据。 可以参考 [axios 实例](https://github.com/axios/axios#creating-an-instance) 这样两个数据请求库封装的时候使用不同的 `baseUrl` 





## 核心内容

必须以VUE_APP_开头