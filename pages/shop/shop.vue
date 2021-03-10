<template>
	<view class="shop-container">
		<view class="search-btn" @click="toSearchGoods()">
			<uni-icons type="search" color="#464646" size="18"></uni-icons>
			<text class="search-text">请输入商品名称</text>
		</view>
		<view class="swiper-containner">
			<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
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
				<view class="image-text">
					<image class="nav-image" :mode="aspectFill" :src="1"></image>
					<view>精选主粮</view>
				</view>
				<view class="image-text">
					<image class="nav-image" :mode="aspectFill" :src="1"></image>
					<view>美味零食</view>
				</view>
				<view class="image-text">
					<image class="nav-image" :mode="aspectFill" :src="1"></image>
					<view>营养保健</view>
				</view>
				<view class="image-text">
					<image class="nav-image" :mode="aspectFill" :src="1"></image>
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
	</view>
</template>

<script>
	export default {
		data() {
			return {
				swiperGoods: [],
			}
		},
		methods: {
			toSearchGoods() {
				// 跳转到搜索商品页面
			}
		}
	}
</script>

<style lang="scss">
	.shop-container {
		padding: 15px 20px;

		.search-btn {
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
	}
</style>
