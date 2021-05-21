<template>
	<view class="add-container">
		<view class="address-detail-item">
			<text>收货人</text> <input class="address-input" v-model="name" placeholder="请输入收货人姓名" />
		</view>
		<view class="address-detail-item">
			<text>手机号码</text> <input class="address-input" v-model="phone" placeholder="请输入收货人手机号码" />
		</view>
		<view class="address-detail-item">
			<text>所在地区</text> <pickerAddress class="address-input" @change="change">{{area}}</pickerAddress>
		</view>
		<view class="address-detail">
			<view>详细地址</view> <textarea class="address-textarea" v-model="detail" placeholder="请输入收货人详细地址" />
		</view>
		<view class="confirm-address"><button class="confirm-address-btn" @click="addAddress()">保存地址</button></view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../../constant/constant.js'
	import pickerAddress from '../../../../components/wangding-pickerAddress/wangding-pickerAddress.vue'
	export default {
		components: {
			pickerAddress
		},
		data() {
			return {
				userid:'',
				name:'',
				phone:'',
				detail:'',
				area: '选择地址',
			}
		},
		mounted() {
			this.getUser();
		},
		methods: {
			getUser(){
				let _this = this;
				uni.getStorage({
					key: 'user',
					success: function(res) {
						_this.userid = res.data.id;
					}
				});
			},
			change(data) {
				this.area = data.data.join('')
			},
			addAddress(){
				uni.request({
					url: `${BASEURL}addAddress`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'POST',
					data: {
						"name":this.name,
						"phone":this.phone,
						"detail":this.detail,
						"area":this.area,
						"userId":this.userid,
						"ifDefault":1
					},
					success: (res) => {
						let resp = res.data.data;
						if(resp === 1){
							uni.showToast({
							    title: '添加地址成功！',
								icon:'success',
							    duration: 2000
							});
							setTimeout(function(){
								uni.navigateBack({
								    delta: 1
								});
							},2500)
						}
					},
					fail() {
						
					}
				});
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			if(option.address !== null){
				let address = JSON.parse(option.address);
				console.log(address);
				this.name = address.name;
				this.phone = address.phone;
				this.detail = address.detail;
				this.area = address.area;
				this.userid = address.userId;
			}
		}
	}
</script>

<style lang="scss">
	.add-container{
		width: 750rpx;
		min-height: 100vh;
		background-color: #f5f5f5;
	}
	.address-detail-item{
		background-color: #FFFFFF;
		padding: 8px;
		margin: 0 8px;
		height: 40px;
		line-height: 40px;
		border-bottom:  1px solid #cccccc;
		display: flex;
		align-items: center;
		.address-input{
			padding-left: 10px;
			width: 80%;
		}
	}
	
	.address-detail{
		background-color: #FFFFFF;
		padding: 8px 0;
		margin: 0 8px;
		height: 100px;
		.address-textarea{
			margin-top: 8px;
		}
	}
	
	.confirm-address {
		position: fixed;
		bottom: 50px;
		width: 100%;
		text-align: center;
	
		.confirm-address-btn {
			background-color: #fdd538;
			text-align: center;
			width: 280px;
			height: 50px;
			line-height: 50px;
			border-radius: 25px;
			color: #333333;
	
		}
	}
</style>
