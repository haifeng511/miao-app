<template>
	<view class="content">
		<view class="logo-container">
			<image class="logo" :src="imageUrl+ 'order.png'"></image>
		</view>
		<view class="text-area">
			<view class="confirm-title">支付成功</view>
			<!-- <view class="title">支付成功</view> -->
			<button class="confirm-btn" @click="toShopCart()">确认</button>
		</view>
	</view>
</template>

<script>
	import {
		BASEURL,BASRIMAGEURL
	} from '../../../constant/constant.js'
	export default {
		data() {
			return {
				imageUrl:BASRIMAGEURL,
			}
		},
		onLoad() {
			

		},
		methods: {
			toShopCart(){
				// 跳转到购物车
				uni.navigateTo({
					url: `/pages/shop/cartPage/cartPage`,
				});
			}
		}
	}
</script>

<style lang="scss">
	.content {}

	.logo-container {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		margin: 120px 0 30px 0;
	}

	.logo {
		height: 120px;
		width: 120px;
	}

	.text-area {
		text-align: center;

	}

	.confirm-btn {
		width: 80%;
		background-color: #fbbc02;
		color: #FFFFFF;
		margin-top: 50px;
		border-radius: 20px;
	}

	.confirm-title {
		font-size: 36rpx;
		color: #666666;
		font-weight: 700;
		margin: 15px 0;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
