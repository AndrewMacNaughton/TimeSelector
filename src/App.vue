<template>
  <div id="app">
    <time-selector-component>09:00</time-selector-component>
  </div>
</template>
<script>
import TimeSelectorComponent from './TimeSelectorComponent'
export default {
  name:'App',
  components:{
    TimeSelectorComponent
  }
}
</script>
<style lang="css">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  margin-top:60px;
  padding:20px;
  background-color: #eee;
  justify-content: center;
}
.pointer {
  cursor: pointer;
}
</style>
