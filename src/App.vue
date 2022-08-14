<template>
	<div class="app-container">
		<!-- 头部区域 -->
		<Header title="购物车案例"></Header>
		
		<!-- 商品区域 --  需要渲染每一个商品信息-->
		<Goods v-for="item in list" :key="item.id"
		:id="item.id"
		:title="item.goods_name"
		:pic="item.goods_img"
		:price="item.goods_price"
		:state="item.goods_state"
		:count="item.goods_count"
		@state-change="getNewstate"
		></Goods>

<!-- 底部 -->
		<Footer :isfull="fullState" :amount="amt" :sum="total" @full-change="getFullState"></Footer>
	</div>
</template>

<script>
// 导入axios请求库
import axios from "axios";


// 导入需要的组件
// 头部
import Header from "@/components/Header/Header.vue";
// 商品
import Goods from "@/components/Goods/Goods.vue";
// 底部
import Footer from "@/components/Footer/Footer.vue";

import bus from '@/components/eventBus.js'
export default {
	data() {
		return {
			//用来存储购物车的列表数据，默认为空数组
			list: [],
		};
	},

	created() {
		//调用请求数据的方法
		this.initCartList();
		
		bus.$on('share',(val) =>{
			this.list.some(item =>{
				if(item.id === val.id){
					item.goods_count = val.value
					return true
				}
			})
		})
	},
	methods: {
		// 封装请求列表数据的方法
		async initCartList() {
			const { data: res } = await axios.get("https://www.escook.cn/api/cart");
			// 只要请求回来的数据，在页面渲染期间要用到，则必须转存到data中
			console.log(res);
			if (res.status === 200) {
				this.list = res.list;
			}
		},

		//接受子组件传过来的选项状态
		getNewstate(val){
			this.list.some(item =>{
				if(item.id === val.id){
					item.goods_state = val.value
				}
			})

		},

		// 接受footer子组件传递过来的全选按钮的状态
		getFullState(check){
			this.list.forEach(item => item.goods_state = check)
		}
	},

	computed:{
		//动态计算全选的状态是 true 还是 false
		//循环每一项都等于true返回的就是true，否则就是false
		fullState(){
			return this.list.every(item => item.goods_state === true)
		},

    // 已经勾选商品的总价格
	 amt(){
			// 1.先filter过滤
			// 2. 在计算
				return this.list.filter(item => item.goods_state === true).reduce((total,item)=>{
				return total += item.goods_price * item.goods_count
			},0)

		},

//已经勾选商品的总价格
   total(){
		 return this.list.filter(item => item.goods_state === true).reduce((sum,item)=>{
			 return sum += item.goods_count
		 },0)
	 }

	},

	components: {
		Header,
		Goods,
		Footer,
	},
};
</script>

<style lang="less" scoped>
.app-container {
	padding-top: 45px;
	padding-bottom: 50px;
}
</style>
