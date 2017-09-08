<template>
  <div id="app" class="clearFix">
    <VleftNav></VleftNav>
    <div class="main">
    	 <router-view></router-view>
    </div>
  </div>
</template>

<script>
import VleftNav from '@/components/VleftNav'
export default {
  name: 'app',
  components:{
  	VleftNav
  }
}
</script>

<style>
#app {
  font-family: 'Microsoft YaHei','Avenir', sans-serif;
  text-align: center;
  color: #2c3e50;
  height:100%;
}
.main{
  float:left;
  width:95%; 
  background-color: #EFF2F7;
  height:100%;
  overflow: auto;
 
}
</style>
