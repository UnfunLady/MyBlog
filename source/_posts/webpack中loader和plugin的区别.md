---
title: "webpack中loader和plugin的区别"
catalog: true
date: 2022-09-21 09:43:42
subtitle: "学习经验."
header-img: 
tags:
- webpack
catagories:
- webpack
---

### 笔记描述
> loader和plugins的区别是很么
### loader
>loader从字面的意思理解，是 加载 的意思。
由于webpack 本身只能打包commonjs规范的js文件，所以，针对css，图片等格式的文件没法打包，就需要引入第三方的模块进行打包。
loader虽然是扩展了 webpack ，但是它只专注于转化文件（transform）这一个领域，完成压缩，打包，语言翻译。
loader是运行在NodeJS中。仅仅只是为了打包

loader描述了webpack如何处理非javascript模块，并且在build中引入这些依赖。
loader可以将文件从不同的语言（如TypeScript）转换为JavaScript，或者将内联图像转换为data URL。比如说：CSS-Loader，Style-Loader等。

### plugin
>Plugin 是扩展 Webpack 功能的一种方法，它们可以在整个构建过程中的不同阶段执行自定义操作。与 loader 不同，
plugin 不直接作用于特定文件类型，而是用于解决构建过程中更广泛的任务，如代码优化、资源管理和环境变量注入等。
简单来说，plugin 主要负责在构建过程中执行广泛的任务和自定义操作。
例如，使用 HtmlWebpackPlugin 自动生成 HTML 文件，或使用 UglifyJsPlugin 压缩 JavaScript 代码。

### 总结
* Loader 主要用于针对单个文件执行转换操作，它们在模块加载之前执行。
* Plugin 主要用于执行构建过程中的广泛任务和自定义操作，可以在构建过程中的不同阶段工作。
* Loader 是在 Webpack 配置文件中的 module 对象下的 rules 数组中定义的，而 plugin 是在 plugins 数组中定义的。


---



