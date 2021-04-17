<template>
	<view class="food-category">
		<eatfoodItem :foods="foods"></eatfoodItem>
		<view class="isOver" v-if="isOver">页面到底了</view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../../constant/constant.js'
	import eatfoodItem from '../eatFoodItem.vue'
	export default {
		components: {
			eatfoodItem
		},
		data() {
			return {
				isOver:false,
				foods: [],
				page: 1,
				category:''
			}
		},
		mounted() {
			this.getFoodsList();
		},
		onReachBottom() {
			if (this.foods.length < this.page * 6) {
				return this.isOver = true;
			}
			this.page = this.page + 1;
			this.getFoodsList();
		
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option); //打印出上个页面传递的参数。
			this.category = option.category;
		},
		methods: {
			getFoodsList(){
					let oldfoods = this.foods;
					uni.request({
						url: `${BASEURL}selectCatEatPage`,
						headers: {
							'Content-Type': 'application/json'
						},
						method: 'GET',
						data: {
							page: this.page,
							category:this.category,
							limit:8
						},
						success: (res) => {
							let resp = res.data.data;
							console.log(resp);
							this.foods = [...this.foods, ...resp];
							console.log(this.foods)
						},
						fail() {
							this.foods = oldfoods;
						}
					});
				},
		}
	}
</script>

<style lang="scss">
	.food-category{
		width: 100%;
	}
	.isOver {
				width: 100%;
				text-align: center;
				color: #999999;
				margin: 10px;
	}
</style>
