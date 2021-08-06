<template>
  <div id="app">
    <div id="nav" class="spring-container">
      <router-link to="/" class="LOGO fz-s" @mouseup.self.native='updateScrollOffset(1),isActive = false'>
        M <span class="halftext"><span>uzi</span><span>Sheng</span></span>
      </router-link>
      <div type="button" class="trigger fz-s" @mouseup.prevent="isActive = !isActive">{{`${isActive ? 'Close' : 'MENU'}`}}</div>
      <menu :class="{'menuHidden':!isActive}">
        <div class="spring-container row">
          <div class="option col-6">
            <ul>
              <li><router-link to="/#main" class="link fz-m" @mouseup.self.native='updateScrollOffset(1),isActive = !isActive'>Home</router-link></li>
              <li><router-link to="/#main" class="link fz-m" @mouseup.self.native='updateScrollOffset(2),isActive = !isActive'>Studio</router-link></li>
              <li><router-link to="/#main" class="link fz-m" @mouseup.self.native='updateScrollOffset(3),isActive = !isActive'>Pottery</router-link></li>
              <li><a href="#" class="link fz-m">Contact</a></li>
            </ul>
            <!-- <router-link to="/about">About</router-link> -->
          </div>
          <div class="randomobj col-6">
            <picture>
              <img src="./assets/pottery/pottery_01.jpg" alt="這是一件精心製作的作品">
            </picture>
          </div>
        </div>
        <div class="background animation-active" :class="{'animation-ready-pour':!isActive}"></div>
      </menu>
      <!-- <div class="ruler"></div> -->
    </div>
    <router-view :componentIndex='componentIndex'/>
  </div>
</template>

<script>
export default {
  name:'app',
  data(){
    return{
      isActive:false,
      componentIndex:1,
    }
  },
  methods:{
    updateScrollOffset(val){
      console.log('updateIndex');
      this.componentIndex = val;
    }
  }
}
</script>


<style lang="scss">
$pc: 1440px;
$table: 768px;
$mobile: 360px;

* {
    padding: 0;
    margin: 0;
    text-decoration: none;
    --Indentation: 4vw;
    --fz-xl:calc(5rem + 4vw);
    --fz-m:calc(3rem + 3vh);
    --fz-s:calc(1rem + 1.5vh);
}

 :root {
    --BG-color: #fff;
    --main-color: #200; 
    --secondary-color: #c28842;
}
html{
  height: 100vh;
  overflow-x: hidden;
}
body{
  height: 100vh;
  overflow: hidden;
}
section {
  // scroll-snap-align: start;
  width: 100%;
  height: 100vh;
  position: relative;
  text-align: center;
}
.spring-container{
  padding-left: var(--Indentation);
  padding-right: var(--Indentation);
  width: 100%;
  max-width: 177.778vh;
  margin: 0 auto;
  right: 0;
  left: 0;
  box-sizing: border-box;
}
.fixed-container {
    position: relative;
    margin: 0 auto;
    height: 100vh;
    width: 177.778vh;
    @media (max-width: 177.77vh) {
        margin: 0 calc((100% - 177.778vh) / 2);
    }
}
[class*="Rect"] {
        background-color: var(--secondary-color);
        position: absolute;
}
// html{
//   scroll-snap-type: y mandatory;
// }

.fz-xl{
  font-size: var(--fz-xl);
}
.veritcal-write{
  writing-mode: vertical-lr;
}
.fz-m{
  font-size: var(--fz-m);
}
.fz-s{
  font-size: var(--fz-s);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.row{
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: space-between;
}
.col-6{
  flex: 6 0 0;
}
.ruler{
  position: absolute;
  top:120px;
  left: 0;
  width: 100%;
  height: 0px;
  outline: 1px solid #000;
  z-index: 1001;
}

#nav {
  padding-top: 30px;
  padding-bottom: 30px;
  position: fixed;
  z-index: 900;
  // width: 100%;
  // box-sizing: border-box;
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-between;
  align-items: flex-start;
  a {
    flex: 0 0 0;
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
  .LOGO{
    display: flex;
    flex-flow: row nowrap;
    z-index: 901;
    font-size: 2em;
    .halftext{
      display: flex;
      flex-flow: column;
      font-size: 0.5em;
      span{
        flex: 1 0 0;
        height: 40%;
        &:nth-child(2){
          margin-top: -0.5rem;
        }
      }
    }
  }
  .trigger{
    z-index: 901;
    &:focus{
      color: #42b983;
    }
    cursor: pointer;
  }
  .menuHidden{
    opacity: 0;
    visibility: hidden;
    transition: opacity .4s linear,visibility 0s .4s linear;
  }
  menu{
    position: fixed;
      top:0;
      left: 0;
    width: 100%;
    height: 0vh;
    opacity: 1;
    visibility: visible;
    transition: opacity .4s linear;
    .option{
      position: relative;
      top: calc(60px + 2rem);
      left: 0px;
      padding-top: 30px;
      ul{
        list-style-type: none;
        // width: 30%;
        li{
          text-align: start;
          // font-size: 6rem;
          height: calc(6rem - 6px);
          &:not(:first-child){
            margin-top: 5vh;
          }
        }
      }
    }
    .randomobj{
      position: relative;
      top: calc(60px + 2rem + 18px);
      right: 0px;
      padding-top: 30px;
      height: calc(100vh - 60px - 2rem - 18px);
      padding-bottom: 30px;
      box-sizing: border-box;
      text-align: end;
      picture{
        img{
          height: 100%;
        }
      }
    }
    .background{
      background-color: var(--secondary-color);
      width: 100vh;
      height: 100vh;
      position: fixed;
        top: 0%;
        right: 0%;
      z-index: -1;
      transform: translateY(-50%) translateX(50%) scale(5);
      border-radius: 50%;
    }
    .animation-active{
      transition: transform .5s linear;
    }
    .animation-ready-pour{
      transform: translateY(-50%) translateX(50%) scale(.1) ;
    }
  }
}
</style>
