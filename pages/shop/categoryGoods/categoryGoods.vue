<template>
	<view class="category-containner">
		<view class="search-btn" @click="toSearchGoods()">
			<uni-icons type="search" color="#464646" size="18"></uni-icons>
			<text class="search-text">请输入商品名称</text>
		</view>
		<view class="category-orderby">
			<text @click="categoryGoodsOrderby()" class="orderby-text">综合</text>
			<text @click="categoryGoodsOrderby()" class="orderby-text-checked">销量</text>
			<text @click="categoryGoodsOrderby()" class="orderby-text">价格</text>
		</view>
		<goods :goodsList = "goodsList"></goods> 
		<view class="isOver" v-if="isOver">页面到底了</view >
	</view> 
</template>

<script>
	import {
		BASEURL
	} from '../../../constant/constant.js';
	import goods from '../goods.vue'
	export default {
		components: {
			goods
		},
		data() {
			return {
				goodsList:[],
				page: 1,
				isOver: false,
				category:''
			}
		},
		mounted() {
			this.getGoodsList();
		},
		methods: {
			toSearchGoods() {
				// 跳转到搜索商品页面
				uni.navigateTo({
					url: `/pages/shop/goodsSearch/goodsSearch`,
				});
			},
			categoryGoodsOrderby(){
				
			},
			getGoodsList(){
				let oldGoodsList = this.goodsList;
				uni.request({
					url: `${BASEURL}selectGoodsPage`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						page: this.page,
						category:this.category
					},
					success: (res) => {
						let resp = res.data.data;
						console.log(resp);
						this.goodsList = [...this.goodsList, ...resp];
						console.log(this.goodsList)
					},
					fail() {
						this.goodsList = oldGoodsList;
					}
				});
			},
			onReachBottom() {
				if (this.goodsList.length < this.page * 6) {
					return this.isOver = true;
				}
				this.page = this.page + 1;
				this.getGoodsList();
			
			},
			onLoad: function(option) { 
				this.category = option.category;
			}
		}
	}
</script>

<style lang="scss">
	.category-containner{
		padding: 10px;
		
		.search-btn {
			margin: 10px;
			padding: 0 10px;
			background-color: #f4f1f2;
			height: 30px;
			line-height: 30px;
			border-radius: 14px;
			display: flex;
			justify-content: center;
		
			.search-text {
				color: #d8d7d7;
			}
		}
		
		.category-orderby{
			display: flex;
			justify-content: space-around;
			padding: 10px;
			.orderby-text{
				font-size: 16px;
			}
			.orderby-text-checked{
				color: #ff984d;
				font-size: 16px;
			}
		}
		
		.isOver {
			width: 100%;
			text-align: center;
			color: #999999;
			margin: 10px;
		}
	}
</style>
