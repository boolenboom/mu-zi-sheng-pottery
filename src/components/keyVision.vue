<script>
const dataSet = require("../assets/data/attractSection/contentSet.json");
console.log("attractSection:");
console.log(dataSet);
let timeNum = 8;
let classlist = {
  ready: "fadeactive fade",
  run: "fadeactive",
  leave: "fadeactive fade",
};
export default {
  name: "keyVision",
  data() {
    return {
      groupNum: 0,
      fadeoutDirection: "fadeout-rightmove",
      timer: null,
      time: timeNum,
      state: "ready",
      orderdelay: 1.2,
    };
  },
  props: {
    viewon: {
      type: Number,
      default: 1,
    },
  },
  methods: {
    countdown() {
      this.time--;
      if (this.time == 0) {
        this.groupNum =
          this.groupNum == dataSet.length - 1 ? 0 : this.groupNum + 1;
      }
    },
    changestate(e) {
      console.log(e);
    },
  },
  computed: {
    carouselContent: function () {
      return dataSet;
    },
    carouselImg: function () {
      return dataSet.map(function (obj) {
        return {
          id: obj.id,
          path: require(`../assets/attractSection/${obj.filename}`),
        };
      });
    },
    animationList: function () {
      return classlist[this.state];
    },
  },
  watch: {
    groupNum: function (newVal, oldVal) {
      this.fadeoutDirection =
        newVal > oldVal ? "fadeout-rightmove" : "fadeout-leftmove";
      this.time = timeNum;
    },
    viewon: function (newVal) {
      this.orderdelay = 0;
      this.state = newVal !== 1 ? "leave" : "run";
    },
  },
  mounted() {
    this.timer = setInterval(this.countdown, 1000);
    setTimeout(() => (this.state = "run"), 100);
  },
  beforeDestroy() {
    clearInterval(this.timer);
  },
};
</script>
<template>
  <section id="main">
    <div class="spring-container h-100">
      <div class="title text-xl responsive-writeMode">
        <!--標題文字-->
        <transition-group
          :name="fadeoutDirection"
          :class="animationList"
          @update="changestate($event)"
        >
          <div
            v-for="(item, index) of carouselContent"
            v-show="groupNum == index"
            :key="`title` + item.id"
          >
            {{ item.text.title }}
          </div>
        </transition-group>
      </div>
    </div>
    <div class="fixed-container vh-100" :style="`--order-delay:${orderdelay}s;`">
      <div class="content" :class="{ 'ready-slidein': state === 'ready' }">
        <!--中央圖片&跑馬燈-->
        <div class="marquee top text-s">
          <p v-for="i of 6" :key="i" :id="i" :class="animationList">
            / Art·Handicrafts·Practical /
          </p>
        </div>
        <transition-group :name="fadeoutDirection" tag="div" class="big-img" :class="animationList">
          <img v-for="(item, index) of carouselImg" :key="`img` + item.id" :src="item.path" v-show="groupNum === index"
            alt="hero image"/>
        </transition-group>
        <div class="marquee bottom text-s">
          <p v-for="i of 6" :key="i" :id="i" :class="animationList">
            / Art·Handicrafts·Practical /
          </p>
        </div>
      </div>
      <div class="indicators" :class="animationList">
        <!--指標器-->
        <div
          v-for="(item, index) of carouselContent"
          :key="item.id"
          class="item responsive"
          :class="{ curr: groupNum == index }"
          @click="groupNum = index"
        ></div>
      </div>
    </div>
  </section>
