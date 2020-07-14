---
title: 新建一个vue工程
date: 2020-07-10 17:03:15
tags: vue
---

1.npm install -g @vue/cli
2.快速原型开发 npm install -g @vue/cli-service-global 仅需一个App.vue,vue serve可快速浏览效果
3.使用旧版 npm install -g @vue/cli-init 
4.vue init webpack-simple element-ui[这里是文件夹名称,会自动创建],这样完成了基础的vue项目创建
5.安装element-ui
npm i element-ui -Source
6.引入
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

Vue.use(ElementUI);

7.启动报错 ERROR in ./node_modules/element-ui/lib/theme-chalk/fonts/element-icons.ttf
Module parse failed: Unexpected character ' ' (1:0)
You may need an appropriate loader to handle this file type.
(Source code omitted for this binary file)
 @ ./node_modules/css-loader!./node_modules/element-ui/lib/theme-chalk/index.css 7:894-930
 @ ./node_modules/element-ui/lib/theme-chalk/index.css
 @ ./src/main.js
 @ multi (webpack)-dev-server/client?http://localhost:8080 webpack/hot/dev-server ./src/main.js

ERROR in ./node_modules/element-ui/lib/theme-chalk/fonts/element-icons.woff
Module parse failed: Unexpected character ' ' (1:4)
You may need an appropriate loader to handle this file type.
(Source code omitted for this binary file)
 @ ./node_modules/css-loader!./node_modules/element-ui/lib/theme-chalk/index.css 7:818-855
 @ ./node_modules/element-ui/lib/theme-chalk/index.css
 @ ./src/main.js
 @ multi (webpack)-dev-server/client?http://localhost:8080 webpack/hot/dev-server ./src/main.js
 
安装 file-loader npm install --save-dev file-loader,参考https://segmentfault.com/q/1010000009855168/
编辑 webpack.config.js,在 file-loader的test里加入 ttf和woff
