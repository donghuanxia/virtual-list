<template>
<!-- 能滚动的盒子 -->
  <div class="viewport" ref="viewport" @scroll="hancleScroll">
    <!-- //自己做的一个滚动条 -->
    <div class="scroll-bar" ref="scrollBar"></div>
    <!-- 真实的渲染内容 -->
    <div class="scroll-list" :style="{transform:`translate3d(0,${offset}px,0)`}">
      <div v-for="item in visibleData" :key="item.id" :vid="item.id">
        <slot :item="item"></slot>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props:{
    size:Number,//当前每一项的高度度
    remain:Number,//可见区域的个数
    items:Array
  },
  mounted(){
    this.$refs.viewport.style.height = this.size*this.remain+'px'
    this.$refs.scrollBar.style.height  = this.size*this.items.length+'px'
  },
  data(){
    return{
      start:0,
      end:this.remain,//默认显示8个
      offset:0
    }
  },
  computed:{
    //渲染3屏
    //前面预留几个
    preCount(){
      return Math.min(this.start,this.remain)//例如（5，8）//取5
    },
    //后面预留几个
    nextCount(){
      return Math.min(this.remain,this.items.length-this.end)//例如（8，5）取5
    },
    visibleData(){
      let start = this.start - this.preCount
      let end = this.end + this.nextCount
      return this.items.slice(start,end)
    }
  },
  methods:{
    hancleScroll(){
      //1、应该下算出来当前滚过去几个了，当前应该从几个开始显示
      //拿到当前用户滚动的距离
      let scrollTop = this.$refs.viewport.scrollTop
      //2、获取当期应该从第几个开始渲染
      console.log(scrollTop)
      this.start = Math.floor(scrollTop/this.size)// 130/40 = 3.25取3
      this.end = this.start +this.remain
      //定位当前的可视区域，让当前渲染的内容在当前的viewport的可视区域
      //如果有预留渲染，应该把这个位置在向上移动当这么多
      this.offset = this.start *this.size - this.size*this.preCount//让可视区域去调整偏移量

    }
  }
}
</script>
<style lang="scss" scoped>
  .viewport{
    overflow-y: scroll;
    position: relative;
  }
  .scroll-bar{
    position: absolute;
    top:0;
    left:0;
    width:100%;
  }
</style>