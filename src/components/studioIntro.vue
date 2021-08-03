<template>
  <section id="studioIntro">
    <div class="spring-container">
      <div class="txt">
        <div class="title fz-xl veritcal-write" :class="{ leftmove: show }">Studio</div>
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
          :class="{ filterblur: !show }"
        >
          <img
            v-for="(item, index) of IntroImg"
            v-show="index === currBG"
            :src="item.path"
            :key="item.id"
            alt=""
          />
        </transition-group>
        <div v-show="show" class="back">
          <div class="icon" @click="eventEnd()"></div>
        </div>
      </div>
    </div>
    <div class="fixed-container" :class="{ show: show }">
      <div class="comp">
        <div
          class="Rect"
          v-for="(item, index) of IntroImg"
          :key="item.id"
          :style="item.style + animationSetting"
          :class="[item.class ,timing]"
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
        <transition-group tag="div" class="subtitle" name="fadeout-opacity" :class="animationList">
          <h2
            v-for="(item, index) of Subtitle"
            :key="index"
            v-show="index === currBG"
            class="txt"
          >
            {{ item }}
          </h2>
        </transition-group>
      </div>
    </div>
  </section>
</template>
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
      show: false,
      scrollDir:'down'
    };
  },
  props: {
    viewon: { type: Number, default: 1 },
    offset: { type: Number }
  },
  computed: {
    IntroImg: function () {
      const global = dataSet.slice(0, 1)[0],
        set = dataSet.slice(1),
        widthratio = 100 / global.widthGrid,
        heightratio = 100 / global.heightGrid;
      let item = set.map(function (obj) {
        //目前因資料獨立所以不做填空白的功能
        let top = (obj.pos.y - 1) * heightratio,
          left = (obj.pos.x - 1) * widthratio,
          width = obj.size.width * widthratio,
          height = obj.size.height * heightratio;
        return {
          id: obj.id,
          style: `--id:${obj.id};
                  top:${top}%;
                  left:${left}%;
                  width:${width}%;
                  height:${height}%;`,
					class:`${width > height ?'left-right' :'up-down'}`,
          path: require(`../${path}/${obj.filename}`),
        };
      });
      console.log(item);
      return item;
    },
    Subtitle: function () {
      return dataSet.slice(1).map((e) => e.subtitle);
    },
    animationList: function () {
      return this.offset !== -window.innerHeight ? "fadeout-opacity-enter" : '';
    },
		timing:function(){
			return this.viewon===1?' slide':'';
		},
    animationSetting: function(){
      return `--order:${this.scrollDir === 'down' ? 'calc(var(--id) - 1)' : 'calc(5 - var(--id))'};
              --duration:0.2s;
              --image-delay:${this.scrollDir === 'down' ? '0.8' : '0'}s;`;
    }
  },
  methods: {
    changeBG(index) {
      if (this.mouseEventPermission) this.currBG = index;
    },
    detailshow(index) {
      if (!this.mouseEventPermission) return 0;
      this.changeBG(index);
      this.mouseEventPermission = false;
      this.show = true;
    },
    eventEnd() {
      this.show = false;
      this.mouseEventPermission = true;
    }
  },
  watch:{
    offset:function(newVal,oldVal){
      this.scrollDir = Math.abs(newVal) > Math.abs(oldVal) ? 'down' : 'up';
    }
  }
};
</script>
<style lang="scss" scoped>
#studioIntro {
  height: auto;
  background-color: #85b8cb;
  .spring-container {
    position: absolute;
    display: flex;
    flex-direction: row;
    height: 100%;
    // background-color: rgba($color: #364286, $alpha: .3);
    .txt {
      width: var(--fz-xl);
      .title {
        position: absolute;
        bottom: 0;
        z-index: 201;
        left: 3%;
        letter-spacing: 1.5rem;
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
        top: 0;
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
      width: 98.4375%;
      height: 175%;
      transform: rotate(45deg) translate(7%, -24.35%);
      // width: 78.75%;
      // height: 140%;
      // transform: rotate(45deg) translate(10%,-19.7%);  //layout ver.
      z-index: 1;
      [class*="Rect"] {
        background-color: var(--BG-color);
        // outline: 1px solid #000;
        overflow: hidden;
        position: absolute;
        img {
          transform: rotate(-45deg);
          transition: opacity 0.3s var(--image-delay);
        }
      }
      [id*="img0"]:hover {
        cursor: pointer;
      }
      #img01 {
        position: relative;
        top: 15%;
        left: -5%;
      }
      #img02 {
        position: relative;
        top: -50%;
        left: -30%;
      }
      #img03 {
        position: relative;
        top: -75%;
        left: -20%;
      }
      #img04 {
        position: relative;
        top: -90%;
        left: -20%;
      }
      #img05 {
        position: relative;
        top: -40%;
        left: -30%;
      }
      .subtitle {
        position: absolute;
        top: 40%;
        left: 20%;
        width: 20%;
        height: 20%;
        // top: 25%;
        // left: 25%;
        // width: 25%;
        // height: 25%; //layout ver.
        transition: opacity 0.3s;
        .txt {
          position: absolute;
          transform: translateY(-50%) rotate(-45deg);
          top: 50%;
          width: 100%;
          // font-size: 56px;
        }
      }
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
    height: 0% !important;
  }
  .left-right.slide{
    width: 0% !important;
  }
  .up-down {
    transition: height var(--duration) calc(var(--order) * 0.9 * var(--duration)) ease;
  }
  .left-right {
    transition: width var(--duration) calc(var(--order) * 0.9 * var(--duration)) ease;
  }
}
</style>