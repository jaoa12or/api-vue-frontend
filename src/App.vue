<template>
  <div>
    <Header />
    <div id="app">
      <!-- <img alt="Vue logo" src="./assets/logo.png" style="position: absolute"> -->
      <SearchDomain msg="Welcome to Your Vue.js App" />
    </div>
    <Footer />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import SearchDomain from "./components/SearchDomain";
import Header from "./components/Header";
import Footer from "./components/Footer"

export default {
  name: "App",
  components: {
    Header,
    SearchDomain,
    Footer
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #555;
  margin-top: 60px;
}
</style>
