<template>
  <div id='Home' class="home" @wheel="scrollhandler($event)" @transitionend.self='snap($event)' :style="`transform:translateY(${pageoffset}px);`">
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

.home{
  transition: transform 1.5s cubic-bezier(.2,.4,.7,.9);
}

[class*='Rect-'] {
    position: absolute;
    background-color: #fff;
}
</style>
