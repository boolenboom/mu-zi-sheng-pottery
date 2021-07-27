<template>
  <div id="app">
    <div id="nav" class="maincontainer">
      <router-link to="/" class="LOGO">
        M <span class="halftext"><span>uzi</span><span>Sheng</span></span>
      </router-link>|
      <div type="button" class="trigger" @mouseup.prevent="isActive = !isActive">{{`${isActive ? 'CLOSE' : 'MENU'}`}}</div>
      <menu :style="`--menuVisible:${isActive ? 'visible' : 'hidden'}`">
        <div class="maincontainer row">
          <div class="option col-6">
            <ul>
              <li><a href="#" class="link">Home</a></li>
              <li><a href="#" class="link">Studio</a></li>
              <li><a href="#" class="link">Pottery</a></li>
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
      </menu>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default {
  name:'app',
  data(){
    return{
      isActive:true,
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
  menu{
    visibility: var(--menuVisible);
    background-color: #fff;
    position: fixed;
      top:0;
      left: 0;
    width: 100%;
    height: 100vh;
    .option{
      position: relative;
      top: calc(60px + 2rem);
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
      top: calc(60px + 2rem);
      padding-top: 30px;
      height: calc(100vh - 60px - 2rem);
      padding-bottom: 30px;
      box-sizing: border-box;
      text-align: end;
      picture{
        img{
          height: 100%;
        }
      }
    }
  }
}
</style>
