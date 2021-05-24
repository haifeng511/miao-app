<template>
	<view class="content">
		<view class="data-container">
			<image class="no-data" :src="imageUrl+ 'null.png'"></image>
		</view>
		<view class="text-area">
			<view class="title">暂无数据</view>
		</view>
	</view>
</template>

<script>
	import {
		BASRIMAGEURL
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
		
		}
	}
</script>

<style lang="scss">
	.content {
		
	}

	.data-container {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		margin: 120px 0 30px 0;
	}

	.no-data {
		height: 120px;
		width: 220px;
	}

	.text-area {
		text-align: center;

	}

	.confirm-title {
		font-size: 36rpx;
		color: #666666;
		font-weight: 700;
		margin: 10px 0;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
