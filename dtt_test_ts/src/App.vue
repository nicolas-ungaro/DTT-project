<template>
  <div id="app">
    <Navbar />
    <Movies />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Movies from './components/Movies.vue';
import Navbar from "./components/Navbar.vue"
import axios from "axios";
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'


@Component({
  components: {
    Movies,
    Navbar

  },

})



export default class App extends Vue {}
</script>

<style>
#app {
  font-family: 'Dosis', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100%;
  padding-top:50px;
  background: #000000;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #434343, #000000);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to left, #333333, #000000); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}
</style>
