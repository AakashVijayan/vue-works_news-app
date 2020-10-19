<template>
  <div>
    <News :api_key="apiKey" />
  </div>
</template>

<script>
import News from './components/News';


export default {
  name: 'App',
  components: {
  News
  },data(){
    return{
      apiKey:'36986e8a49224def9c74948e232a5b16'
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
