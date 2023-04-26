<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import Logo from './components/icons/Logo.vue'
import SearchIcon from './components/icons/Search.vue'
</script>

<script lang="ts">
import Header from './components/Header.vue'


export default {
  components: {
    Header,
  }
}
</script>
<template>
  <div class="wrapper">
    <Header />
    <RouterView />
    <Footer />
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Montserrat&family=Pathway+Extreme&family=Roboto&display=swap');

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: 'Roboto', 'Montserrat', 'Pathway Extreme', 'Arial', sans-serif;
}

h3 {
    font-size: 1.17em;
    font-weight: bold;
}

a {
  text-decoration: none;
}


html, body {
  height: 100vh;
  width: 100vw;
  background-color: #0f0f0f;
  overflow-x: hidden;
}
.wrapper {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}

.guide {
  display: flex;
  width: 240px;
  &-wrapper {
    width: 240px;
    background: transparent;
  }
  &-spacer {
    height: 64px;
  }
  &-content {
    &__main {
      width: 100%;
      display: flex;
      flex-direction: column;
      text-align: left;
      justify-content: center;
      font-size: 16px;
      color: white;
      font-weight: 500;

      hr {
        margin: 6px 0;
        color: red;
      }

      div {
        margin: auto;
        text-align: left;
        display: flex;
        justify-content: left;
        padding: 8px;
        border-radius: 12px;
        width: 90%;
        &.active {
          background-color: rgba(70,70,70,.5);
          &:hover {
            background-color: rgba(120,120,120,.5);
          }
        }
        &:hover {
          background-color: rgba(70,70,70,.5);
          cursor: pointer;
        }
      }
    }
  }
}
.flat {
  display: flex;
}
</style>
