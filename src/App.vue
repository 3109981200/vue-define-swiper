<template>
  <div id="app">
    <h2>使用vue.js手动封装一个轮播图插件</h2>
    <p>使用vue.js手动封装一个轮播图插件，小巧方便不依赖第三方库，纯vue实现，插件实现效果：点击左右按钮进行轮播切换，可自行选择显示或隐藏导航条以及切换按钮，图片宽高比例自适应。</p>
    <Swiper ref="swiper" :interval="interval">
      <SwiperItem v-for="(item,index) in imageList" :key="index">
        <img :src="item.src" alt="">
      </SwiperItem>
    </Swiper>
  </div>
</template>

<script>
import Swiper from "./components/swiper/Swiper";
import SwiperItem from "./components/swiper/SwiperItem";

export default {
  name: 'App',
    data(){
      return{
          imageList:[{
              src:require("./assets/img/001.jpg")
          },{
              src:require("./assets/img/002.jpg")
          },{
              src:require("./assets/img/003.jpg")
          },{
              src:require("./assets/img/004.jpg")
          },{
              src:require("./assets/img/005.jpg")
          },{
              src:require("./assets/img/006.jpg")
          }],
        interval: 2000
      }
    },
  components: {
    Swiper,
    SwiperItem
  },
    methods:{
        /*startTimer() {
            this.$refs.swiper && this.$refs.swiper.startTimer()
        },
        stopTimer() {
            this.$refs.swiper && this.$refs.swiper.stopTimer()
        }*/
    }
}
</script>

<style>
h2{
  text-align: center;
  padding-top: 50px;
}
  p{
    padding: 10px ;
    text-align: center;
  }
</style>
