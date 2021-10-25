<script>
const imgdata = require("../assets/data/studioIntro/contentSet.json"),
  dataSet = [...imgdata.set];
let path = "assets/studioIntro";
console.log(dataSet);
export default {
  name: "studioIntro",
  data() {
    return {
      // imgpath:[require('../assets/studioIntro/studio02@2x.jpg')],
      currBG: 0,
      mouseEventPermission: true,
      isShow: false,
      scrollDir: "down",
    };
  },
  props: {
    viewon: { type: Number, default: 1 },
    offset: { type: Number },
  },
  computed: {
    IntroImg: function () {
      const global = dataSet.slice(0, 1)[0],
        set = dataSet.slice(1),
        widthratio = 100 / global.widthGrid,
        heightratio = 100 / global.heightGrid;
      let item = set.map(function (obj) {
        let top = (obj.pos.y - 1) * heightratio,
          left = (obj.pos.x - 1) * widthratio,
          width = obj.size.width * widthratio,
          height = obj.size.height * heightratio;
        return {
          id: obj.id,
          style: `--id:${obj.id};
                  --rect-top:${top};
                  --rect-left:${left};
                  --rect-width:${width};
                  --rect-height:${height};`,
          class: `${width > height ? "left-right" : "up-down"}`,
          path: require(`../${path}/${obj.filename}`),
        };
      });
      return item;
    },
    Subtitle: function () {
      return dataSet.slice(1).map((e) => e.subtitle);
    },
    animationList: function () {
      return this.offset !== -window.innerHeight ? "fadeout-opacity-enter" : "";
    },
    isReadySlide: function () {
      return this.viewon === 1 ? "slide" : "";
    },
    animationSetting: function () {
      return `--order:${
        this.scrollDir === "down"
          ? "calc(var(--id) - 1)"
          : "calc(5 - var(--id))"
      };
      --duration:0.2s;
      --image-delay:${this.scrollDir === "down" ? "0.8" : "0"}s;`;
    },
  },
  methods: {
    changeBG(index) {
      if (this.mouseEventPermission) this.currBG = index;
    },
    detailshow(index) {
      if (!this.mouseEventPermission) return 0;
      this.changeBG(index);
      this.mouseEventPermission = false;
      this.isShow = true;
    },
    eventEnd() {
      this.isShow = false;
      this.mouseEventPermission = true;
    },
  },
  watch: {
    offset: function (newVal, oldVal) {
      this.scrollDir = Math.abs(newVal) > Math.abs(oldVal) ? "down" : "up";
    },
  },
};
</script>

<template>
  <section id="studioIntro">
    <div class="spring-container h-100">
      <div class="txt">
        <div class="title text-xl veritcal-write" :class="{ leftmove: isShow }">
          Studio
        </div>
        <!-- <div class="intro" :class="{ leftmove: !show }">
          <h3>次標題</h3>
          <p>
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Ad quos eum
            quidem vitae hic, ut recusandae praesentium at rem dolore provident
            ipsam harum inventore assumenda excepturi perferendis modi nemo
            ipsa.
          </p>
        </div> -->
      </div>
      <div class="image-section" :class="animationList">
        <transition-group
          tag="div"
          class="detailimg"
          name="fadeout-opacity"
          :class="{ filterblur: !isShow }"
        >
          <img
            v-for="(item, index) of IntroImg"
            v-show="index === currBG"
            :src="item.path"
            :key="item.id"
            alt=""
          />
        </transition-group>
        <div v-show="isShow" class="back">
          <div class="icon" @click="eventEnd()"></div>
        </div>
      </div>
    </div>
    <div class="fixed-container vh-100" :class="{ show: isShow }">
      <div class="comp">
        <div
          v-for="(item, index) of IntroImg"
          :key="item.id"
          :style="item.style + animationSetting"
          :class="[item.class, isReadySlide, 'Rect']"
          @mouseenter="changeBG(index)"
          @click="detailshow(index)"
        >
          <img
            :id="`img` + item.id"
            :src="item.path"
            :class="animationList"
            alt=""
          />
        </div>
        <!-- <div class='Rect-01'>
                    <img src="../assets/studioIntro/studio02@2x.jpg" alt="">
                </div> -->
        <transition-group
          tag="div"
          class="subtitle"
          name="fadeout-opacity"
          :class="animationList"
        >
          <h2
            v-for="(item, index) of Subtitle"
            :key="index"
            v-show="index === currBG"
            class="txt text-base"
          >
            {{ item }}
          </h2>
        </transition-group>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@import "~@/assets/scss/_variables.scss";
