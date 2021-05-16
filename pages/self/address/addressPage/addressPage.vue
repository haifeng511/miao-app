<template>
	<view class="address-container">
		<view class="address-item" v-for="(address,index) in addressList" :key="index">
			<view class="address">
				<view class="contact">
					<text class="name">{{address.name}}</text>
					<text>{{address.phone}}</text>
				</view>
				<view class="address-detail">{{address.area}}{{address.detail}}</view>
			</view>
			<view class="edit">
				<uni-icons type="compose" size="26" color="#cdcdcd" @click="toEditAddress(address)"></uni-icons>
				<uni-icons type="trash" size="26" color="#fdd538" @click="deleteAddress(address)"></uni-icons>
			</view>
		</view>
		<view class="add-address"><button class="add-address-btn" @click="toAddAddress()">新增地址</button></view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../../constant/constant.js'
	export default {
		data() {
			return {
				userid:'',
				addressList:[]
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
						_this.getAddressList();
					}
				});
			},
			toAddAddress() {
				// 新增地址
				uni.navigateTo({
					url: '/pages/self/address/addAddress/addAddress',
				});
			},
			toEditAddress(address){
				let str = JSON.stringify(address);
				// 编辑地址
				uni.navigateTo({
					url: `/pages/self/address/addAddress/addAddress?address=${str}`,
				});
			},
			deleteAddress(address){
				let id = Number(address.id);
				console.log(id)
				uni.request({
					url: `${BASEURL}selectAddressById?id=${id}`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'POST',
					success: (res) => {
						let resp = res.data.data;
						if(resp == 1){
							uni.showToast({
							    title: '删除地址成功！',
								icon:'success',
							    duration: 2000
							});
							this.getAddressList();
						}
					},
					fail() {
						
					}
				});
			},
			getAddressList(){
				uni.request({
					url: `${BASEURL}selectAllAddressByUserId`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						"userId":this.userid,
					},
					success: (res) => {
						let resp = res.data.data;
						this.addressList = resp;
					},
					fail() {
						
					}
				});
			}
		}
	}
</script>

<style lang="scss">
.address-container{
	background-color: #f5f5f5;
	width: 100%;
	height: 100vh;
	
	.add-address {
		position: fixed;
		bottom: 40px;
		width: 100%;
		text-align: center;
	
		.add-address-btn {
			background-color: #fdd538;
			text-align: center;
			width: 180px;
			height: 50px;
			line-height: 50px;
			border-radius: 25px;
			color: #333333;
	
		}
	}
	
	.address-item{
		background-color: #FFFFFF;
		display: flex;
		margin: 10px;
		padding: 10px;
		align-items: center;
		justify-content: space-between;
		.address{
			.contact{
				margin: 5px 0;
				.name{
					padding-right: 15px;
				}
			}
			.address-detail{
				white-space: nowrap; 
				 overflow: hidden;
				 text-overflow:ellipsis;
			}
		}
		.edit{
			width: 80px;
			height: 40px;
			line-height: 40px;
			text-align: center;
		}
	}
}
</style>
