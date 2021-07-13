<template>
  <div class="home" data='123456' slot="pageoffset" @wheel="scrollhandler($event)" @transitionend.self='snap($event)' :style="`transform:translateY(${pageoffset}px);`">
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <keyVision :viewon='currView'/>
    <studiointro :viewon='currView' :offset='pageoffset'/>
    <potterylist :viewon='currView'/>
  </div>
</template>

<script>
// @ is an alias to /src
import keyVision from '@/components/keyVision.vue';
import studiointro from "@/components/studioIntro.vue";
import potterylist from '@/components/potteryList.vue';

let scrollratio = 0.2;

export default {
  name: "Home",
  components: {
    keyVision,
    studiointro,
    potterylist,
  },
  data(){
    return{
      pageoffset:0,
      childNum:0,
      viewheight:window.innerHeight
    }
  },
  methods:{
    scrollhandler(e){
      let delta = e.deltaY,
          viewH = this.viewheight;
      this.pageoffset -= delta / 100 * viewH * scrollratio;
      this.pageoffset = this.pageoffset > 0 ? 
        0 : -this.pageoffset > viewH* (this.childNum - 1) ?
        -viewH* (this.childNum - 1) : this.pageoffset;//限制範圍
      
    },
    snap(e){
      console.log(e.type);
      // let viewH = this.viewheight;
      // if(this.pageoffset % viewH=== 0)return 0;
      // this.pageoffset = Math.round(this.pageoffset / viewH) * viewH;
    }
  },
  computed:{
    currView:function(){
      let viewscope = [0.5*this.viewheight,1.5*this.viewheight],
      curr = viewscope[0] > -this.pageoffset ? 1 : viewscope[1] > -this.pageoffset ? 2 : 3;
      return curr;
    }
  },
  mounted(){
    const parent = document.querySelector('.home');
    this.childNum = parent.childNodes.length;
  }
};
</script>

<style lang="scss">
$pc: 1440px;
$table: 768px;
$mobile: 360px;

* {
    padding: 0;
    margin: 0;
    text-decoration: none;
}

 :root {
    --BG-color: #C4C4C4;
}

section {
  // scroll-snap-align: start;
  width: 100%;
  height: 100vh;
  position: relative;
  text-align: center;
}

.home{
  transition: transform 1.5s cubic-bezier(.2,.4,.7,.9);
}

.background {
    position: relative;
    margin: 0 auto;
    height: 100vh;
    width: 177.778vh;
    @media (max-width: 177vh) {
        margin: 0 calc((100% - 177.778vh) / 2);
    }
}

[class*='Rect-'] {
    position: absolute;
    background-color: #fff;
}
</style>
