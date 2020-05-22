# vue-define-swiper
使用vue.js手动封装一个轮播图插件，插件实现效果：点击左右按钮进行轮播切换，可自行选择显示或隐藏导航条以及切换按钮，图片宽高比例自适应。

###参数说明
```
interval   滚动时间间隔
animDuration   滚动切换延时
moveRatio   滚动翻页比例
showPagination  显示导航条
showChangeBtn   显示切换按钮
```
以上参数都可以在引用组件时通过参数传递覆盖默认参数

### 方法说明
```$xslt
定时器：startTimer，stopTimer
图片滚动到指定位置：imgScroll
检验滚动正确位置：checkPosition
设置图片滚动位置：setTransform
操作DOM元素实现滚动：handlerDOM
移动端的拖动事件：touchStart，touchMove，touchEnd
按钮切换翻页事件：previous，next，changeItem
```
## 预览地址
```$xslt
http://www.codertian.top/demo/vue-define-swiper/index.html
```
##[前往预览](http://www.codertian.top/demo/vue-define-swiper/index.html)

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
