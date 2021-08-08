<script>
// @ is an alias to /src
import keyVision from "@/components/keyVision.vue";
import studiointro from "@/components/studioIntro.vue";
import potterylist from "@/components/potteryList.vue";

let scrollratio = 0.2;
let clamp = function (val, min, max) {
    return Math.min(max, Math.max(val, min));
  },
  checkCurrheight = function (cacheHeight) {
    if (window.innerHeight === cacheHeight) return cacheHeight;
    return window.innerHeight;
  };

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
    };
  },
  methods: {
    scrollhandler(e) {
      this.viewheight = checkCurrheight(this.viewheight);
      let delta = e.deltaY,
        viewH = this.viewheight;
      this.pageoffset -= (delta / 100) * viewH * scrollratio;
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
  transition: transform 1s cubic-bezier(0.2, 0.4, 0.7, 0.9);
  background-color: var(--main-color);
}
</style>
