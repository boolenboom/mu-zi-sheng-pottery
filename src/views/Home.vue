<script>
// @ is an alias to /src
import keyVision from "@/components/keyVision.vue";
import studiointro from "@/components/studioIntro.vue";
import potterylist from "@/components/potteryList.vue";

// let scrollratio = 0.2;
let clamp = function (val, min, max) {
    return Math.min(max, Math.max(val, min));
  },
  checkCurrheight = function (cacheHeight) {
    if (window.innerHeight === cacheHeight) return cacheHeight;
    return window.innerHeight;
  };
let scrollhandler = function(ratio){
  let scrollratio = ratio || 1;
  let startpoint = 0;
  let curroffset = 0;
  return{
    wheel:function(delta, curr, viewH){
      console.log(delta);
      return curr - (Math.sign(delta) * viewH * scrollratio);
    },
    touchstart:function(start, curr){
      startpoint = start;
      curroffset = curr;
      return curroffset;
    },
    touchmove:function(point){
      let offset = curroffset + (point - startpoint);
      return offset;
    },
    touchend:function(){
      const endoffset = curroffset;
      startpoint = 0;
      curroffset = 0;
      return endoffset;
    }
  }
}

export default {
  name: "Home",
  components: {
    keyVision,
    studiointro,
    potterylist,
  },
  props: {
    componentIndex: {
      type: Number,
      default: 1,
    },
  },
  data() {
    return {
      pageoffset: 0,
      childNum: 0,
      viewheight: 0,
      controller: scrollhandler(0.2),
    };
  },
  methods: {
    scrollhandler(e) {
      console.log(e);
      this.viewheight = checkCurrheight(this.viewheight);
      let value = e.type.match('wheel') ? e.deltaY : e.changedTouches[0].clientY,
        viewH = this.viewheight;
      this.pageoffset = this.controller[e.type](value, this.pageoffset, viewH);
      this.pageoffset = clamp(this.pageoffset, -viewH * (this.childNum - 1), 0);
    },
    snap(e) {
      console.log(e.type);
      let viewH = this.viewheight;
      if (this.pageoffset % viewH === 0) return 0;
      this.pageoffset = Math.round(this.pageoffset / viewH) * viewH;
    },
  },
  computed: {
    currView: function () {
      let viewscope = this.viewheight,
        count = 1,
        curr = 0;
      while (-this.pageoffset > viewscope * (count - 0.5)) {
        count++;
      }
      curr = count;
      return curr;
    },
  },
  watch: {
    componentIndex(newVal) {
      this.pageoffset = (newVal - 1) * this.viewheight * -1;
    },
  },
  mounted() {
    const parent = document.querySelector(".home");
    this.childNum = parent.childNodes.length;
    this.viewheight = window.innerHeight;
  },
};
</script>

<template>
  <div
    id="Home"
    class="home"
    @wheel="scrollhandler($event)"
    @touchstart='scrollhandler($event)' @touchmove='scrollhandler($event)'
    @transitionend.self="snap($event)"
    :style="`transform:translateY(${pageoffset}px);`"
  >
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <keyVision :viewon="currView" />
    <studiointro :viewon="currView" :offset="pageoffset" />
    <potterylist :viewon="currView" />
  </div>
</template>

<style lang="scss">

.home {
  transition: transform 1s cubic-bezier(0.165, 0.84, 0.44, 1);
  background-color: var(--main-color);
}
</style>
