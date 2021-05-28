<template>
	<view>
		<!-- 图片展示 -->
		<view class="top-img">
			<image v-if="isDefault" :src="imageUrl +image"></image>
			<image v-else :src="image"></image>
		</view>

		<!-- 选择图片及猫咪识别 -->
		<view class="center">
			<text class="selece-img" v-if="isShow">请选择图片！</text>
			<button @click="imgSelect()">选择图片</button>
			<button class="identity-btn" @click="catIdentify()">猫咪识别</button>
		</view>

		<!-- 识别结果 -->
		<view class="bottom" v-if="result.length != 0">
			<view class="title">
				<text>猫咪名称</text>
				<text>识别率</text>
			</view>
			<view class="info" v-for="item in result" :key="index">
				<text>{{item.name}}</text>
				<text>{{item.score}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	// 官网获取的 API Key 百度云应用的AK---->API Key)
	const clientId = "9SHvD5X2tg655c7848B5PqTR";
	// 官网获取的 Secret Key 百度云应用的AK---->Secret Key)
	const clientSecret = "z69H3LF5RfgbaicMak4sZiZKKRFGGhBb";
	import {
		BASRIMAGEURL
	} from '../../../constant/constant.js';
	export default {
		data() {
			return {
				isDefault: true, //是否默认图片
				isShow: false,
				imageUrl: BASRIMAGEURL,
				image: 'cat-identity.png',
				base64Img: '',
				token: '',
				result: []
			}
		},
		methods: {
			//图片转为base64
			getB64ByUrl(url) {
				let _this = this;
				uni.getFileSystemManager().readFile({
					filePath: url,
					encoding: 'base64',
					success: (res) => {
						_this.base64Img = res.data;
					}
				})
			},
			//选择图片按钮
			imgSelect() {
				this.isShow = false;
				this.isDefault = false;
				let _this = this;
				uni.chooseImage({
					count: 1,
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album'], //从相册选择
					success: function(res) {
						const tempFilePaths = res.tempFilePaths[0];
						console.log(JSON.stringify(res.tempFilePaths));
						_this.getB64ByUrl(tempFilePaths)
						_this.image = tempFilePaths;
						console.log(_this.image);

					}
				});

			},
			//猫咪识别
			catIdentify() {
				if (!this.base64Img) {
					this.isShow = true;
					return
				}
				this.getToken()
			},

			//获取token
			getToken() {
				uni.showLoading({
					title: '识别中...',
				})
				uni.request({
					url: `https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}`,
					success: (res) => {
						const token = res.data.access_token
						this.getResult(token)
					}
				})
			},
			//获取识别结果
			getResult(token) {
				uni.request({
					url: 'https://aip.baidubce.com/rest/2.0/image-classify/v1/animal?access_token=' + token,
					method: 'POST',
					data: {
						image: this.base64Img
					},
					header: {
						'Content-Type': 'application/x-www-form-urlencoded'
					},
					success: (res) => {
						console.log(res);
						this.result = this.resultFilter(res.data.result);
					},
					complete: () => {
						uni.hideLoading();
						uni.showToast({
							title: '识别成功',
							duration: 2000
						});
					}
				})
			},
			//result结果过滤
			resultFilter(arr) {
				arr.forEach((item) => {
					item.score = (parseFloat(item.score).toFixed(4) * 100).toFixed(2) + '%'
				})
				return arr
			},
		}
	}
</script>

<style lang="scss">
	.top-img {
		margin: 20rpx;
		width: 95%;
		height: 440rpx;
		// border: 1rpx solid #efefef;
		border-radius: 20rpx;
		box-shadow: 1rpx 1rpx 15rpx #ddd;
		overflow: hidden;
	}

	.top-img image {
		width: 100%;
		height: 100%;
	}

	.center {
		margin: 20rpx;
		width: 95%;
	}

	.selece-img {
		font-size: 26rpx;
		color: red;
	}

	.identity-btn {
		background-color: #ffd054;
		color: #FFFFFF;
	}

	.center button {
		margin-top: 30rpx;
	}

	.bottom {
		margin: 20rpx;
		width: 95%;
		height: 400rpx;
		border: 1rpx solid #efefef;
		border-radius: 20rpx;
		box-shadow: 1rpx 1rpx 15rpx #ddd;
	}

	.title {
		display: flex;
		justify-content: space-around;
		margin-top: 20rpx;
	}

	.title text {
		font-weight: 700;
		font-size: 32rpx;
	}

	.info {
		display: flex;
		justify-content: space-around;
		margin-top: 20rpx;
	}
</style>
