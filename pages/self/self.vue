<template>
	<view>
		我的
		<button open-type="getUserInfo" v-on:click="login()">登录</button>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../constant/constant.js'
	export default {
		data() {
			return {

			}
		},
		methods: {
			login() {
				uni.login({
					provider: 'weixin',
					success: function(loginRes) {
						// console.log(loginRes);
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
									// 获取用户信息
									uni.getUserInfo({
										provider: 'weixin',
										success: function(infoRes) {
											// console.log(infoRes);
											// console.log(infoRes.userInfo);
											console.log('用户昵称为：' + infoRes.userInfo.nickName);
									
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

<style>

</style>
