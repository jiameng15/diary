在家学习使用create-react-app,使用 `npx create-react-app my-app` 下载超级慢,看了一下npm的源为原始,所以切换至淘宝源

使用命令查看npm当前使用的库

```node
npm config get registry
// https://registry.npmjs.org/  原始的npm源
```

切换淘宝镜像库

```node
npm config set registry https://registry.npm.taobao.org

// 原始源
npm config set registry https://registry.npmjs.org/
```

创建项目

```
npx create-react-app my-app
// npx comes with npm 5.2+，可以直接使用 npx create-react-app
// 也可以使用 npm init react-app my-app       // 需要 npm 6+
// 或可以使用 yarn create react-app my-app  //在 Yarn 0.25+ 中可用
cd my-app 
yarn start // 启动项目
```

然而事情并没有这么简单

使用了淘宝镜像库后,创建CRA项目时,因为镜像库的镜像包版本落后,创建项目失败,报文如下:

```
A template was not provided. This is likely because you're using an outdated version of create-react-app.
Please note that global installs of create-react-app are no longer supported.
```

将npm源重置为原始后,方可创建CRA项目

