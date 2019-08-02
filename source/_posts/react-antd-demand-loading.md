---
title: create-react-app按需加载antd
date: 2019-04-08 15:07:42
tag: react
---

- 下载依赖包
```js
yarn add less less-loader -S
yarn add babel-plugin-import -D
```

- 配置package.json
```js
"plugins": [
      ["import", {
        "libraryName": "antd",
        "libraryDirectory": "es",
        "style": true
      }]
    ]
```
- 配置webpack.config.js
```js
// 1.增加less支持模块
const lessRegex = /\.less$/;
const lessModuleRegex = /\.module\.less$/;

// 2.照猫画虎，把sass相关的配置复制一份，改成less
{
    test: lessRegex,
    exclude: sassModuleRegex,
    use: getStyleLoaders(
       {
         importLoaders: 2,
         sourceMap: isEnvProduction && shouldUseSourceMap,
       },
       'less-loader'
     ),
     sideEffects: true,
 },
{
     test: lessModuleRegex,
     use: getStyleLoaders(
       {
         importLoaders: 2,
         sourceMap: isEnvProduction && shouldUseSourceMap,
         modules: true,
         getLocalIdent: getCSSModuleLocalIdent,
       },
       'less-loader'
   ),
 },

// 3.修改一下less的preProcessor配置项，加入antd配置
if (preProcessor) {
	let loader = {
	    loader: require.resolve(preProcessor),
	    options: {
	      sourceMap: isEnvProduction && shouldUseSourceMap,
	    }
	  }

	if (preProcessor === "less-loader") {
	 loader.options.modifyVars = {
	   'primary-color': 'green',
	   'link-color': '#1DA57A',
	   'border-radius-base': '2px',
	 }
	 loader.options.javascriptEnabled = true
	}
	loaders.push(loader);
}
```

