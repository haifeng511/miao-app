<template>
	<view class="food-container">
		<view class="food-image-container" v-if="food.image != '' ">
			<image class="food-image" :mode="aspectFill" :src="food.image"></image>
		</view>
		<view class="food-eat-info">
			<view>{{food.child}}奶猫</view>
			<view>幼年</view>
			<view>成年</view>
			<view>老年</view>
		</view>
		<view class="food-content">
			<view class="food-title">
				{{food.food}}
			</view>
			<view class="food-text">
				{{food.content}}
			</view>
		</view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from  '../../../../constant/constant.js'
	export default {
		data() {
			return {
				foodId:'',
				food:{}
			}
		},
		mounted() {
			this.getEatFoodDetail();
		},
		methods: {
			getEatFoodDetail(){
				uni.request({
					url: `${BASEURL}selectCatEatById`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						id: this.foodId
					},
					success: (res) => {
						let resp = res.data.data;
						this.food = resp;
				
					},
					fail() {
				
					}
				});
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option); //打印出上个页面传递的参数。
			this.foodId = option.foodId;
		}
	}
</script>

<style lang="scss">
	.food-container{
		padding: 10px;
		.food-image-container{
			height: 210px;
			border-radius: 10px;
			.food-image{
				width: 100%;
				height: 210px;
			}
		}
		
		.food-eat-info{
			display: flex;
			justify-content: space-around;
		}
		
		.food-content{
			margin: 20px 0;
			.food-title{
				font-size: 20px;
				font-weight: 700;
				margin: 10px 0;
			}
			.food-text{
				
			}
		}
	}

</style>
