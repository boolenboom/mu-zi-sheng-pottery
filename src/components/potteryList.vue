<template>
  <section id="potteryList">
    <div class="spring-container h-100">
      <carousel sectiontitle="Featured" :quantity="9" />
    </div>
    <div class="fixed-container vh-100">
      <div class="Rect-07"></div>
      <div class="Rect-08"></div>
    </div>
    <div class="full"></div>
  </section>
</template>
<script>
import carousel from "./carousel.vue";

export default {
  name: "potteryList",
  components: {
    carousel,
  },
};
</script>
<style lang="scss" scoped>
@import "~@/assets/scss/_variables.scss";
@import "~@/assets/scss/_mixins.scss";
#potteryList {
  z-index: 2;
  overflow: hidden;
  [class*="-07"] {
    width: 19.6%;
    height: 150%;
    transform: translate(61%, -25%) rotate(45deg);
    @include small-pad-width-containFollowing{
      transform: translate(89%, -25%) rotate(45deg);
    }
  }
  [class*="-08"] {
    right: 0;
    height: 160%;
    width: 19.6%;
    transform: translate(83.5%, -25%) rotate(-45deg);
    @include small-pad-width-containFollowing{
      transform: translate(90%, -25%) rotate(-45deg);
    }
  }
  .full {
    position: absolute;
    z-index: -1;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: var(--main-color);
  }
}
</style>
