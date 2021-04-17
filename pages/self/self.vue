<template>
	<view>
	<view v-if="toLogin">
		<button open-type="getUserInfo" v-on:click="login()">登录</button>
	</view>
	<view  v-if="getUser" class="self-container">
		<view class="userinfo-container">
			<image class="user-image" :mode="aspectFill" :src="avator"></image>
			<text class="user-name"> {{username}}</text>
		</view>
		<view class="gray-line"></view>
		<view class="self-list">
			<view class="self-text">个人中心</view>
			<view class="self-list-icon">
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="self-icon" :mode="aspectFill" :src="img"></image>
					<view>我的动态</view>
				</view>
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="self-icon" :mode="aspectFill" :src="img"></image>
					<view>我的问答</view>
				</view>
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="self-icon" :mode="aspectFill" :src="img"></image>
					<view>地址管理</view>
				</view>
			</view>
		</view>
		<view class="gray-line"></view>
		<view class="self-list">
			<view class="self-text">我的订单</view>
			<view class="self-list-icon">
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="self-icon" :mode="aspectFill" :src="img"></image>
					<view>购物车</view>
				</view>
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="self-icon" :mode="aspectFill" :src="img"></image>
					<view>待付款</view>
				</view>
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="self-icon" :mode="aspectFill" :src="img"></image>
					<view>待发货</view>
				</view>
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="self-icon" :mode="aspectFill" :src="img"></image>
					<view>待收货</view>
				</view>
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="self-icon" :mode="aspectFill" :src="img"></image>
					<view>售后服务</view>
				</view>
			</view>
		</view>
		<view class="gray-line"></view>
		<view class="self-list">
			<view class="self-text">更多功能</view>
			<view class="self-list-icon">
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="self-icon" :mode="aspectFill" :src="img"></image>
					<view>宠物识别</view>
				</view>
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="self-icon" :mode="aspectFill" :src="img"></image>
					<view>意见投诉</view>
				</view>
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="self-icon" :mode="aspectFill" :src="img"></image>
					<view>关于我们</view>
				</view>
				<view class="image-text" @click="toCategoryGoods(0)">
					<image class="self-icon" :mode="aspectFill" :src="img"></image>
					<view>退出登录</view>
				</view>
			</view>
		</view>
	</view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../constant/constant.js';
	export default {
		data() {
			return {
				toLogin:true,
				getUser:false,
				avator:'',
				username:'Alice'
			}
		},
		methods: {
			login() {
			let _this = this;
				uni.login({
					provider: 'weixin',
					success: function(loginRes) {
						console.log(loginRes);
						if (loginRes.code) {
							//发起网络请求
							uni.request({
								// 请求路径
								url: `${BASEURL}wxLogin`,
								// 请求参数code
								data: {
									code: loginRes.code
								},
								method: 'GET',
								success(res) {
									// 请求成功后获取openid和session_key
									console.log("用户openID",res.data.data)
									// console.log(_this.userinfo.username);
									// 获取用户信息
									uni.getUserInfo({
										provider: 'weixin',
										success: function(infoRes) {
											console.log(infoRes);
											// console.log(_this.userinfo.username);
											console.log('用户昵称为：' + infoRes.userInfo.nickName);
											_this.username = infoRes.userInfo.nickName;
											_this.avator = infoRes.userInfo.avatarUrl;
											_this.getUser = true;
											_this.toLogin = false;
									console.log(_this.username);
									
										},
										fail() {
											console.log("fail")
										}
									});
								}
							});
						}
					}
				});
			}
		}
	}
</script>

<style lang="scss">

.self-container{
	
	.userinfo-container{
		padding: 20px;
		background-color: #ffd054;
		.user-image{
			width: 90px;
			height: 90px;
			border-radius: 45px;
			background-color: #3F536E;
		}
		
		.user-name{
			font-size: 20px;
			font-weight: 700;
			
		}
	}
	
	.self-list{
		padding: 10px;
		.self-text{
			font-size: 20px;
			font-weight: 700;
			margin: 10px 0;
		}
		.self-list-icon{
			display: flex;
			justify-content: space-around;
			font-size: 16px;
			
			.image-text {
				padding: 10px 0;
				
				.self-icon {
					width: 60px;
					height: 60px;
					border-radius: 30px;
					background-color: #C0C0C0;
				}
			}
		}
	}
	
	.gray-line{
		height: 20px;
		background-color: #f5f5f5;
	}
}


</style>
