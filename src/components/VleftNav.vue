<template>
  <div class="leftNav">
    <ul>
    	<li>
    		<i class='icon iconfont icon-dianpu'></i>
    		<div>店铺</div>
    	</li>
    	<li>
    		<i class='icon iconfont icon-goumai'></i>
    		<div>收银</div>
    	</li>
    	<li>
    		<i class='icon iconfont icon-hanbao'></i>
    		<div>商品</div>
    	</li>
    	<li>
    		<i class='icon iconfont icon-huiyuanqia'></i>
    		<div>会员</div>
    	</li>
    	<li>
    		<i class='icon iconfont icon-gongnengjianyi'></i>
    		<div>设置</div>
    	</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'VleftNav',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.leftNav{
	color:#fff;
	font-size: 10px;
	height: 100%;
	background-color: #1d8ce0;
	float: left;
	width: 5%;
}
.iconfont{
	font-size: 24px;
}
.leftNav li{
	text-align: center;
	border-bottom: 1px solid #2a20ff;
	padding: 10px;
}
</style>
