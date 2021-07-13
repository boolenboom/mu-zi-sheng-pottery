<template>
  <div id="app">
    <div id="nav">
      <router-link to="/" class="LOGO">
        M <span class="halftext"><span>uzi</span><span>Sheng</span></span>
      </router-link> |
      <menu>
        <div class="trigger">MENU</div>
        <div class="option">
          <div class="trigger">CLOSE</div>
          <router-link to="/about">About</router-link>
        </div>
      </menu>
    </div>
    <router-view/>
  </div>
</template>

<script>
// export default {
//   name:'app',
// }
// </script>


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

#nav {
  padding: 30px;
  position: fixed;
  z-index: 20;
  width: calc(100% - 60px);
  display: flex;
  flex-flow: row nowrap;
  font-size: 2rem;
  a {
    flex: 1 0 0;
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
  .LOGO{
    display: flex;
    flex-flow: row nowrap;
    justify-content: center;
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
    margin-right: 80%;
  }
  menu{
    --menuVisible: hidden;
    .trigger{
      z-index: 901;
      &:focus{
        color: #42b983;
      }
    }
    .option{
      position: fixed;
      top:0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 900;
      background-color: #fff;
      visibility: var(--menuVisible);
    }
  }
}
</style>
