<template>
	<view>
		<view class="shop-car">
			<view class="common-car">
				<view class="shop-car">
					<view class="shop-car-header">
						<text>确定订单</text>
					</view>
					<view class="address-container"> 
						<view class="address-item" v-if="ifAddress">
							<view class="address">
								<view class="contact">
									<text class="name">{{address.name}}</text>
									<text>{{address.phone}}</text>
								</view>
								<view class="address-detail">{{address.area}}{{address.detail}}</view>
							</view>
							<view class="edit">
								<uni-icons type="compose" size="26" color="#cdcdcd" @click="toEditAddress(address)"></uni-icons>
							</view>
						</view>
						<view v-else @click="toAddAddress()" class="add-address-text">
							<text class="add-text">点击添加地址</text>
							</view>
					</view>
					<view class="store-box" v-for="(itemq,indexq) in datas" :key="indexq">
						<view class="store-header">
							<text>{{itemq.name}}</text>
						</view>
						<view class="goodsInfo" v-for="(itemw,indexw) in itemq.goodsList" :key="indexw" >
							<view class="goodsInfo-right" >
								<image :src="itemq.orderList[indexw].image" class="goods-image" mode="" @click="goodsDetail(itemw.id)"></image>
								<view class="goodsInfo-box">
									<text class="goods-name" @click="goodsDetail(itemw.id)">{{itemw.title}}</text>
									<view class="goods-box">
										<text class="goods-price">¥{{itemw.salePrice}}</text>
									</view>
								</view>
							</view>
						</view>
					</view>
					<view class="statistics-box">
						<view class="statistics">
							<view class="statistics-right"  @tap="accounts">
								<text>运费：</text><text class="text-color">¥{{freightPrice}}</text>
								<text>总计：</text><text class="text-color">¥{{total}}</text>
								<view class="btn">
									<text>结算</text>
								</view>
							</view>
						</view>
						<view class="gap"></view>
					</view>
				</view>
				<slot></slot>
			</view>

			<!-- <image src="" mode=""></image> -->
		</view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../constant/constant.js'
	export default {
		data() {
			return {
				userid: '',
				// goodsProducts: [],
				datas: {},
				statisticsIndex: false,
				total: 0,
				isCut: true,
				orderList:[],
				freightPrice:0,
				address:{},
				ifAddress:false,
			}
		},
		mounted() {
			this.getUser();
		},
		methods: {
			toAddAddress() {
				// 新增地址
				uni.navigateTo({
					url: '/pages/self/address/addAddress/addAddress',
				});
			},
			getDefaultAddress(){
				uni.request({
					url: `${BASEURL}selectDefaultAddresss`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						userId: this.userid,
					},
					success: (res) => {
						let resp = res.data.data;
						console.log('-------------')
						console.log(resp)
						if(resp != null){
							this.ifAddress = true;
						}
						this.address = resp;
						
					
					},
					fail() {
				
					}
				});
			},
			getCheckOrder(){
				//查看立即下单的订单信息
				uni.request({
					url: `${BASEURL}selectOrderCategory`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						userId: this.userid,
						state: 1,
						orderList: this.orderList
					},
					success: (res) => {
						let resp = res.data.data;
						this.datas = resp;
						this.statistics();
					},
					fail() {
				
					}
				});
			},
			goodsDetail(goodsId){
				uni.navigateTo({
					url: `/pages/shop/goodsDetail/goodsDetail?goodsId=${goodsId}`,
				});
			},
			getUser() {
				let _this = this;
				uni.getStorage({
					key: 'user',
					success: function(res) {
						_this.userid = res.data.id;
						_this.getCheckOrder();
						_this.getDefaultAddress();
					}
				});
			},
			//统计
			statistics() {
				let total = 0;
				let freightPrice = 0;
				this.datas.find((item, index) => {
					item.goodsList.find((itemq, indexq) => {
							freightPrice = freightPrice +  itemq.freightPrice * item.orderList[indexq].orderNum;
							total = total + (itemq.salePrice + itemq.freightPrice) * item.orderList[indexq].orderNum;
						
					})
				})
				this.total = total.toFixed(2);
				this.freightPrice = freightPrice.toFixed(2);
			},
			accounts() {
				let judge = this.judgeSelect();
				if (judge.length > 0) {
						console.log(judge)
						//修改order的状态
						// uni.request({
						// 	url: `${BASEURL}updateOrderListState`,
						// 	headers: {
						// 		'Content-Type': 'application/json'
						// 	},
						// 	method: 'POST',
						// 	data: judge,
						// 	success: (res) => {
						// 		let resp = res.data.data;
						// 		console.log('==============')
						// 	},
						// 	fail() {
						
						// 	}
						// });
						
					
				} else {
					uni.showToast({
						title: '您当前未选择任何商品,结算失败',
						icon: 'none'
					})
				}
			},
			judgeSelect() {
				// let judge = false
				let selectArr = [];
				this.datas.find((item, index) => {
					item.goodsList.find((itemq, indexq) => {
						// if (itemq.checked == 2) {
							// judge = true
							selectArr.push(item.orderList[indexq])
						// }
					})
				})
				// return judge
				return selectArr
			},
		},
		onLoad(option) {
			this.orderList = JSON.parse(option.orderList);
			this.getUser();
		}
	}
