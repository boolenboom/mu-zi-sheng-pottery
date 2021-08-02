<template>
  <div id="app">
    <div id="nav" class="maincontainer">
      <router-link to="/" class="LOGO">
        M <span class="halftext"><span>uzi</span><span>Sheng</span></span>
      </router-link>|
      <div type="button" class="trigger" @mouseup.prevent="isActive = !isActive">{{`${isActive ? 'CLOSE' : 'MENU'}`}}</div>
      <menu :class="{'menuHidden':!isActive}">
        <div class="maincontainer row">
          <div class="option col-6">
            <ul>
              <li><a href="#Home" class="link">Home</a></li>
              <li><a href="#studioIntro" class="link">Studio</a></li>
              <li><a href="#potteryList" class="link">Pottery</a></li>
              <li><a href="#" class="link">Contact</a></li>
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
    <router-view/>
  </div>
</template>

<script>
export default {
  name:'app',
  data(){
    return{
      isActive:false,
    }
  }
}
</script>


<style lang="scss">
html{
  height: 100vh;
  overflow-x: hidden;
}
body{
  height: 100vh;
  overflow: hidden;
}
// html{
//   scroll-snap-type: y mandatory;
// }

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.maincontainer{
  padding-left: 4vw;
  padding-right: 4vw;
  width: 100%;
  box-sizing: border-box;
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
  font-size: 2rem;
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
    .halftext{
      display: flex;
      flex-flow: column;
      span{
        flex: 1 0 0;
        height: 40%;
        font-size: 1rem;
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
          font-size: 6rem;
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
      background-color: #000;
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
