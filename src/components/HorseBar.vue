<template>
  <div class="marquee" ref="marquee">
    <span class="content" ref="content"
    :style="{animationDuration: onceTime + 'ms',animationPlayState : active}">
      {{ notice }} <span class="content-space"></span>
    </span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      notice: '測試的長度影響次數如果再長一點的話呢?測試的長度影響次數如果再長一點的話呢?',
      animationDuration:'',  
      speed: 50,  //動畫速度
      active: 'runing', //控制動畫 runing / paused
      onceTime: 0, // 一次动画应该要花费多少时间
    }
  },

  mounted(){
    this.run()
  },

  methods:{
    run(){
      // console.log(this.$refs.marquee.clientWidth,this.$refs.content.clientWidth)
      // var speed = 50; // 速度 -- px每秒
      var $marquee = document.querySelector('.marquee');
      var $marqueeContent = $marquee.querySelector('.content');
      // 内容复制4份好有连续性
      $marqueeContent.innerHTML = $marqueeContent.innerHTML + $marqueeContent.innerHTML + $marqueeContent.innerHTML + $marqueeContent.innerHTML
      var contentWidth = $marqueeContent.getBoundingClientRect().width;
      if (contentWidth <= 0) { // 没有内容搞什么动画
        return;
      }

      //判斷動畫寬度有沒有超過畫面,不足就乘一定倍數
      let copyTimes = parseInt(this.$refs.marquee.clientWidth / contentWidth)
      let finalHtml = ''
      for (let i=0;i<=copyTimes+1;i++){
        finalHtml += $marqueeContent.innerHTML
      }
      $marqueeContent.innerHTML = finalHtml;

      // 内容复制了4份，宽度也要除以4
      contentWidth = contentWidth / 4

      // 计算一次动画应该要花费多少时间
      this.onceTime = contentWidth / this.speed * 1000; //ms
      // var onceTime = contentWidth / this.speed * 1000; //ms

      // $marqueeContent.style.animationDuration = onceTime + "ms"
      // this.animationDuration = 'animationDuration:' + onceTime + "ms"
    }
  }

}
</script>

<style lang="scss" scoped>
.marquee{
  // width: 400px;
  height: 44px;
  line-height: 44px;
  background: #e9eaea;
  display: block;
  margin: 0 auto;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: clip;
  position: relative;
}
.marquee .content{
  display: inline-block;
  position: relative;
  padding-right: 0px;
  white-space: nowrap;
  animation: kf-marque-animation 12s infinite linear;
  animation-fill-mode: forwards;
}
.marquee .content-space{
  display: inline-block;
  width: 5em;
}

@keyframes kf-marque-animation
{ 
  0% { transform: translateX(0); } 
  100% { transform: translateX(-25%); } 
}

</style>