</script>

<style lang="scss" scoped>
	.add-address-text{
		margin: 10px 0;
		width: 750rpx;
		height: 60px;
		background-color: #FFFFFF;
		display: flex;
		align-items: center;
		justify-content: center;
		
		.add-text{
			padding: 10px 30px;
			border:  3px dotted #fbbc02;
		}
	}
	.address-container{
		width: 750rpx;
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
	
	.shop-car {
		width: 750rpx;
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		background: #F5F5F5;
	}


	.common-car {
		width: 750rpx;
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		background: #F5F5F5;
	}



	.shop-car {
		width: 750rpx;
		display: flex;
		flex-direction: column;
		align-items: center;

		.shop-car-header {
			width: 690rpx;
			height: 80rpx;
			display: flex;
			flex-direction: row;
			align-items: center;
			justify-content: flex-end;

			text {
				font-size: 28rpx;
				font-weight: 400;
				color: #313133;
			}
		}

		.store-box {
			width: 750rpx;
			margin-bottom: 20rpx;
			display: flex;
			flex-direction: column;
			align-items: center;
			background-color: #FFFFFF;

			.store-header {
				width: 690rpx;
				height: 78rpx;
				padding: 0 30rpx;
				display: flex;
				flex-direction: row;
				align-items: center;
				font-size: 28rpx;
				font-weight: 400;
				color: #313133;


				text {
					font-size: 28rpx;
					font-weight: 400;
					color: #313133;
					margin-left: 20rpx;
				}

				.label {
					width: 14rpx;
					height: 24rpx;
					margin-left: 10rpx;
					margin-top: 5rpx;
				}
			}

			.goodsInfo {
				width: 690rpx;
				height: 246rpx;
				display: flex;
				flex-direction: row;
				align-items: center;
				border-bottom: 2rpx solid #EDEDED;

				&:nth-last-child(1) {
					border: none;
				}

				.goodsInfo-right {
					width: 634rpx;
					height: 246rpx;
					margin-left: 20rpx;
					display: flex;
					flex-direction: row;
					align-items: center;

					.goods-image {
						width: 180rpx;
						height: 180rpx;
					}

					.goodsInfo-box {
						width: 428rpx;
						margin-left: 24rpx;
						display: flex;
						flex-direction: column;
						align-items: center;

						.goods-name {
							width: 428rpx;
							font-size: 26rpx;
							font-weight: 400;
							color: #313133;
							text-overflow: -o-ellipsis-lastline;
							overflow: hidden;
							text-overflow: ellipsis;
							display: -webkit-box;
							-webkit-line-clamp: 2;
							line-clamp: 2;
							-webkit-box-orient: vertical;
						}

						.spe {
							width: 428rpx;
							margin-top: 10rpx;
							font-size: 24rpx;
							font-weight: 400;
							color: #919298;
						}

						.goods-box {
							width: 428rpx;
							margin-top: 18rpx;
							display: flex;
							flex-direction: row;
							align-items: center;
							justify-content: space-between;

							.goods-price {
								font-size: 32rpx;
								font-weight: 400;
								color: #313133;
							}

							.goods-num-box {
								width: 168rpx;
								height: 46rpx;
								display: flex;
								flex-direction: row;
								align-items: center;

								.goods-image {
									width: 46rpx;
									height: 44rpx;
									text-align: center;
									line-height: 44rpx;
									border: 1rpx solid #CFCFCF;
									font-size: 38rpx;
								}

								.goods-num {
									width: 76rpx;
									height: 44rpx;
									text-align: center;
									line-height: 44rpx;
									font-size: 33rpx;
									font-weight: 400;
									color: #666666;
									border-top: 1px solid #CFCFCF;
									border-bottom: 1px solid #CFCFCF;
								}
							}
						}
					}
				}
			}
		}

		.statistics-box {
			width: 750rpx;
			display: flex;
			flex-direction: column;
			align-items: center;
			background-color: #FFFFFF;
			position: fixed;
			bottom: 0;

			.statistics {
				width: 750rpx;
				padding: 0 0 0 30rpx;
				height: 98rpx;
				display: flex;
				flex-direction: row;
				align-items: center;
				justify-content: space-between;

				.statistics-left {
					width: 120rpx;
					display: flex;
					flex-direction: row;
					align-items: center;
					justify-content: space-between;

					image {
						width: 36rpx;
						height: 36rpx;
					}

					text {
						font-size: 30rpx;
						font-weight: 400;
						color: #313133;
					}
				}

				.statistics-right {
					// width: 600rpx;
					width: 750rpx;
					display: flex;
					flex-direction: row;
					align-items: center;
					justify-content: space-between;

					.btn {
						width: 218rpx;
						height: 98rpx;
						background: #FBBC02;
						text-align: center;
						line-height: 98rpx;
						font-size: 30rpx;
						font-weight: 400;
						color: #313133;
						margin-left: 40rpx;
					}

					text {
						font-size: 30rpx;
						font-weight: 400;
						color: #313133;
					}

					.text-color {
						color: rgba(242, 18, 18, 1);
					}
				}
			}

			.gap {
				width: 750rpx;
				height: 40rpx;
			}
		}
	}
</style>
