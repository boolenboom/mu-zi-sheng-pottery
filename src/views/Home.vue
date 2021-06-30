<template>
  <div class="home" data='123456' slot="pageoffset" @wheel="scrollhandler($event)" :style="`transform:translateY(${pageoffset}px);`">
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <keyVision />
    <studiointro />
    <potterylist />
  </div>
</template>

<script>
// @ is an alias to /src
import keyVision from '@/components/keyVision.vue';
import studiointro from "@/components/studioIntro.vue";
import potterylist from '@/components/potteryList.vue';

export default {
  name: "Home",
  components: {
    keyVision,
    studiointro,
    potterylist,
  },
  data(){
    return{
      pageoffset:0
    }
  },
  methods:{
    scrollhandler(e){
      console.log(window.innerHeight);
      this.pageoffset -= e.deltaY;
      this.pageoffset = this.pageoffset > 0 ? 0 : -this.pageoffset > window.innerHeight * 2 ?  -window.innerHeight * 2 : this.pageoffset;
    }
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
  transition: transform .5s cubic-bezier(0,.5,.5,1);
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
