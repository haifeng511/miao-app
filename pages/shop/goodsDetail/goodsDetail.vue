<template>
	<view class="goods-detail-containner">
		<view class="swiper-containner">
			<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
				<swiper-item v-for="carousel in carousels" :key=index>
					<image class="swiper-item" :mode="aspectFill" :src="carousel.image"></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="goods-detail">
			<view class="goods-price">
				<text class="new-price">￥{{goods.salePrice}}</text>
				<text class="old-price">￥{{goods.referencePrice}}</text>
			</view>
			<view class="goods-title-content">
				<text class="goods-title">{{goods.title}}</text>
				<view class="goods-share"><uni-icons type="redo" size="18"></uni-icons>分享</view>
			</view>
			<view class="gray-line"></view>
			<view class="detail-pop">
				<view class="goods-freight"><text class="param-text">运费</text>		{{goods.freightPrice}}</view>
				<view class="goods-param" @click="goodsParamPop()">
					<text class="param-text">参数</text>		品牌名称 净含量
					<uni-icons class="arrow-right" type="arrowright" size="18"></uni-icons>
				</view>
				<view class="goods-service" @click="goodsServicePop()">
					<text class="param-text">服务</text>		
					<uni-icons type="checkbox" size="18" color="#ff8d8d"></uni-icons>正品保证 
					<uni-icons type="checkbox" size="18" color="#ff8d8d"></uni-icons>包邮 
					<uni-icons type="checkbox" size="18" color="#ff8d8d"></uni-icons>七天无理由退换
					<uni-icons class="arrow-right" type="arrowright" size="18"></uni-icons>
				</view>
			</view>

			<view class="gray-line"></view>
			<view class="goods-shop">
				<image class="shop-image" :mode="aspectFill" :src="1"></image>
				<view class="shop-name">
					商铺名称
				</view>
				<view class="shop-btn">
					进店逛逛
					<uni-icons type="arrowright" size="18"></uni-icons>
				</view>
			</view>
			<view class="gray-line"></view>
			<view class="goods-rich-text">
				<view class="goods-detail">商品详情</view>
				<rich-text :nodes="goods.detail"></rich-text>
			</view>
			<view class="price-intro-container">
				<view class="price-intro-title">价格说明:</view>
				<view class="price-intro">划线价格：商品展示的划线价格为该商品的参考价格，指商品的正品零售价、厂商指导价或改商品曾经展示过的销售价等，并非远嫁，仅供参考使用</view>
				<view class="price-intro">未划线价格：指该商品在喵喵商城上的实时销售价，具体的成交价格根据该商品参加的活动或会员使用优惠券、红包、积分等发生变化，最终以订单结算页面价格为准</view>
			</view>
		</view>

		<uni-popup ref="paramPopup" type="bottom">
			<view class="paramPopup">
				<view class="title">产品参数</view>
				<view class="pop-line">
					<text class="pop-text">适用对象</text>
					<text>{{goods.applicableCat}}</text>
				</view>
				<view class="pop-line">
					<text class="pop-text">品牌</text>
					<text>{{goods.brand}}</text>
				</view>
				<view class="pop-line">
					<text class="pop-text">生产地</text>
					<text>{{goods.productPlace}}</text>
				</view>
				<button class="confirm-btn" @click="goodsParamClose()">确定</button>
			</view>
		</uni-popup>

		<uni-popup ref="servicePopup" type="bottom">
			<view class="servicePopup">
				<view class="title">服务说明</view>
				<view class="service-item">
					<uni-icons type="checkbox" size="18" color="#ff8d8d"></uni-icons>正品保证
				</view>
				<view class="service-detail">全场正品，假一赔三</view>
				<view class="service-item">
					<uni-icons type="checkbox" size="18" color="#ff8d8d"></uni-icons>包邮
				</view>
				<view class="service-detail">全场商品包邮，免配送费</view>
				<view class="service-item">
					<uni-icons type="checkbox" size="18" color="#ff8d8d"></uni-icons>七天无理由退换
				</view>
				<view class="service-detail">在不影响二次销售的情况下，可以提出七天无理由退换的申请</view>
				<button class="confirm-btn" @click="goodsServiceClose()">确定</button>
			</view>
		</uni-popup>
		<uni-popup ref="orderPopup" type="bottom">
			<view class="orderPopup">
				<view class="order-goods">
					<image class="order-goods-img" :mode="aspectFill" :src="carousels[0].image"></image>
					<view class="order-title">{{goods.title}}</view>
				</view>
				<view class="order-num">
					<view>商品数量</view>
					<sunui-stepper :label='1' :max="10" :val="count" :min="1" @change="stepper"></sunui-stepper>
				</view>
				<button class="cancle-btn" @click="goodsAddCancle()">取消加入</button>
				<button class="confirm-btn" @click="goodsAddCart()">加入购物车</button>
			</view>
		</uni-popup>
		<!-- <uni-goods-nav :fill="true"  :options="options" :buttonGroup="buttonGroup"  @click="onClick" @buttonClick="buttonClick" /> -->
		<!-- <uni-goods-nav :fill="true"  :options="options"/> -->
	
	<view class="goood-nav-container">
	<view class="goods-nav">
		<view class="icon-container">
			<uni-icons type="shop" size="18" color="#646566"></uni-icons>
			<view>客服</view>
		</view>
		<view class="icon-container" @click="toGoodCart()">
			<uni-icons type="cart" size="20" color="#646566"></uni-icons>
			<view>购物车</view>
		</view>
		<view class="add-cart" @click="addCart()">加入购物车</view>
		<view class="to-buy">立即购买</view>
	</view>
	</view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../constant/constant.js';
	import sunuiStepper from '../../../components/sunui-stepper/sunui-stepper.vue'
	// import uniGoodsNav from '../../../components/uni-goods-nav/uni-goods-nav.vue'
	export default {
		components: {
			sunuiStepper
			// uniGoodsNav
			},
		data() {
			return {
				goodsId: '',
				goods: {},
				carousels: [],
				count:1,//加入购物车的数量
				userid:''
			}
		},
		mounted() {
			this.getUser();
		},
		methods: {
			toGoodCart(){
				// 跳转到购物车
				console.log('跳转到购物车')
				uni.navigateTo({
					url: `/pages/shop/cartPage/cartPage`,
				});
			},
			getUser(){
				let _this = this;
				uni.getStorage({
					key: 'user',
					success: function(res) {
						_this.userid = res.data.id;
					}
				});
			},
			 stepper(e) {
				 this.count = e.val;
			 },
			addCart(){
				this.$refs.orderPopup.open()
			},
			goodsAddCancle(){
				this.$refs.orderPopup.close()
			},
			goodsParamPop(){
				this.$refs.paramPopup.open()
			},
			goodsServicePop(){
				this.$refs.servicePopup.open()
			},
			goodsParamClose(){
				this.$refs.paramPopup.close()
			},
			goodsServiceClose(){
				this.$refs.servicePopup.close()
			},
			goodsAddCart(){
				uni.request({
					url: `${BASEURL}addOrder`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'POST',
					data: {
						userId:this.userid,
						state:0,
						goodId:this.goodsId,
						orderNum:this.count,
						orderPrice:this.goods.salePrice
					}, 
					success: (res) => {
						let resp = res.data.data;	
						this.goodsAddCancle();
					},
					fail() {
				
					}
				});
			},
			getGoodsDetail(id) {
				uni.request({
					url: `${BASEURL}selectGoodsById`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						id: id
					},
					success: (res) => {
						let resp = res.data.data;
						this.goods = resp;
						this.carousels = resp.carousels;
						console.log(this.goods)
						console.log('轮播图--------------')
						console.log(this.carousels)

					},
					fail() {

					}
				});
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			this.getGoodsDetail(option.goodsId);
			this.goodsId = option.goodsId;
		}
	}
