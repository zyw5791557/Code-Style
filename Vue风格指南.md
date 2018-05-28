## Vue 风格指南

> 新脚手架结构 [查看](https://www.github.com/zyw5791557/vue-starter)



### 脚手架配置目录

> vue.config.js

```javascript
module.exports = {
	// 项目部署的基础路径
	// 我们默认假设你的应用将会部署在域名的根部，
	// 比如 https://www.my-app.com/
	// 如果你的应用时部署在一个子路径下，那么你需要在这里
	// 指定子路径。比如，如果你的应用部署在
	// https://www.foobar.com/my-app/
	// 那么将这个值改为 `/my-app/`
	baseUrl: '/',
  
	// 将构建好的文件输出到哪里
	outputDir: 'dist',
  
	// 是否在保存的时候使用 `eslint-loader` 进行检查。
	// 有效的值：`ture` | `false` | `"error"`
	// 当设置为 `"error"` 时，检查出的错误会触发编译失败。
	lintOnSave: true,
  
	// 使用带有浏览器内编译器的完整构建版本
	// 查阅 https://cn.vuejs.org/v2/guide/installation.html#运行时-编译器-vs-只包含运行时
	compiler: false,
  
	// 调整内部的 webpack 配置。
	// 查阅 https://github.com/vuejs/vue-doc-zh-cn/vue-cli/webpack.md
	chainWebpack: () => {},
	configureWebpack: () => {},
  
	// vue-loader 选项
	// 查阅 https://vue-loader.vuejs.org/zh-cn/options.html
	vueLoader: {},
  
	// 是否为生产环境构建生成 source map？
	productionSourceMap: true,
  
	// CSS 相关选项
	css: {
		// 将组件内的 CSS 提取到一个单独的 CSS 文件 (只用在生产环境中)
		extract: true,
	
		// 是否开启 CSS source map？
		sourceMap: false,
	
		// 为预处理器的 loader 传递自定义选项。比如传递给
		// sass-loader 时，使用 `{ sass: { ... } }`。
		loaderOptions: {},
	
		// 为所有的 CSS 及其预处理文件开启 CSS Modules。
		// 这个选项不会影响 `*.vue` 文件。
		modules: false
	},
  
	// 在生产环境下为 Babel 和 TypeScript 使用 `thread-loader`
	// 在多核机器下会默认开启。
	parallel: require('os').cpus().length > 1,
  
	// 是否使用 `autoDLLPlugin` 分割供应的包？
	// 也可以是一个在 DLL 包中引入的依赖的显性的数组。
	// 查阅 https://github.com/vuejs/vue-doc-zh-cn/vue-cli/cli-service.md#dll-模式
	dll: false,
  
	// PWA 插件的选项。
	// 查阅 https://github.com/vuejs/vue-doc-zh-cn/vue-cli-plugin-pwa/README.md
	pwa: {},
  
	// 配置 webpack-dev-server 行为。
	devServer: {
		open: process.platform === 'darwin',
		host: '0.0.0.0',
		port: 8080,
		https: false,
		hotOnly: false,
		// 查阅 https://github.com/vuejs/vue-doc-zh-cn/vue-cli/cli-service.md#配置代理
		proxy: {
			'/dev': {
				target: 'http://dev-wm.shaozi.com',
				changeOrigin:true,
				pathRewrite: {
					'^/dev': ''
				}
			}
		}, // string | Object
		before: app => {
			// `app` 是一个 express 实例
		}
	},
  
	// 三方插件的选项
	pluginOptions: {
	  	// ...
	}
}
```





### src 目录结构

```
    |-- [api]                   // api
    |-- [assets]                // assets
    |-- [base]                  // 基础组件
    |-- [components]            // 业务组件
    |-- [config]                // Config
    |-- [router]                // 路由
    |-- [store]                 // 全局状态管理
    |-- [styles]                // Style
    |-- [utils]                 // 工具函数
    |-- [views]                 // 页面组件
    |-- App.vue                 // 根组件
    |-- main.js                 // 入口文件，实例化根节点
    ...
```



### 项目目录注意事项

- public 和 assets 的区别
  - public 存放不经常变动的静态资源
  - assets 存放经常变动静态资源
- src 中的 config 只允许存放开发和部署上线等同的配置
- 开发和上线不等同的配置应放到根目录下的 .env.development 和 .env.prooduction 下。



### 请求代理

> 根目录下的 config 文件夹，建立一个 proxyConfig.js ，配置代理，在当前目录 index.js 中引入配置文件并重启项目。

```javascript
module.exports = {
    devServer: {
        proxy: {
			'/dev': {
				target: 'http://dev-wm.shaozi.com',
				changeOrigin:true,
				pathRewrite: {
					'^/dev': ''
				}
			}
		}
    }
}
```





### 单页面文件模板

> 统一采用此顺序

```vue
<script>
export default {
    name: '',
    data () {
        return {

        }
    }
}
</script>

<template>
    <div>

    </div>
</template>

<style lang="scss" scoped>

</style>

```



### 组件

1. [组件的 data 必须是一个函数，根实例除外](https://cn.vuejs.org/v2/style-guide/#%E7%BB%84%E4%BB%B6%E6%95%B0%E6%8D%AE-%E5%BF%85%E8%A6%81)。

2. [props 定义的尽量详细，至少需要指定其类型](https://cn.vuejs.org/v2/style-guide/#Prop-%E5%AE%9A%E4%B9%89-%E5%BF%85%E8%A6%81)。

3. [v-for 必须配合 key 使用](https://cn.vuejs.org/v2/style-guide/#%E4%B8%BA-v-for-%E8%AE%BE%E7%BD%AE%E9%94%AE%E5%80%BC-%E5%BF%85%E8%A6%81)。

4. [v-for 和 v-if 不要在同一个文档中出现](https://cn.vuejs.org/v2/style-guide/#%E9%81%BF%E5%85%8D-v-if-%E5%92%8C-v-for-%E7%94%A8%E5%9C%A8%E4%B8%80%E8%B5%B7-%E5%BF%85%E8%A6%81)。

5. [为组件样式设置作用域，推荐采用 scoped ](https://cn.vuejs.org/v2/style-guide/#%E4%B8%BA%E7%BB%84%E4%BB%B6%E6%A0%B7%E5%BC%8F%E8%AE%BE%E7%BD%AE%E4%BD%9C%E7%94%A8%E5%9F%9F-%E5%BF%85%E8%A6%81)。

6. [在 Vue 的插件、混入等扩展中采用私有属性名](https://cn.vuejs.org/v2/style-guide/#%E7%A7%81%E6%9C%89%E5%B1%9E%E6%80%A7%E5%90%8D-%E5%BF%85%E8%A6%81)。

7. 单页面组件的文件名命名统一采用大驼峰法命名。

8. 组件必须指定 name ，采用横线大驼峰法命名，方便调试。

9. 引入的组件，命名也统一采用大驼峰法命名，多单词组合，避免单个单词。

10. 组件模板中复杂的表达式应采用计算属性或过滤器计算获得。

11. 组件内 methods 下的方法采用 XXX () {} 命名。

12. 指定统一采用缩写的形式，@ 代替 v-on , : 代替 v-bind。

13. 推荐使用 props 和事件进行父子组件之间的通讯，而不是使用 this.$parent。

14. 复杂的状态管理统一使用 Vuex，例如多组件共享一个状态。

15. 没有插槽的组件，引用的使用采用 ` <panel-item />`。

16. props 采用小驼峰法命名。

    ```js
    props: {
      greetingText: String
    }
    ```

    ```html
    <WelcomeMessage greeting-text="hi"/>
    ```

17. methods 里的方法采用小驼峰法命名。

    ​



### 组件/实例的选项的优先顺序



1. 全局感知
   - name
2. 模板依赖
   - components
   - directives
   - filters
3. 组合
   - extends
   - mixins
4. 接口
   - props
5. 本地状态
   - data
   - computed
6. 事件
   - watch
   - 生命周期钩子
7. 非响应式的属性
   - methods





### Vue 生命周期

- beforeCreate
- created
- beforeMount
- mounted
- beforeUpdate
- updated
- beforeDestory
- destoryed





### 其他

1. CSS 预处理器统一采用 SASS，采用 SCSS 书写。
2. 统一采用箭头函数替代常规函数，个别情况除外。
3. 前后端交互采用 Axios 的 HTTP 库。
4. 统一采用 let 和 const 代替 var 。
   1. 元素大于三个特性时，换行书写，便于阅读。

```vue
// demo
<el-form-item label="推送摘要">
    <el-input 
        :autosize="{
        	minRows: 6
        }"
        type="textarea" 
        placeholder="请输入摘要内容" 
        v-model="loadData.form.des">
    </el-input>
</el-form-item>
```

5. 文档注释采用 doc 形式。

```javascript
/**@data - 状态
 * loadData                     table 数据
 * 
 * @method - 方法
 * handleClick                  table 操作
 * paginationDataRequest        分页数据请求
 */
```