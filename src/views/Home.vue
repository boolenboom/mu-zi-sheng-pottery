<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <attractSection />
    <studiointro />
    <potterylist />
  </div>
</template>

<script>
// @ is an alias to /src
import attractSection from '../components/attractSection.vue';
import studiointro from "@/components/studioIntro.vue";
import potterylist from '../components/potteryList.vue';

export default {
  name: "Home",
  components: {
    attractSection,
    studiointro,
    potterylist,
  },
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
