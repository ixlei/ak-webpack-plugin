# ak-webpack-plugin

AlloyKit平台生成离线包命令


## 安装

```
npm i --save ak-webpack-plugin

npm i --save-dev ak-webpack-plugin
```

##  使用

``` javascript
var AkWebpackPlugin = require('ak-webpack-plugin');

// 通用配置，webserver 针对 html 文件，而 cdn 是针对 cdn 文件
plugins: [
	new AkWebpackPlugin({
	    "zipFileName": "dist/offline", 
	    // String, 最终生成的离线包名称，默认值是 `offline`，**当前文件夹位置以命令执行位置为基准**
	    "src": "dist",
	    // String, 生成环境的代码源，默认值 `dist`
	    "map": [
	        {
	            "src": "webserver",
	            "url": "//localhost:9000/"
	        },
	        {
	            "src": "cdn",
	            "url": "//localhost:8000/"
	        }
	    ]
	    // 具体的文件目录及cdn映射
	})
]

```

如果你使用上述的配置，它会在 `dist` 目录下，生成 `offline` 文件夹和 `offline.zip` 文件：

``` javascript
-- dist
	|
	|- webserver
	|- cdn
	|- offline
	|- offline.zip
```

``` javascript

// 多个cdn文件配置
plugins: [
	new AkWebpackPlugin({
	    "zipFileName": "offline",
        "src": "dist",
        "map": [
        {
            "src": "cdn/js",
            "dest": "js",
            // String, 目标文件路径子文件夹
            "isSameOrigin": true, 
            // Boolean， 默认 false，如果为 true， 则会将 cdn 的 url替换成与 isWebserver 为 true 的 cdn url
            "url": "s1.url.cn/huayang/"
        },
        {
            "src": "cdn/css",
            "dest": "css",
            "url": "s2.url.cn/huayang/"
        },
        {
            "src": "cdn/img",
            "dest": "img",
            "url": "s3.url.cn/huayang/"
        },
        {
            "src": "cdn/lib",
            "dest": "lib",
            "url": "s3.url.cn/huayang/"
        },
        {
            "src": "webserver",
            "isWebserver": true,
            // Boolean， 默认为 false，如果为 true，则这将告诉插件这是 html 的主要 cdn url 
            "url": "huayang.qq.com/huayang/activity/"
        }
	})
]

```

之所以要用 `isSameOrigin` 与 `isWebserver`，是有时候需要 `html` 文件和 `js` 文件的域名一致，例如有时候需要收集js的报错，让两者的 `cdn` 一致会更方便收集到具体的报错信息。


## 测试
```
npm run test
```

## 变更
* v1.0.0 离线包打包及 `webserver` 替换 `cdn` 的 `url`
* v1.0.1 更换成中文文档
* v1.1.0 配置更改并修复同域js文件位置错误问题