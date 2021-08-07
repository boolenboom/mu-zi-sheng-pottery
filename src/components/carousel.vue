<template>
  <div class="carousel">
    <div
      class="warpper"
      @mousedown.prevent="controller($event)"
      @mousemove.prevent="controller($event)"
      @mouseup.prevent="controller($event)"
    >
      <div class="title text-l veritcal-write">{{ sectiontitle }}</div>
      <ul
        class="contents"
        :data-offset="offset"
        :style="`--offset:${offset}%;`"
      >
        <li v-for="(item,index) of showImage" class="item" :key="`card${index}`" draggable="false">
          <div class="pic">
            <img :src="item.filePath" alt="這是精心製作的陶器">
          </div>
          <p class="itemname text-s">card {{ index + 1}}</p>
        </li>
      </ul>
      <a href="#" class="button text-base">All</a>
    </div>
    <!-- <div class="ruler"></div> -->
  </div>
</template>
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
    };
  // damp = function(result,val,middle){
  //     let coefficient = (middle - result)/(val + result);
  //     console.log(coefficient);
  //     return val;
  // }
  return {
    mousedown: function (val) {
      on = true;
      startpoint = val;
    },
    mousemove: function (val) {
      if (!on) return;
      console.log("mousemove");
      currentoffset = (val - startpoint) * speed;
    },
    mouseup: function (val) {
      let temp = currentoffset;
      currentoffset = 0;
      on = false;
      offset += temp;
      offset = clamp(min, offset, max);
      offset = snap(offset);
      console.log("slide event end ,end point:", val);
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
      // console.log(e,this.moveOffset);
      this.UIcontroller[e.type](-e.x);
      this.offset = this.UIcontroller.getoffset();
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
        return result;
      };
      while (result.length < this.quantity) {
        if (this.quantity - result.length === temp.length){
          result.push(...temp);
          break;
        };
        if (Math.random() > 0.5) result.push(temp.shift());
        temp.shift();
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
      initoffset: -120,
      min: 0,
      max: (this.quantity - 1) * 140,
      onecycle: 140,
    };
    this.UIcontroller.initial(setting);
    this.offset = this.UIcontroller.getoffset();
    this.fetchData();
    console.log("carousel mounted");
  },
};
</script>

<style lang="scss" scoped>
.carousel {
  position: relative;
  z-index: 101;
  width: 100%;
  overflow-x: hidden;
  .title {
    top: 10%;
    left: -2%;
    position: absolute;
  }
  .button {
    position: absolute;
    top: 10%;
    right: 0;
  }
  .contents {
    margin-top: calc(var(--text-base) * 2);
    width: 100%;
    display: flex;
    flex-flow: row nowrap;
    // margin-left: clamp(0px, calc(20% + 5% - 16px + var(--offset)), calc(20% + 5% - 16px)) ;
    margin-left: calc(-1 * (50% - 15%));
    .item {
      list-style-type: none;
      flex: 50% 0 0;
      // height: calc(20vh + 300px);
      background-color: #000;
      padding: 16px;
      display: flex;
      flex-flow: column nowrap;
      align-items: center;
      justify-content: center;
      box-sizing: border-box;
      transform: translateX(var(--offset));
      transition: transform 0.3s cubic-bezier(0.1, 0.7, 0.5, 1);
      .pic {
        min-width: 192px;
        width: 100%;
        border-radius: 12px;
        background-color: #000;
      }
      .itmename {
        width: 100%;
        margin-top: 3rem;
      }
      &:not(:first-child) {
        margin-left: 20%;
      }
    }
  }
}
.warpper {
  width: 100%;
}
</style>