</template>
<style lang="scss" scoped>
@import "~@/assets/scss/_variables.scss";
@import "~@/assets/scss/_mixins.scss";
#main {
  background: var(--main-color); //#85B8CB;
  .spring-container {
    display: flex;
    flex-flow: row-reverse nowrap;
    align-items: center;
    @include small-pad-width-containFollowing{
      flex-flow: column nowrap;
    }
    .title {
      z-index: 201;
      position: relative;
      height: 100%;
      width: var(--text-xl);
      @include small-pad-width-containFollowing{
        height: var(--text-xl);
        width: 100%;
        margin-top: 15vh;
      }
      div {
        position: absolute;
        height: 100vh;
        @include small-pad-width-containFollowing{
          height: var(--text-xl);
          width: 100%;
        }
      }
    }
  }
  .fixed-container {
    display: flex;
    flex-flow: column nowrap;
    justify-content: space-between;
    z-index: 101;
    .content {
      background-color: var(--secondary-color);
      position: relative;
      width: 73.4375%;
      height: 87.037%;
      transform: rotate(45deg) translate(-25%, -12%);
      @include small-pad-width{
        height: 70%;
        transform: rotate(45deg) translate(-20%, -12%);
      }
      @include phone-width{
        height: 55%;
      }
      transition: transform var(--order-delay) ease-in-out;
      &.ready-slidein {
        transform: rotate(45deg) translateX(-100%) translateY(-12%);
      }
      .marquee {
        position: absolute;
        // font-size: 24px;
        width: 100%;
        border-top: 1px solid var(--secondary-color);
        border-bottom: 1px solid var(--secondary-color);
        padding: 8px 0;
        overflow-x: hidden;
        display: flex;
        flex-flow: row nowrap;
        &.top {
          transform: translateY(calc(-100% - 24px));
        }
        &.bottom {
          transform: translateY(24px);
        }
        p {
          flex: 1 0 25rem;
          animation: marquee-left 8s linear infinite;
        }
        @keyframes marquee-left {
          to {
            transform: translateX(-100%);
          }
        }
      }
      .big-img {
        position: relative;
        height: 100%;
        box-shadow: 0px 0px 20px 15px var(--shadow-color);
        img {
          transform: rotate(-45deg) translate(20% ,20%) rotate(8deg);
          position: absolute;
          bottom: 0;
          right: 0;
          // max-width: 640px;
          // max-height: 640px;
          width: 80vh;
          @include pad-width{
            transform: rotate(-45deg) translate(20% ,4vh) rotate(8deg);
            width: 50vh;
          }
          @include small-pad-width-containFollowing{
            transform: rotate(-45deg) translate(20% ,4vh) rotate(8deg);
            width: 50vh;
          }
          // box-shadow: 0px 0px 20px rgba($color: #000000, $alpha: .8);
          filter: drop-shadow(
            10px 20px 20px rgba($color: #000000, $alpha: 0.8)
          );
        }
      }
    }
    .indicators {
      display: flex;
      flex-direction: row;
      justify-content: center;
      .item {
        z-index: 50;
        background-color: var(--text-color);
        &:hover {
          cursor: pointer;
        }
        &.curr{
          background-color: var(--secondary-color);
        }
        &.responsive{
          @include pc-width{
            width: 100px;
            height: 8px;
            margin: 62px 0px;
            border-radius: 2px;
            &:not(:first-child){
              margin-left: 8px;
            }
          }
          @include pad-width-containFollowing{
            width: 24px;
            height: 24px;
            border-radius: 50%;
            margin: 24px 0px;
            &:not(:first-child){
              margin-left: 64px;
            }
          }
        }
      }
    }
  }
  .fadeout-rightmove-leave-active,
  .fadeout-leftmove-enter-active,
  .fadeout-leftmove-leave-active,
  .fadeout-rightmove-enter-active {
    transition: transform 0.8s ease, opacity 0.8s ease;
  }
  .fadeout-rightmove-enter,
  .fadeout-leftmove-leave-to {
    transform: translateX(-25%);
    opacity: 0;
  }
  .fadeout-rightmove-enter-to,
  .fadeout-rightmove-leave,
  .fadeout-leftmove-enter-to,
  .fadeout-leftmove-leave {
    transform: translateX(0);
    opacity: 1;
  }
  .fadeout-rightmove-leave-to,
  .fadeout-leftmove-enter {
    transform: translateX(25%);
    opacity: 0;
  }
  .fadeactive {
    transition: opacity 1s var(--order-delay) ease;
  }
  .fade {
    opacity: 0;
  }
}
</style>
