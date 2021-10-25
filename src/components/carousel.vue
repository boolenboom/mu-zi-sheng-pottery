<script>
let path = "assets/pottery";

let setting = require('../assets/pottery/list.json');
function scrollhandler() {
  let on = false;
  let speed = 0.8;
  let startpoint = 0;
  let offset = 0,
    currentoffset = 0,
    initoffset = 0;
  let min = 0,
    max = 0;
  let onecycle = 0;
  let clamp = function (min, val, max) {
      return Math.min(max, Math.max(min, val));
    },
    snap = function (val) {
      return Math.round(val / onecycle) * onecycle;
    },
  // damp = function(result,val,middle){
  //     let coefficient = (middle - result)/(val + result);
  //     console.log(coefficient);
  //     return val;
  // },
    start = function (val){
      on = true;
      startpoint = val;
    },
    doing = function (val){
      if (!on) return;
      currentoffset = (val - startpoint) * speed;
    },
    done = function (val){
      let temp = currentoffset;
      currentoffset = 0;
      on = false;
      offset += temp;
      offset = clamp(min, offset, max);
      offset = snap(offset);
      console.log("slide event end ,end point:", val);
    };
  return {
    mousedown: function (val) {
      start(val);
    },
    mousemove: function (val) {
      doing(val);
    },
    mouseup: function (val) {
      done(val);
    },
    touchstart: function (val){
      start(val);
    },
    touchmove: function (val){
      doing(val);
    },
    touchend: function (val){
      done(val);
    },
    touchcancel: function(val){
      done(val);
    },
    getStatus: function () {
      return [on, offset, startpoint, currentoffset, initoffset];
    },
    getoffset: function () {
      return -(initoffset + offset + currentoffset);
    },
    initial: function (obj) {
      initoffset = obj.initoffset;
      min = obj.min;
      max = obj.max;
      onecycle = obj.onecycle;
    },
  };
};
export default {
  name: "carousel",
  props: {
    sectiontitle: { type: String, default: "Intro" },
    quantity: { type: Number, default: 3 },
    countdown: { type: Number, default: 3 },
  },
  data() {
    return {
      UIcontroller: scrollhandler(),
      offset: 0,
      dataSrc: [],
    };
  },
  methods: {
    controller(e) {
      // let reg = new RegExp(/mouse/gm);
      // let methodName = (e.type.match(reg)).toString();
      let vueObj = this;
      requestAnimationFrame(function (){
        let offsetX = e.type.match('mouse') ? -e.x : -e.changedTouches[0].clientX;
        vueObj.UIcontroller[e.type](offsetX);
        vueObj.offset = vueObj.UIcontroller.getoffset();
      });
    },
    deviceReset(){
      let vueObj = this;
      requestAnimationFrame(function(){
        let setting = {
          initoffset: window.innerWidth > 360 ? -50 : -20,
          min: 0,
          max: window.innerWidth > 360 ? (vueObj.quantity - 1) * 140 : (vueObj.quantity - 1) * 120,
          onecycle: window.innerWidth > 360 ? 140 : 120,
        };
        vueObj.UIcontroller.initial(setting);
      })
    },
    fetchData() {
      console.log(setting.__fileNumber);
      
      for(let i = 1; i < setting.__fileNumber + 1; i++){
        this.dataSrc.push(`${i / 9 > 1 ? i : '0' + i}.jpg`);
      };
      console.log(this.dataSrc);
      // let fetchPath = '../assets/pottery/list.json';
      // console.log("ready fetch, path:", fetchPath);
      // axios
      //   .get(fetchPath)
      //   .then(function (res) {
      //     console.log("success", res.data.results);
      //   })
      //   .catch(function (err) {
      //     console.log("fetch fail, error msg:", err);
      //   });
    },
  },
  computed: {
    showImage() {
      let result = [];
      let temp = [...this.dataSrc];
      if (this.quantity > this.dataSrc.length) {
        result = this.dataSrc;
      }else{
        for(let count = 1; count < this.quantity + 1; count++){
          result.push(temp.splice(Math.floor(Math.random() * (setting.__fileNumber - count)),1));
        }
      };
      console.log('carousel img:',result);
      return result.map(element => {
        console.log(element);
        let obj = {
          filePath:require(`@/${path}/${setting.__fileNameFormat}${element}`),
        };
        return obj;
      });
    },
  },
  mounted() {
    let setting = {
      initoffset: window.innerWidth > 360 ? -50 : -20,
      min: 0,
      max: window.innerWidth > 360 ? (this.quantity - 1) * 140 : (this.quantity - 1) * 120,
      onecycle: window.innerWidth > 360 ? 140 : 120,
    };
    this.UIcontroller.initial(setting);
    this.offset = this.UIcontroller.getoffset();
    this.fetchData();
    window.addEventListener('resize',this.deviceReset);
    console.log("carousel mounted");
  },
};
</script>

