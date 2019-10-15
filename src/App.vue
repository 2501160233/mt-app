<template>
  <div id="app">
  <!-- Header -->
  <ap-header :poiInfo="poiInfo"></ap-header>
  <!-- Nav -->
  <ap-nav></ap-nav>
  <!-- content -->
  <router-view></router-view>


  </div>
</template>

<script>
import Header from './components/header/Header'
import Nav from './components/nav/Nav'

export default {
  name: 'App',
  data(){
    return{
      poiInfo:{}
    }
  },
  components: {
    "ap-header":Header,
    "ap-nav":Nav
  },
  created(){
    // axios
    // ES6 fetch
    fetch("/api/goods")
    .then(res=>{
      return res.json()
    })
    .then(response=>{
     if(response.code==0){
       this.poiInfo = response.data.poi_info;
      //  console.log(this.poiInfo)
     }
    })
  }
}
</script>

<style>
.app{
  overflow: hidden;
}
</style>
