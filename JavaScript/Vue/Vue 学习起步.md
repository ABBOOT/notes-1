## 准备
webpack 可以为我们的模块打包，预处理，热加载。而且 Vue 也为我们提供了 CSS 预处理，所以我们可以选择在 .vue 文件里写 LESS 或者 SASS 去代替原生 CSS。

虽然可以选择自己根据需要手动的从零开始搭建 Vue +  Webpack 开发环境，但是现在可以选择[`Vue-cli`](https://github.com/vuejs/vue-cli)来快速创建。

1. 首先，全局安装`vue-cli`(确保你有`node`和`npm`)：

```bash
npm install -g vue-cli
```

2. 然后使用 webpack 模板创建一个项目并且下载依赖：

```bash
vue init webpack Vue-Learn
```

执行这个命令之后，会有一些提示信息，根据提示信息填写或者选择，之后就会在 Vue-Learn 目录中生成一些基础的文件夹和相应的配置文件。

> vue-cli 可以根据指定的模板建立项目的基础配置。目前官方提供了 webpack 和 browserify 两种项目构建的模板。命令格式为`vue init <template-name> <project-name>`。

3. 安装项目需要的模块：
在第二步的过程中，vue-cli 会将项目构建和开发过程中需要用的模块都写入到了 package.json 文件中。需要我们手动安装一遍这些依赖：

```bash
npm install
```

4. 开发
上面的第二步中，在 package.json 文件的'script'配置中，设置了多个命令，分别对应不同的功能

- `dev`  开发时启动一个热加载的 webpack 服务器
- `build` 进行打包
- `unit`
- `e2e`
- `test`
- `lint`

所以我们就可以执行`npm run dev`来开启开发服务器。执行这个命令后，默认情况下，会在`localhost:8080`中看到一个简单的 Hello word 页面。执行这个命令后，webpack 是将打包编译的文件放在内存中的，而并没有输出到硬盘中，所以我们并不能在项目目录中找到编译后的文件。除非我们执行了`npm run build`来打包编译然后输出。


## 起步
上面的准备步骤中，已经将开发环境运行起来了。解下来，我们可以针对自己的需求进行一定的修改和增加。

参考：

1. [(1/2)Vue构建单页应用最佳实战](https://github.com/MeCKodo/vue-tutorial/tree/549659f091e5b7a67402a422e774af09f8db5ae4)
2. [vue.js学习笔记1 搭建vue.js本地开发框架](http://sugarball.me/vue-jsxue-xi-bi-ji-1-da-jia-vue-jskai-fa-kuang-jia/)

### 添加路由和 XHR 模块
为了开发单页应用(SPA)，我们需要前端也有路由，并进行 XHR 请求。对于此需求，Vue 分别提供了`vue-router`和`vue-resource`两个模块。下面需要将其安装并添加到项目依赖中：

`npm install --save vue-router vue-resource`

### 路由模块
制作 SPA 的时候，需要注意的是，链接需要改成`v-link`的形式，而不能使用 HTML 原生的`href`，否则就是直接跳转，而不是通过 vue-router 代理跳转。

### Resource 模块
1. 跨域访问

对于需要跨域的请求，需要在跨域的服务器上设置相关的 Access-Control-Allow-* 头信息：

```conf
# Nginx 设置
# 允许跨域访问的域名
add_header 'Access-Control-Allow-Origin' 'http://localhost:8080';
# 允许携带 Cookie 信息
add_header 'Access-Control-Allow-Credentials' 'true';
# 允许跨域使用的方法
add_header 'Access-Control-Allow-Methods' 'GET, POST, DELETE, PUT, PATCH, OPTIONS';
# 允许携带的头信息
add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
```

如果还需要跨域的时候传递 Cookie，那就需要后台设置之后，设置 Vue.htt.options.xhr 的信息：

```js
Vue.http.options.xhr = { withCredentials: true }
```

> [Cross-Origin Request - Github Issue](https://github.com/vuejs/vue-resource/issues/22)
> * Backend server should set Access-Control-Allow-Origin to `*` or `[ Frontend server domains whitelist ]`
> * `Vue.http.options.xhr = { withCredentials: true }` is not needed.
> * url must begin with `http(s)://`. e.g. `localhost:8080` would fail.

### 注意事项
* 注意如果 prop 是一个对象或数组，是按引用传递。在子组件内修改它会影响父组件的状态，不管是使用哪种绑定类型
* 针对同一个元素的后一个 watch 会覆盖前一个 watch，无论是不是 deep
* 自定义指令内部可以通过 this.vm.someKey 来访问到组件的数据
* 自定义指令名不要有大写，props 命名也不要有大写