<template>
  <div class="carousel">
    <div
      class="warpper"
      @mousedown.prevent.stop="controller($event)" @mousemove.prevent.stop="controller($event)" @mouseup.prevent.stop="controller($event)"
      @touchstart.stop='controller($event)' @touchmove.stop='controller($event)' @touchend.stop='controller($event)'
      @touchcancel.stop='controller($event)'>
      <div class="carousel-text">
        <div class="title text-xl veritcal-write">{{ sectiontitle }}</div>
      </div>
      <div class="carousel-contents">
        <router-link to="/works" class="contents-moreButton text-base">All</router-link>
        <ul class="carousel-contents-wrapper responsive-mt-base_reverse" :data-offset="offset" :style="`--offset:${offset}%;`">
          <li v-for="(item,index) of showImage" class="contents-item" :key="`card${index}`" draggable="false">
            <div class="contents-item-image">
              <img :src="item.filePath" alt="這是精心製作的陶器">
            </div>
            <p class="contents-item-text text-base">card {{ index + 1}}</p>
          </li>
        </ul>
      </div>
    </div>
    <!-- <div class="ruler"></div> -->
  </div>
</template>

<style lang="scss" scoped>
@import "~@/assets/scss/_variables.scss";
@import "~@/assets/scss/_mixins.scss";
.carousel {
  position: relative;
  top:50%;
  transform: translateY(-50%);
  z-index: 101;
  width: 100%;
  .warpper {
    width: 100%;
    display: flex;
    flex-flow: row nowrap;
    .carousel-text{
      flex: 0 0 0;
    }
    .carousel-contents{
      flex: 0 0 100%;
      width: 100%;
      margin-left: calc(var(--text-xl) * -0.75);
      text-align: end;
    }
  }
  .title {
    line-height: .75;
    letter-spacing: -16px;
    @include pad-width-containFollowing{
      letter-spacing: initial;
    }
    font-variant-caps: petite-caps;
  }
  .contents-moreButton {
    font-variant-caps: petite-caps;
  }
  .carousel-contents-wrapper {
    // margin-top: calc(var(--text-base));
    width: 100%;
    display: flex;
    flex-wrap: nowrap;
    text-align: center;
    .contents-item {
      list-style-type: none;
      background-color: var(--shadow-color);
      display: flex;
      flex-flow: column nowrap;
      align-items: center;
      justify-content: space-evenly;
      flex: 0 0 50%;
      @include phone-width{
        flex: 0 0 100%;
      }
      padding: 16px;
      transform: translateX(var(--offset));
      transition: transform 0.3s cubic-bezier(0.1, 0.7, 0.5, 1);
      width: 50%;
      .contents-item-image {
        min-width: 192px;
        max-height: 575px;
        width: 100%;
        @include pad-width-containFollowing{
          height: 500px;
          overflow-x: hidden;
          img{
            height: 100%;
            max-width: unset;
            transform: translateX(-14%) translateY(100px);
          }
        }
        @include phone-width{
          img{
            height: 100%;
            max-width: unset;
            transform: translateX(-35%) translateY(20px);
          }
        }
        // border-radius: 12px;
        overflow-y: hidden;
      }
      .contents-item-text {
        width: 100%;
      }
      &:not(:first-child) {
        margin-left: 20%;
      }
    }
  }
}
</style>
