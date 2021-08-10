<script>
export default {
  name: "app",
  data() {
    return {
      isActive: false,
      componentIndex: 1,
    };
  },
  methods: {
    updateScrollOffset(val) {
      console.log("updateIndex");
      this.componentIndex = val;
    },
  },
};
</script>

<template>
  <div id="app">
    <div id="nav" class="nav spring-container">
      <router-link to="/" class="nav-logo text-base" @mouseup.self.native="updateScrollOffset(1), (isActive = false)">
        M <span class="nav-logo-half-text"><span>uzi</span><span>Sheng</span></span>
      </router-link>
      <div type="button" class="menu-trigger text-base" @mouseup.prevent="isActive = !isActive">
        {{ `${isActive ? "Close" : "MENU"}` }}
      </div>
      <menu :class="{ 'menu-hidden': !isActive }">
        <div class="spring-container row">
          <div class="option col-6">
            <ul>
              <li>
                <router-link
                  to="/#main"
                  class="link text-base"
                  @mouseup.self.native="
                    updateScrollOffset(1), (isActive = !isActive)
                  "
                  >Home</router-link
                >
              </li>
              <li>
                <router-link
                  to="/#main"
                  class="link text-base"
                  @mouseup.self.native="
                    updateScrollOffset(2), (isActive = !isActive)
                  "
                  >Studio</router-link
                >
              </li>
              <li>
                <router-link
                  to="/#main"
                  class="link text-base"
                  @mouseup.self.native="
                    updateScrollOffset(3), (isActive = !isActive)
                  "
                  >Pottery</router-link
                >
              </li>
              <li><a href="#" class="link text-base">Contact</a></li>
            </ul>
            <!-- <router-link to="/about">About</router-link> -->
          </div>
          <div class="random-obj col-6">
            <picture>
              <img
                src="./assets/pottery/pottery_01.jpg"
                alt="這是一件精心製作的作品"
              />
            </picture>
          </div>
        </div>
        <div
          class="background animation-active"
          :class="{ 'animation-ready-pour': !isActive }"
        ></div>
      </menu>
      <!-- <div class="ruler"></div> -->
    </div>
    <router-view :componentIndex="componentIndex" />
  </div>
</template>

<style lang="scss">
@import "~@/assets/scss/_variables.scss";
@import "~@/assets/scss/_mixins.scss";
*,*::before,*::after {
  padding: 0;
  margin: 0;
  text-decoration: none;
  box-sizing: border-box;
}
*{
  @include pc-width {
    --text-xl: 14rem;
    --text-l: 8rem;
    --text-base: 4rem;
    --text-s: 2rem;
  };
  @include pad-width {
    --text-xl: 12rem;
    --text-l: 6rem;
    --text-base: 3rem;
    --text-s: 1.5rem;
  };
  @include small-pad-width {
    --text-xl: 8rem;
    --text-l: 4rem;
    --text-base: 2rem;
    --text-s: 1rem;
  };
  @include phone-width {
    --text-xl: 6rem;
    --text-l: 4rem;
    --text-base: 2rem;
    --text-s: 1rem;
  };
}
:root {
  --BG-color: #fff;
  --main-color: #200;
  --secondary-color: #c28842;
  --Indentation: 4%;
}

.h-100 {
  height: 100%;
}
.vh-100 {
  height: 100vh;
}

html {
  height: 100vh;
  overflow-x: hidden;
}
body {
  height: 100vh;
  overflow: hidden;
}
section {
  position: relative;
}
img {
  max-width: 100%;
  height: auto;
}

.spring-container {
  padding-left: var(--Indentation);
  padding-right: var(--Indentation);
  position: absolute;
  width: 100%;
  max-width: 177.778vh;
  margin: 0 auto;
  right: 0;
  left: 0;
}
.fixed-container {
  position: relative;
  margin: 0 auto;
  width: 177.778vh;
  @media all and (max-width: 177.77vh) {
    margin: 0 calc((100% - 177.778vh) / 2);
  }
}

[class*="Rect"] {
  background-color: var(--secondary-color);
  position: absolute;
}
.text-xl {
  font-size: var(--text-xl);
}
.text-l {
  font-size: var(--text-l);
}
.text-base {
  font-size: var(--text-base);
}
.text-s {
  font-size: var(--text-s);
}
.veritcal-write {
  writing-mode: vertical-lr;
}
.responsive-writeMode{
  @include responsive-writeMode(vertical-lr,horizontal-tb);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.row {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: space-between;
}
.col-6 {
  flex: 6 0 0;
}

.nav {
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
  .nav-logo {
    display: flex;
    flex-flow: row nowrap;
    z-index: 901;
    .nav-logo-half-text {
      font-size: 0.5em;
      span {
        display: block;
        line-height: 1;
      }
    }
  }
  .menu-trigger {
    z-index: 901;
    &:focus {
      color: #42b983;
    }
    cursor: pointer;
  }
  .menu-hidden {
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.4s linear, visibility 0s 0.4s linear;
  }
  menu {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 0vh;
    opacity: 1;
    visibility: visible;
    transition: opacity 0.4s linear;
    .option {
      position: relative;
      top: calc(60px + 2rem);
      left: 0px;
      padding-top: 30px;
      ul {
        list-style-type: none;
        li {
          text-align: start;
          height: calc(6rem - 6px);
          &:not(:first-child) {
            margin-top: 5vh;
          }
        }
      }
    }
    .random-obj {
      position: relative;
      top: calc(60px + 2rem + 18px);
      right: 0px;
      padding-top: 30px;
      height: calc(100vh - 60px - 2rem - 18px);
      padding-bottom: 30px;
      box-sizing: border-box;
      text-align: end;
      picture {
        img {
          height: 100%;
        }
      }
    }
    .background {
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
    .animation-active {
      transition: transform 0.5s linear;
    }
    .animation-ready-pour {
      transform: translateY(-50%) translateX(50%) scale(0.1);
    }
  }
}
</style>
