<template>
  <div>
    <Header />
    <div id="app">
      <SearchDomain msg="Welcome to Search Domain App" />
    </div>
    <Footer />
  </div>
</template>

<script>
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
