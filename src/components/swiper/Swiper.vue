<template>
    <div class="container">
        <div class="swiper" @touchstart="touchStart" @touchmove="touchMove" @touchend="touchEnd">
            <slot></slot>
        </div>
        <div class="change-btn">
            <slot name="change-btn" v-if="showChangeBtn && slideCount > 1">
                <div class="pre" @click="previous"><img src="../../assets/img/pre.svg" alt=""></div>
                <div class="next" @click="next"><img src="../../assets/img/next.svg" alt=""></div>
            </slot>
        </div>
        <div class="pagination">
            <slot name="pagination"  v-if="showPagination && slideCount > 1">
                <div class="pagination-item" v-for="(item,index) in slideCount" :class="{active: index === currentIndex-1}" :key="index"></div>
            </slot>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Swiper",
        props: {
            interval: { //滚动时间间隔
                type: Number,
                default: 3000
            },
            animDuration: { //滚动延时
                type: Number,
                default: 300
            },
            moveRatio: { // 滚动翻页比例
                type: Number,
                default: 0.25
            },
            showPagination: { //导航条 pagination
                type: Boolean,
                default: true
            },
            showChangeBtn:{ // 左右切换按钮
                type: Boolean,
                default: true
            }
        },
        data (){
            return{
                slideCount: 0, // 元素个数
                totalWidth: 0, // swiper的宽度
                swiperStyle: {}, // swiper样式
                currentIndex: 1, // 当前的index
                scrolling: false // 是否正在滚动
            }
        },
        mounted() {
            setTimeout(()=>{
                // 开始操作DOM
                this.handlerDOM()
                //计时开始
                this.startTimer()
            },100)
        },
        methods:{
            // 定时器
            startTimer (){
                this.playTime = window.setInterval(()=>{
                    this.currentIndex ++
                    this.imgScroll(-this.currentIndex * this.totalWidth)
                },this.interval)
            },
            stopTimer (){
                window.clearInterval(this.playTime)
            },
            //图片滚动到指定位置
            imgScroll (currentPosition){
                // 设置正在滚动
                this.scrolling = true
                // 开始滚动动画
                this.swiperStyle.transition = 'transform ' + this.animDuration + 'ms'
                this.setTransform(currentPosition)
                //判断滚动位置
                this.checkPosition()
                // 滚动完成
                this.scrolling = false
            },

            // 检验正确位置
            checkPosition (){
                window.setTimeout(() => {
                    // 1.校验正确的位置
                    this.swiperStyle.transition = '0ms';
                    if (this.currentIndex >= this.slideCount + 1) {
                        this.currentIndex = 1;
                        this.setTransform( -this.currentIndex * this.totalWidth)
                    } else if (this.currentIndex <= 0) {
                        this.currentIndex = this.slideCount;
                        this.setTransform( -this.currentIndex * this.totalWidth);
                    }
                    // 2.结束移动后的回调
                    this.$emit('transitionEnd', this.currentIndex-1);
                }, this.animDuration)
              //  console.log('checkPosition');
            },
            // 设置滚动位置
            setTransform (position) {
                this.swiperStyle.transform = `translate3d(${position}px, 0, 0)`;
                this.swiperStyle['-webkit-transform'] = `translate3d(${position}px), 0, 0`;
                this.swiperStyle['-ms-transform'] = `translate3d(${position}px), 0, 0`;
              //  console.log('setTransform');
            },
            // 操作DOM元素
            handlerDOM (){
                // 获取要操作的元素
                let swiperElement = document.querySelector('.swiper')
                let slideElement = swiperElement.getElementsByClassName('slider')
                //console.log(slideElement);
                // 获取滚动元素个数
                this.slideCount = slideElement.length
                // 如果滚动个数大于1 则在当前slider前后各添加一个slider
                if (this.slideCount > 1) {
                    let cloneFirst =  slideElement[0].cloneNode(true)
                    let cloneEnd = slideElement[this.slideCount - 1].cloneNode(true)
                    swiperElement.insertBefore(cloneEnd,slideElement[0])
                    swiperElement.appendChild(cloneFirst)
                    this.totalWidth = swiperElement.offsetWidth
                    this.swiperStyle = swiperElement.style
                }
                // 让swiper元素, 显示第一个(目前是显示前面添加的最后一个元素)
                this.setTransform(-this.totalWidth)
               // console.log(this.totalWidth);
            },
            // 拖动事件处理
            touchStart(e){
                // 如果是正在滚动，禁止拖动
                if(this.scrolling) return
                // 停止计时器
                this.stopTimer()
                //保存开始滚动的位置
                this.startX = e.touches[0].pageX
            },
            touchMove(e){
                // 计算用户拖动的距离
                this.currentX = e.touches[0].pageX
                this.distance = this.currentX - this.startX
                let currentPosition = -this.currentIndex * this.totalWidth
                let moveDistance = this.distance + currentPosition

                // 设置当前的位置
                this.setTransform(moveDistance)
            },
            touchEnd(){
                // 获取移动距离
                let currentMove = Math.abs(this.distance)
                // 判断最终的举例
                if(this.distance === 0){
                    return
                }else if (this.distance > 0 && currentMove > this.totalWidth * this.moveRatio){
                    this.currentIndex --
                }else if (this.distance < 0 && currentMove > this.totalWidth * this.moveRatio){
                    this.currentIndex ++
                }

                // 移动到正确位置
                this.imgScroll(-this.currentIndex * this.totalWidth)
                // 开启定时器
                this.startTimer()
            },
            // Pagination 翻页
            previous (){
                this.changeItem(-1)
               // console.log('pre');
            },
            next (){
                this.changeItem(1)
              //  console.log('next');
            },
            // 改变slide-item
            changeItem (num){
                // 移除定时器
                this.stopTimer()
                // 修改index和位置
                this.currentIndex += num
                this.imgScroll(-this.currentIndex * this.totalWidth)
                // 添加定时器
                this.startTimer()
            }
        }
    }
</script>

<style scoped>
.container{
    overflow: hidden;
    position: relative;
}
    .swiper{
        display: flex;
    }
    .change-btn{
        position: absolute;
        top: 50%;
        left: 0;
        right: 0;
        transform: translateY(-50%);
        display: flex;
        justify-content: space-between;
        padding: 0 10px;
    }
    .change-btn img{
        width: 24px;
        height: 24px;
        background: rgba(0,0,0,.3);
        padding: 8px;
        border-radius: 100%;
    }
    .change-btn .pre,.next{
        box-sizing: border-box;
    }
    .pagination{
        display: flex;
        justify-content: center;
        position: absolute;
        width: 100%;
        bottom: 8px;
    }
    .pagination-item{
        box-sizing: border-box;
        width: 8px;
        height: 8px;
        border-radius: 4px;
        background-color: rgba(0,0,0,.3);
        border: 1px solid #dcdcdc;
        line-height: 8px;
        text-align: center;
        font-size: 12px;
        margin: 0 5px;
    }
    .pagination-item.active{
        background-color: #ff5777;
    }
</style>