@import "~@/assets/scss/_mixins.scss";
#studioIntro {
  height: auto;
  background-color: var(--main-color);
  .spring-container {
    display: flex;
    flex-direction: row;
    // background-color: rgba($color: #364286, $alpha: .3);
    .txt {
      width: var(--text-l);
      .title {
        position: absolute;
        bottom: 0;
        z-index: 201;
        line-height: 0.75;
        transition: transform 0.3s 0.1s ease;
      }
      .intro {
        // font-size: 36px;
        position: absolute;
        line-height: 2;
        transition: transform 0.4s 0.6s ease;
      }
    }
    .image-section {
      width: 70%;
      height: 100%;
      .detailimg {
        position: relative;
        transition: filter 0.4s 0.8s ease;
        img {
          position: absolute;
          left: 0;
          right: 0;
          margin: 0 auto;
          width: 80%;
        }
      }
      .back {
        position: absolute;
        top: 10%;
        right: 0;
        z-index: 10;
        .icon {
          width: 24px;
          height: 24px;
          background-color: #fff;
        }
      }
    }
  }
  .fixed-container {
    transition: transform 1.2s cubic-bezier(0.61, -0.25, 0.26, 1);
    .comp {
      position: absolute;
      width: 175vh;
      height: 175vh;
      transform: rotate(45deg) translate(0.5%, -20%);
      @include small-pad-width{
        transform: rotate(45deg) translate(4.5%, -24.8%) ;
      }
      @include phone-width{
        width: 125vh;
        height: 125vh;
        transform: rotate(45deg) translate(25%, -24.8%) ;
      }
      z-index: 1;
      .subtitle {
        position: absolute;
        top: 40%;
        left: 20%;
        width: 20%;
        height: 20%;
        transition: opacity 0.3s;
        .txt {
          position: absolute;
          transform: translateY(-50%) rotate(-45deg);
          top: 50%;
          width: 100%;
        }
      }
    }
  }
  .Rect{
        top: calc(var(--rect-top) * 1%);
        left: calc(var(--rect-left) * 1%);
        width: calc(var(--rect-width) * 1%);
        height: calc(var(--rect-height) * 1%);
        cursor: pointer;
        overflow: hidden;
        img {
          width: 100%;
          height: 100%;
          transform: rotate(-45deg)
            scaleX(calc((1 + var(--rect-height) / var(--rect-width)) / 1.414))
            scaleY(calc((1 + var(--rect-width) / var(--rect-height)) / 1.414));
          transition: opacity 0.3s var(--image-delay);
        }
      }
  .fadeout-opacity-enter-active,
  .fadeout-opacity-leave-active {
    transition: opacity 0.3s;
  }
  .fadeout-opacity-enter,
  .fadeout-opacity-leave-to {
    opacity: 0;
  }
  .show {
    transform: translateX(100%);
  }
  .filterblur {
    filter: blur(30px) grayscale(0.8);
  }
  .leftmove {
    transform: translateX(-100%);
  }
  .up-down.slide {
    height: 0%;
  }
  .left-right.slide {
    width: 0%;
  }
  .up-down {
    transition: height var(--duration) calc(var(--order) * 0.9 * var(--duration)) ease;
  }
  .left-right {
    transition: width var(--duration) calc(var(--order) * 0.9 * var(--duration)) ease;
  }
}
</style>
