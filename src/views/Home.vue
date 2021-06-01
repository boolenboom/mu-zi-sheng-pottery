<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <studioIntro msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
// @ is an alias to /src
import studioIntro from "@/components/studioIntro.vue";

export default {
  name: "Home",
  components: {
    studioIntro,
  },
};
</script>
<style lang="scss">
$pc:1440px;
$table:768px;
$mobile:360px;
body {
    height: 100%;
}

* {
    padding: 0;
    margin: 0;
    text-decoration: none;
}

 :root {
    --BG-color: #C4C4C4;
}

.section {
    width: 100%;
    overflow: hidden;
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