</script>

<style lang="scss">
	.goods-detail-containner{
		padding-bottom: 50px;
	}
	.goood-nav-container{
		position: fixed;
		bottom: 0px;
		width: 100%;
		background-color: #FFFFFF;
	}
	.goods-nav{
		display: flex;
		justify-content: space-between;
		align-items: center;
		border-top: 1px solid #f5f5f5;
		padding: 5px;
		
		.icon-container{
			width: 50px;
			text-align: center;
		}
		.add-cart{
			border-radius: 18px;
			width: 110px;
			height: 36px;
			background-color: #ffa200;
			color: #fff;
			line-height: 36px;
			text-align: center;
		}
		.to-buy{
			border-radius: 18px;
			width: 110px;
			height: 36px;
			background-color: #ff0000;
			color: #fff;
			line-height: 36px;
			text-align: center;
		}
	}
	
	.gray-line{
		height: 20px;
		background-color: #f5f5f5;
	}
	.swiper-containner {
		width: 100%;

		.swiper {
			height: 360px;

			.swiper-item {
				width: 100%;
				display: block;
				height: 360px;
				line-height: 360px;
				text-align: center;
			}
		}
	}
	
	.goods-detail{
		.goods-price{
			padding: 10px;
			.new-price{
				font-weight: 700;
				font-size: 18px;
				color: #e54d42;
			}
			.old-price{
				padding: 0 10px;
				text-decoration:line-through;
				font-size: 16px;
				color: #a1a1a1;
			}
		}
		
		.goods-title-content{
			display: flex;
			justify-content: space-between;
			margin: 10px 0;
			padding: 10px;
			
			.goods-title{
				font-weight: 700;
				font-size: 18px;
				overflow: hidden;
				text-overflow: ellipsis;
				display: -webkit-box;
				-webkit-line-clamp: 2;
				-webkit-box-orient: vertical;
				display: -moz-box;
				-moz-line-clamp: 2;
				-moz-box-orient: vertical;
				word-wrap: break-word;
				word-break: break-all;
				white-space: normal;
			}
			.goods-share{
				width: 100px;
			}
			
		}
		
		
		.detail-pop{
			width: 100%;
			.goods-freight{
				width: 100%;
				font-size: 16px;
				height: 40px;
				background-color: #FFFFFF;
				border-bottom: 1px solid #dedede;
				line-height: 40px;
				.param-text{
					padding: 0 20px;
					color: #666666;
				}
			}
			.goods-param{
				width: 100%;
				font-size: 16px;
				height: 40px;
				background-color: #FFFFFF;
				border-bottom: 1px solid #dedede;
				line-height: 40px;
				position: relative;
				.param-text{
					padding: 0 20px;
					color: #666666;
				}
				.arrow-right{
					position: absolute;
					right: 10px;
				}
			}
			.goods-service{
				width: 100%;
				font-size: 16px;
				height: 40px;
				background-color: #FFFFFF;
				border-bottom: 1px solid #dedede;
				line-height: 40px;
				position: relative;
				.param-text{
					padding: 0 20px;
					color: #666666;
				}
				.arrow-right{
					position: absolute;
					right: 10px;
				}
			}
		}
		
		.goods-shop{
			padding: 10px;
			display: flex;
			justify-content: space-between;
			align-items: center;
			.shop-image{
				width: 50px;
				height: 50px;
				border-radius: 50%;
				background-color: #C0C0C0;
			}
			.shop-name{
				font-size: 18px;
			}
			
			.shop-btn{
				color: #666666;
				font-size: 16px;
				width: 120px;
			}
		}
	}
	
	.paramPopup{
		background-color: #FFFFFF;
		width: 100%;
		z-index: 999!important;
		padding: 10px;
		.title{
			font-size: 18px;
			font-weight: 700;
			text-align: center;
			padding: 10px 0;
		}
		.pop-line{
			margin: 10px 0;
			.pop-text{
				padding: 0 20px;
			}
		}
		.confirm-btn{
			background-color: #ff7878;
			color:#FFFFFF;
			border-radius: 20px;
			height: 40px;
			line-height: 40px;
			margin: 0 20px;
		}
	}
	
	
	.servicePopup{
		background-color: #FFFFFF;
		width: 100%;
		z-index: 999!important;
		padding: 10px;
		.title{
			font-size: 18px;
			font-weight: 700;
			text-align: center;
			padding: 10px 0;
		}
		.service-item{
			padding: 10px;
			font-size: 18px;
		}
		.service-detail{
			padding: 0 30px;
			color: #aaaaaa;
			margin-bottom: 15px;
		}
		.confirm-btn{
			background-color: #ff7878;
			color:#FFFFFF;
			border-radius: 20px;
			height: 40px;
			line-height: 40px;
			margin: 0 20px;
		}
	}
	
	.price-intro-container{
		padding: 10px;
		.price-intro-title{
			font-size: 18px;
			margin: 10px 0;
		}
		.price-intro{
			color: #b0b1b1;
			margin: 10px 0;
		}
	}
	
	.goods-rich-text{
		padding: 10px;
		.goods-detail{
			padding: 10px;
			font-size: 18px;
			font-weight: 700;
			text-align: center;
		}
	}
	
	.orderPopup{
		background-color: #FFFFFF;
		z-index: 99;
		padding: 15px 10px;
		display: flex;
		justify-content: space-between;
		flex-wrap: wrap;
		.order-goods{
			width: 90%;
			display: flex;
			justify-content: space-between;
			align-items: center;
			.order-goods-img{
				width: 120px;
				height: 120px;
			}
			.order-title{
				
			}
			
			
		}
		.order-num{
			margin: 10px 0 30px 0;
			width: 90%;
			display: flex;
			justify-content: space-between;
			align-items: center;
		}
		.cancle-btn{
			background-color: #f5f5f5;
			width: 130px;
			border-radius: 20px;
			height: 40px;
			line-height: 40px;
		}
		.confirm-btn{
			background-color: #ff7878;
			color:#FFFFFF;
			border-radius: 20px;
			height: 40px;
			width: 130px;
			line-height: 40px;
		}
	}
</style>
