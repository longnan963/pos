<template>
  <div class="snack">
  	<el-row>
  		<el-col :span='7' class='snackOrder' id='orderList'>
  			<el-tabs>
  				<el-tab-pane label='点餐'>
  					<el-table border :data='tableData' style="width: 100%">
  						<el-table-column prop='goodsName' label='商品名称'></el-table-column>
  						<el-table-column prop='count' label='数量' width="70"></el-table-column>
  						<el-table-column prop='price' label='金额' width="70"></el-table-column>
  						<el-table-column  label='操作' fixed="right" width="100">
  							 <template scope="scope">
					            <el-button type="text" size="small" @click='delSingleGoods(scope.row)'>删除</el-button>
					            <el-button type="text" size="small" @click='addOrderList(scope.row)'>增加</el-button>
					        </template>
  						</el-table-column>
  					</el-table>
  					<div class="totalDiv">
  						<small>数量：</small>{{totalCount}} &nbsp&nbsp    <small>金额：</small>{{totalMoney}}元
  					</div>
  					<div class="divBtn">
  						<el-button type="warning" >挂单</el-button>
                        <el-button type="danger" @click='delAllGoods'>删除</el-button>
                        <el-button type="success" @click='checkOut'>结账</el-button>
  					</div>
  				</el-tab-pane>
  				<el-tab-pane label='挂单'>
  					挂单
  				</el-tab-pane>
  				<el-tab-pane label='外卖'>
  					外卖
  				</el-tab-pane>
  			</el-tabs>
  	    </el-col>
  	    <el-col :span='17'>
  			<div class="oftenGoods">
  				<div class="title">
  					常用商品
  				</div>
  				<div class="oftenGoods-list">
  					<ul class="clearFix" >
  						<li v-for='goods in oftenGoods' @click='addOrderList(goods)'>
  							<span>{{goods.goodsName}}</span>
  							<span class='o-price'>¥{{goods.price}}</span>
  						</li>
  					</ul>
  				</div>
  			</div>

  			<div class="goodType">
  				<el-tabs>
			        <el-tab-pane label="汉堡">
			           <div>
			           		<ul class='cookList'>
							    <li v-for='goods in type0Goods'  @click='addOrderList(goods)'>
							        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
							        <span class="foodName">{{goods.goodsName}}</span>
							        <span class="foodPrice">￥{{goods.price}}元</span>
							    </li>
							</ul>
			           </div>
			        </el-tab-pane>
		            <el-tab-pane label="小食">
		            	<div>
			           		<ul class='cookList'>
							    <li v-for='goods in type1Goods'  @click='addOrderList(goods)'>
							        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
							        <span class="foodName">{{goods.goodsName}}</span>
							        <span class="foodPrice">￥{{goods.price}}元</span>
							    </li>
							</ul>
			           </div>
			        </el-tab-pane>
			        <el-tab-pane label="饮料">
			           <div>
			           		<ul class='cookList'>
							    <li v-for='goods in type2Goods'  @click='addOrderList(goods)'>
							        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
							        <span class="foodName">{{goods.goodsName}}</span>
							        <span class="foodPrice">￥{{goods.price}}元</span>
							    </li>
							</ul>
			           </div>
			        </el-tab-pane>
			        <el-tab-pane label="套餐">
			            <div>
			           		<ul class='cookList'>
							    <li v-for='goods in type3Goods'  @click='addOrderList(goods)'>
							        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
							        <span class="foodName">{{goods.goodsName}}</span>
							        <span class="foodPrice">￥{{goods.price}}元</span>
							    </li>
							</ul>
			           </div>
			        </el-tab-pane>
 
   				 </el-tabs>
  			</div>
  	    </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Vsnack',
  data(){
	return{
		tableData:[],
        oftenGoods:[],
        type0Goods:[],
        type1Goods:[],
        type2Goods:[],
        type3Goods:[],
        totalMoney:0,
        totalCount:0
	}
  },
  created(){
      axios.get('http://jspang.com/DemoApi/oftenGoods.php')
      .then(response=>{
         this.oftenGoods=response.data;
      })
      .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
      });
      axios.get('http://jspang.com/DemoApi/typeGoods.php')
      .then(response=>{
         this.type0Goods=response.data[0];
         this.type1Goods=response.data[1];
         this.type2Goods=response.data[2];
         this.type3Goods=response.data[3];
 
      })
      .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
      });
  },
  mounted:function(){
	var orderListHeight = document.body.clientHeight;
	document.getElementById('orderList').style.height = orderListHeight+'px';
  },
  methods:{
  	addOrderList(goods){
		//商品是否在订单列表中
		let isHave = false;
		for(let i = 0;i<this.tableData.length;i++){
			if(this.tableData[i].goodsId == goods.goodsId){
				isHave = true;
			}
		}
		if(isHave){
			//改变列表中商品的数量
			let arr = this.tableData.filter(o=>o.goodsId==goods.goodsId);
			arr[0].count++;
		}else{
			let newGoods = {
				goodsId:goods.goodsId,
				goodsName:goods.goodsName,
				price:goods.price,
				count:1
			}
			this.tableData.push(newGoods);
		}
		this.getAllMoney();
  	},
  	//模拟结账
  	checkOut(){
  		if(this.totalCount!=0){
			this.tableData = [];
	  		this.totalMoney = 0;
			this.totalCount = 0;
			this.$message({
				message:'结账成功，又为店里贡献了一份力量',
				type:'success'
			});
  		}else{
  			this.$message.error('不能空结，老板了解你急切的心情！！');
  		}
  	},
  	//计算所有的金额
  	getAllMoney(){
  		this.totalMoney = 0;
		this.totalCount = 0;
		if(this.tableData){
			this.tableData.forEach((ele)=>{
				this.totalCount = ele.count;
				this.totalMoney += ele.price*ele.count;
			});
		}
  	},
  	//删除单个商品
  	delSingleGoods(goods){
		this.tableData = this.tableData.filter(o=>o.goodsId!=goods.goodsId);
		this.getAllMoney();
  	},
  	//删除所有商品
  	delAllGoods(){
  		this.tableData = [];
  		this.totalMoney = 0;
		this.totalCount = 0;
  	}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.snackOrder{background-color: #f9fafc;border-right: 1px solid #c0ccda;}
.divBtn{margin-top: 10px;}
.title{height: 20px;border: 1px solid #D3DCE6;background-color: #F9FAFC;padding: 10px;text-align: left;}
.oftenGoods-list ul li{float:left;border:1px solid #E5E9F2;padding:10px;margin:5px;background-color:#fff;cursor: pointer;}
.o-price{color:#58B7FF;}
.cookList li{list-style: none;width:23%;border:1px solid #E5E9F2;height: auto;overflow: hidden;
	background-color:#fff;padding: 2px;float:left; margin: 2px;}
.cookList li span{display: block; float:left;cursor: pointer;}
.foodImg{width: 40%;}
.foodName{font-size: 18px;padding-left: 10px; color:brown;}
.foodPrice{font-size: 16px;padding-left: 10px;padding-top:10px;}
.totalDiv{background-color: #fff;padding: 10px;border-bottom: 1px solid #E5E9F2; }
</style>
