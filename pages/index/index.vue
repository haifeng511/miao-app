<template>
	<view class="content">
		<view class="logo-container">
			<image class="logo" src="/static/logo.png"></image>
		</view>
		<view class="text-area">
			<view class="confirm-title">请确认以下授权信息：</view>
			<view class="title">获取昵称、头像等公开信息</view>
			<button class="confirm-btn" open-type="getUserInfo" v-on:click="login()">授权</button>
		</view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../constant/constant.js'
	export default {
		data() {
			return {}
		},
		onLoad() {
			let userInfo = {
				nickName: '',
				avatarUrl: ''
			};
			uni.getStorage({
				key: 'userInfo',
				success: function(res) {
					userInfo = res.data;
					if (userInfo.nickName !== '') {
						uni.switchTab({
							url: '/pages/home/home',
						});
					}
				}
			});

		},
		methods: {
			login() {
				let code = '';
				let encryptedData = '';
				let iv = '';

				uni.login({
					success: (res) => {
						code = res.code
					}
				});
				uni.getUserProfile({
					desc: '登录',
					success: async (res) => {
						console.log(res);
						encryptedData = res.encryptedData;
						iv = res.iv;
						let nickName = res.userInfo.nickName;
						let avatarUrl = res.userInfo.avatarUrl;
						let userInfo = {
							nickName,
							avatarUrl
						}
						uni.setStorage({
							key: 'userInfo',
							data: userInfo,
							success: function() {
								console.log('success');
							}
						});
						// 后端接口，发起网络请求
						// 解密获取手机号
						uni.request({
							url: `${BASEURL}decodeUser`,
							data: {
								code: code,
								iv: iv,
								encryptedData: encryptedData
							},
							method: 'GET',
							success(res) {
								uni.setStorage({
									key: 'user',
									data: res.data.data.user
								});
								if(res.data.data.num ==0){
									let wxid = res.data.data.openid;
									// 添加用户信息
									let user = {
										username: nickName,
										phone: '177',
										wxid: wxid,
										avatar: avatarUrl
									}
									console.log(user)
									uni.request({
										// 请求路径
										url: `${BASEURL}addUser`,
										// 请求参数code
										data:user,
										method: 'post',
										success(res) {
											// 添加用户成功
											uni.request({
												url: `${BASEURL}selectUserInfoByOPenid`,
												data:{
													openid:wxid
												},
												method: 'get',
												success(res) {
													// 添加用户成功
													uni.setStorage({
														key: 'user',
														data: res.data.data,
														success: function() {
															console.log('添加用户后，缓存user成功');
														}
													});
												}
											});
										}
									});
								}
								
							}
						});
						// 路由跳转到首页
						uni.switchTab({
							url: '/pages/home/home',
						});
					},
					fail: (res) => {
						console.log(res)
					}
				});

			},
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
		background-color: #2b9939;
		color: #FFFFFF;
		margin: 10px auto;
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
