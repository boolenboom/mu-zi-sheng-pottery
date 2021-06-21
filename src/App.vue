<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view />
  </div>
</template>

<style lang="scss">
html{
  overflow-x: hidden;
}
body{
  overflow: hidden;
}
html,body{
  height: 300vh;
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

    &:nth-child(1){
      margin-right: 80%;
    }
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
