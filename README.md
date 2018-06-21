## Vue2.0 新手完全填坑攻略——从环境搭建到发布

https://github.com/xiaolaifeng/vueExample.github.io
目前项目前端框架存在很多问题，急需升级。对比react or vue  or angularJS, 历史变迁中考虑过仅使用模板引擎：Handlebars，
请参考https://www.cnblogs.com/Chen-XiaoJun/p/6246946.html
https://www.cnblogs.com/Zcqian/p/6843787.html
http://www.cnblogs.com/zzcit/p/6053240.html

https://cn.vuejs.org/v2/guide/syntax.html

### 什么是 Vue

是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。

教程：https://cn.vuejs.org/v2/guide/index.html
### 起步
1	使用淘宝 NPM 镜像
npm install -g cnpm --registry=https://registry.npm.taobao.org

大家都知道国内直接使用 npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。

淘宝 NPM 镜像是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

你可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:

2	命令行工具

全局安装 vue-cli
```markdown
$ cnpm install --global vue-cli
```

# 创建一个基于 webpack 模板的新项目
```markdown
$ vue init webpack inor-tempalte
```

3	进入项目，安装并运行：
```markdown
cd inor-tempalte
$ cnpm install
$ cnpm run dev
```

4	Vue.js 目录结构

目录解析
目录/文件 	说明
	build 		项目构建(webpack)相关代码
	config 		配置目录，包括端口号等。我们初学可以使用默认的。
	node_modules 	npm 加载的项目依赖模块
	src 	
				这里是我们要开发的目录，基本上要做的事情都在这个目录里。里面包含了几个目录及文件：
				
				    assets: 放置一些图片，如logo等。
				    components: 目录里面放了一个组件文件，可以不用。
				    App.vue: 项目入口文件，我们也可以直接将组件写这里，而不使用 components 目录。
				    main.js: 项目的核心文件。
	static 	静态资源目录，如图片、字体等。
	test 	初始测试目录，可删除
	.xxxx文件 	这些是一些配置文件，包括语法配置，git配置等。
	index.html 	首页入口文件，你可以添加一些 meta 信息或统计代码啥的。
	package.json 	项目配置文件。
	README.md 	项目的说明文档，markdown 格式
	
4.1	 在前面我们打开 APP.vue 文件，代码如下（解释在注释中）：

 导入组件
 ```markdown
import Hello from './components/Hello'
 
export default {
  name: 'app',
  components: {
    Hello
  }
}
 ```
 ### 实例多重组件嵌套
template-inner.html
```markdown
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>多重组件嵌套</title>
  <style>
    .container{
      width: 500px;height: 190px;margin: 100px auto;text-align: center;
      font-family: 微软雅黑;
    }
    .topBar{
      height: 40px;line-height: 40px;background-color: #4CD68E;
    }
    .header{
      height: 40px;background-color: #0c60ee;color: #fff;line-height: 40px;
    }
    .content{
      height: 50px;background-color: #2ac845;color: #333;line-height: 50px;
    }
    .footer{
      height: 60px;background-color: #30BD8A;color: yellow;line-height: 60px;
    }
  </style>
  <script src="https://cdn.bootcss.com/vue/2.4.2/vue.min.js"></script>

</head>
<body>
<div id="myApp">
  <app>
    <app-header></app-header>
    <app-content></app-content>
    <app-footer></app-footer>
  </app>
</div>
</body>
<script src="js/component.js"></script>
</html>
```
component.js
```markdown
(function () {
  Vue.component('app',{
    template:'<div class="container"><div class="topBar">这个地方在渲染后不会被占用</div><slot></slot></div>'
  });
  Vue.component('app-header',{
    template:'<div class="header" slot="header">this is heade11r</div>'
  });
  Vue.component('app-content',{
    template:'<div class="content">this is content11</div>'
  });
  Vue.component('app-footer',{
    template:'<div class="footer">this is footer11</div>'
  });
  var myApp=new Vue({
    el:'#myApp'
  });
})()
```

###  单前端开发+前后端联调+测试环境发布+生成环境发布
**单前端开发 需要使用mock模拟后端server服务，服务器请求地址为空，配置方式**
inor-tempalte/config/dev.env.js
```markdown
var merge = require('webpack-merge')
var prodEnv = require('./prod.env')
var testEnv = require('./test.env')


module.exports = merge(testEnv,prodEnv, {
  NODE_ENV: '"development"',
  API_ROOT: '""',
  IBI_API_ROOT: '"/app-ibi"',
})

```
前后端联调/测试环境发布
inor-tempalte/config/test.env.js
```markdown
module.exports = {
  NODE_ENV: '"test"',
  API_ROOT: '"http://oa1.indexvc.com:1000"',
  IBI_API_ROOT: '"http://oa1.indexvc.com:1000/app-ibi"',
}

```
### springboot +vue
将vue模板打包到dist的文件复制到springboot/src/main/resources下static与templates中
copy后文件目录如下
```markdown
src/main/resources
 static
 +static
 templates
 +index.html
```
```markdown

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### 什么是webpack 
1	起步
#初始化 npm，然后 在本地安装 webpack，接着安装 webpack-cli（此工具用于在命令行中运行 webpack）：
```markdown
npm init -y
npm install webpack webpack-cli --save-dev
```
#调整 package.json 文件，以便确保我们安装包是私有的(private)，并且移除 main 入口

2	创建一个 bundle 文件
	dist
  ```markdown
	$	npm install --save lodash	#要在 index.js 中打包 lodash 依赖，我们需要在本地安装 library：
	$	npx webpack	#会将我们的脚本作为入口起点，然后 输出 为 main.js

  ```
   

3	使用一个配置文件
	webpack.config.js
    ```markdown
	const path = require('path');

	module.exports = {
	  entry: './src/index.js',
	  output: {
	    filename: 'bundle.js',
	    path: path.resolve(__dirname, 'dist')
	  }
	};
  ```
	3.1	现在，让我们通过新配置文件再次执行构建：
   ```markdown
	npx webpack --config webpack.config.js
   ```


