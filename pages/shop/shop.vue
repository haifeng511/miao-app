<template>
	<view class="shop-container">
		<view class="search-btn" @click="toSearchGoods()">
			<uni-icons type="search" color="#464646" size="18"></uni-icons>
			<text class="search-text">请输入商品名称</text>
		</view>
		<view class="swiper-containner">
			<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
				:duration="duration">
				<swiper-item v-for="good in swiperGoods" :key=index>
					<image class="swiper-item" :mode="aspectFill" :src="good.image"></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="mid-nav">
			<view class="mid-nav-text">
				<view class="icon-text">
					<uni-icons type="checkbox" color="#a2a2a2" size="18"></uni-icons>
					<text>正品保证</text>
				</view>
				<view class="icon-text">
					<uni-icons type="headphones" color="#a2a2a2" size="18"></uni-icons>
					<text>售后无忧</text>
				</view>
				<view class="icon-text">
					<uni-icons type="info" color="a2a2a2" size="18"></uni-icons>
					<text>假一赔三</text>
				</view>
			</view>
			<view class="mid-nav-icon">
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="nav-image" :mode="aspectFill" :src="imageUrl+ 'goods/goods-category-1.png'"></image>
					<view>精选主粮</view>
				</view>
				<view class="image-text" @click="toCategoryGoods(1)">
					<image class="nav-image" :mode="aspectFill" :src="imageUrl+ 'goods/goods-category-2.png'"></image>
					<view>美味零食</view>
				</view>
				<view class="image-text" @click="toCategoryGoods(2)">
					<image class="nav-image" :mode="aspectFill" :src="imageUrl+ 'goods/goods-category-4.png'"></image>
					<view>营养保健</view>
				</view>
				<view class="image-text" @click="toCategoryGoods(3)">
					<image class="nav-image" :mode="aspectFill" :src="imageUrl+ 'goods/goods-category-3.png'"></image>
					<view>玩具用品</view>
				</view>
			</view>
		</view>
		<view class="goods-list">
			<view class="goods-like">
				<view class="like-title">
					<uni-icons type="heart-filled" color="#ff6363" size="18"></uni-icons>
					<text>猜你喜欢</text>
				</view>

			</view>
		</view>
		<goods :goodsList="goodsList"></goods>
		<view class="addCart" @click="toGoodCart()">
			<view class="add-icon">
				<uni-icons type="cart" size="23" color="#ffffff"></uni-icons>
				<!-- 	<view>购物车</view> -->
			</view>
		</view>
		<view class="isOver" v-if="isOver">页面到底了</view>
	</view>
</template>

<script>
	import {
		BASEURL,
		BASRIMAGEURL
	} from '../../constant/constant.js';
	import goods from './goods.vue';
	export default {
		components: {
			goods
		},
		data() {
			return {
				imageUrl: BASRIMAGEURL,
				swiperGoods: [{
					"image": BASRIMAGEURL + 'goods/bg-intro.png'
				}],
				goodsList: [],
				page: 1,
				isOver: false
			}
		},
		mounted() {
			this.getGoodsList();
		},
		methods: {
			toGoodCart() {
				// 跳转到购物车
				console.log('跳转到购物车')
				uni.navigateTo({
					url: `/pages/shop/cartPage/cartPage`,
				});
			},
			toCategoryGoods(category) {
				// 跳转到分类商品页面
				uni.navigateTo({
					url: `/pages/shop/categoryGoods/categoryGoods?category=${category}`,
				});
			},
			toSearchGoods() {
				// 跳转到搜索商品页面
				uni.navigateTo({
					url: `/pages/shop/goodsSearch/goodsSearch`,
				});
			},
			getGoodsList() {
				let oldGoodsList = this.goodsList;
				uni.request({
					url: `${BASEURL}selectGoodsPage`,
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
						this.goodsList = [...this.goodsList, ...resp];
						console.log(this.goodsList)
					},
					fail() {
						this.goodsList = oldGoodsList;
					}
				});
			},
		},
		onPullDownRefresh() {
			console.log('refresh');
			this.page = 1;
			this.goodsList = [];
			this.getGoodsList();
			// 停止刷新
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},
		onReachBottom() {
			if (this.goodsList.length < this.page * 6) {
				return this.isOver = true;
			}
			this.page = this.page + 1;
			this.getGoodsList();

		}
	}
</script>

<style lang="scss">
	.shop-container {
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

		.swiper-containner {
			width: 100%;

			.swiper {
				height: 160px;

				.swiper-item {
					width: 100%;
					display: block;
					height: 160px;
					line-height: 160px;
					text-align: center;
					background-color: #f4f1f2;
				}
			}
		}

		.mid-nav {
			margin-top: 10px;

			.mid-nav-text {
				width: 100%;
				display: inline-flex;
				justify-content: space-around;
				color: #a2a2a2;
				font-size: 12px;

				.icon-text {
					display: flex;
					align-items: center;
				}
			}

			.mid-nav-icon {
				width: 100%;
				display: inline-flex;
				justify-content: space-around;
				font-size: 16px;

				.image-text {
					padding: 10px 0;

					.nav-image {
						width: 60px;
						height: 60px;
						border-radius: 30px;
						background-color: #C0C0C0;
					}
				}
			}
		}


		.addCart {
			background-color: #ffca2c;
			width: 54px;
			height: 54px;
			border-radius: 27px;
			text-align: center;
			position: fixed;
			bottom: 50px;
			right: 20px;
			display: flex;
			align-items: center;
			justify-content: center;
			box-shadow: 3px 3px 2px #d9d9d9;

			.add-icon {
				color: #FFFFFF;
				font-size: 14px;
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
