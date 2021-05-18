<template>
	<view class="food-container">
		<view class="food-image-container" v-if="food.image != '' ">
			<image class="food-image" :mode="aspectFill" :src="food.image"></image>
		</view>
		<view class="food-eat-info">
		<view class="eat-content">
		<image v-if="food.child == 0" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/no.png'"></image>
		<image v-if="food.child == 1" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/warn.png'"></image>
		<image v-if="food.child == 2" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/yes.png'"></image>
		<text>奶猫</text>
		</view>
		<view class="eat-content">
		<image v-if="food.youth == 0" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/no.png'"></image>
		<image v-if="food.youth == 1" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/warn.png'"></image>
		<image v-if="food.youth == 2" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/yes.png'"></image>
		<text>幼年</text>
		</view>
		<view class="eat-content">
		<image v-if="food.adult == 0" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/no.png'"></image>
		<image v-if="food.adult == 1" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/warn.png'"></image>
		<image v-if="food.adult == 2" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/yes.png'"></image>
		<text>成年</text>
		</view>
		<view class="eat-content">
		<image v-if="food.old == 0" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/no.png'"></image>
		<image v-if="food.old == 1" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/warn.png'"></image>
		<image v-if="food.old == 2" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/yes.png'"></image>
		<text>老年</text>
		</view>
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
		BASEURL,BASRIMAGEURL
	} from  '../../../../constant/constant.js'
	export default {
		data() {
			return {
				foodId:'',
				food:{},
				imageUrl:BASRIMAGEURL,
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
			border-radius: 15px;
			.food-image{
				width: 100%;
				height: 210px;
				border-radius: 15px;
			}
		}
		
		.food-eat-info{
			margin-top: 10px;
			display: flex;
			justify-content: space-around;
			.eat-content{
				display: flex;
				justify-content: space-around;
				align-items: center;
				.eat-icon{
					padding: 3px;
					width: 12px;
					height: 12px;
					border-radius: 50%;
				}
			}
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
