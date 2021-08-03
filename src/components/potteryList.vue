<template>
  <section id="potteryList">
    <div class="spring-container">
      <carousel sectiontitle="Featured" :quantity='9'/>
    </div>
    <div class="fixed-container">
      <div class="Rect-07"></div>
      <div class="Rect-08"></div>
    </div>
      <div class="full"></div>
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
  z-index: 2;
	overflow: hidden;
  .spring-container{
    position: absolute;
    height: 100%;
  }
  [class*="-07"] {
    width: 19.5%;
    height: 150%;
    background-color: var(--BG-color);
    transform: rotate(45deg) translate(0%, -32.4%);
  }
  [class*="-08"] {
    right: 0;
    height: 160%;
    width: 19.5%;
    background-color: var(--BG-color);
    transform: rotate(-45deg) translate(162.5%, -8%);
  }
  .full {
    position: absolute;
    z-index: -1;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: #85b8cb;
  }
}
</style>