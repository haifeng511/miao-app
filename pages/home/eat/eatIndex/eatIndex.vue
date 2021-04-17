<template>
	<view>
		<view class="eat-container">
			<view class="search-btn" @click="toSearchEatFoods()">
				<uni-icons type="search" color="#464646" size="18"></uni-icons>
				<text class="search-text">请输入商品名称</text>
			</view>
			<view class="mid-nav-icon">
				<view class="image-text" @click="toCategoryEatFood(7)">
					<image class="nav-image" :mode="aspectFill" :src="img"></image>
					<view>水产海鲜</view>
				</view>
				<view class="image-text" @click="toCategoryEatFood(6)">
					<image class="nav-image" :mode="aspectFill" :src="img"></image>
					<view>肉蛋类</view>
				</view>
				<view class="image-text" @click="toCategoryEatFood(5)">
					<image class="nav-image" :mode="aspectFill" :src="img"></image>
					<view>奶豆制品</view>
				</view>
				<view class="image-text" @click="toCategoryEatFood(4)">
					<image class="nav-image" :mode="aspectFill" :src="img"></image>
					<view>蔬菜</view>
				</view>
				<view class="image-text" @click="toCategoryEatFood(3)">
					<image class="nav-image" :mode="aspectFill" :src="img"></image>
					<view>水果</view>
				</view>
				<view class="image-text" @click="toCategoryEatFood(2)">
					<image class="nav-image" :mode="aspectFill" :src="img"></image>
					<view>零食甜点</view>
				</view>
				<view class="image-text" @click="toCategoryEatFood(0)">
					<image class="nav-image" :mode="aspectFill" :src="img"></image>
					<view>主食</view>
				</view>
				<view class="image-text" @click="toCategoryEatFood(1)">
					<image class="nav-image" :mode="aspectFill" :src="img"></image>
					<view>饮料</view>
				</view>
			</view>
			<view class="gray-line"></view>
			<view class="read-text">推荐阅读</view>
			<eatfoodItem :foods="foods"></eatfoodItem>
			<view class="isOver" v-if="isOver">页面到底了</view>
		</view>
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
				isOver: false,
				foods: [],
				page: 1,
			}
		},
		mounted() {
			this.getFoodsList();
		},
		methods: {
			toCategoryEatFood(category) {
				// 跳转到分类能不能吃页面
				uni.navigateTo({
					url: `/pages/home/eat/eatFoodsCategory/eatFoodsCategory?category=${category}`,
				});
			},
			toSearchEatFoods() {
				// 跳转到搜索能不能吃页面
				uni.navigateTo({
					url: `/pages/home/eat/eatFoodSearch/eatFoodSearch`,
				});
			},
			getFoodsList() {
				let oldfoods = this.foods;
				uni.request({
					url: `${BASEURL}selectCatEatPage`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						page: this.page
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
		},
		onReachBottom() {
			console.log('页面触底了')
			if (this.foods.length < this.page * 6) {
				return this.isOver = true;
			}
			this.page = this.page + 1;
			this.getFoodsList();
		},
	}
</script>

<style lang="scss">
	.eat-container {
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

			.mid-nav-icon {
				width: 100%;
				display: flex;
				justify-content: space-between;
				flex-wrap: wrap;
				font-size: 16px;

				.image-text {
					padding: 10px;
					text-align: center;
					.nav-image {
						width: 60px;
						height: 60px;
						border-radius: 30px;
						background-color: #C0C0C0;
					}
				}
			}
		

		.gray-line{
				height: 20px;
				background-color: #f5f5f5;
			}
		.read-text {
			margin: 10px 0;
			font-size: 20px;
			font-weight: 700;
		}

		.isOver {
			width: 100%;
			text-align: center;
			color: #999999;
			margin: 10px;
		}
	}
</style>
