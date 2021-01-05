<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <el-row :gutter="20">
      <el-col :span="6" :offset="6">
        <v-month v-model="monthObj" range-separator="-" ></v-month>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import vMonth from './components/Month.vue'
export default {
  name: 'App',
  components: {
    HelloWorld,
    vMonth
  },
  data(){
    return {
      monthObj:'2021-01,2021-10,2020-12,2020-05,2019-01,2021-06,2021-05'
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
