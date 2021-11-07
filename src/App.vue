<template>

  <Navbar/>
  <router-view/>
  
</template>

<script>
import Navbar from './components/Navbar.vue'

export default{

  components: { Navbar}

}
</script>

<style>

html{
  font-size: 14px;
}
body{
  margin: 0;
  padding: 0;
  background: #f2f2f2;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 600px;
  margin: 0 auto;
  color: #555;
}


</style>
