在家学习使用create-react-app,使用 `npx create-react-app my-app` 下载超级慢,看了一下npm的源为原始,所以切换至淘宝源

使用命令查看npm当前使用的库

```node
npm config get registry
// https://registry.npmjs.org/  原始的npm源
```

切换淘宝镜像库

```node
npm config set registry https://registry.npm.taobao.org
```

创建项目

```
npx create-react-app my-app
// npx comes with npm 5.2+，可以直接使用 npx create-react-app
// 也可以使用 npm init react-app my-app         --需要 npm 6+

cd my-app 
npm start // 启动项目
```

