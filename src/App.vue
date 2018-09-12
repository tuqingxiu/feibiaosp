<template>
  <div id="app">
    <transition>
      <keep-alive>
        <router-view></router-view>
      </keep-alive>
    </transition>
    
  </div>
</template>

<script>
// import { mapState, mapMutations } from "vuex";
import Tool from "./utils/Tool";
export default {
  name: 'App',
  computed: {
    
  },
  methods: {
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 750px;
  min-width: 320px;
  min-height: 100vh;
  margin: auto;
  position: relative;
}
</style>
