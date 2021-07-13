<template>
  <section id="potteryList" class="section">
    <div class="background">
      <!-- <div class="Rect-07"></div> -->
      <!-- <div class="Rect-08"></div> -->
      <carousel />
      <div class="full"></div>
    </div>
  </section>
</template>
<script>
import carousel from './carousel.vue';

export default {
  name: "potteryList",
  components:{
    carousel,
  }
};
</script>
<style lang="scss" scoped>
#potteryList {
	overflow-y: hidden;
  // [class*="-07"] {
  //   width: 73.4375%;
  //   height: 87.037%;
  //   background-color: #fff;
  //   transform: rotate(45deg) translate(54.6%, -42.4%);
  // }
  // [class*="-08"] {
  //   top: 0;
  //   left: 0;
  //   @media (min-width: 177vh) {
  //     left: calc((100vw - 177.778vh) / -2);
  //     width: 100vw;
  //   }
  //   width: 100%;
  //   height: 60%;
  //   transform: translateY(26%);
	// 	background-color: transparent;
  // }
  .full {
    position: absolute;
    z-index: -1;
    top: 0;
    left: 0;
    @media (min-width: 177vh) {
      left: calc((100vw - 177.778vh) / -2);
      width: 100vw;
    }
    width: 100%;
    height: 120%;
    background: #85b8cb;
  }
}
</style>