
<template>
	<view class="goods-container">
		<view v-for="goods in goodsList" :key="goods.id">
			<view class="goods-content" @click="goodsDetail(goods.id)">
				<image class="goods-cover" :mode="scaleToFill" :src="goods.carousels[0].image"></image>
				<view class="goods-title">{{goods.title}}</view>
				<view class="goods-price">￥{{goods.salePrice}}</view>
			</view>
		</view>
	</view> 
</template>

<script>
	import {
		BASEURL
	} from  '../../constant/constant.js'
	export default {
		props: {
			goodsList: {
				type: Array,
				required: true,
				default: function() {
					return []
				}
			}
		},
		data() {
			return {
	
			}
		},
		methods: {
			goodsDetail(goodsId){
				uni.navigateTo({
					url: `/pages/shop/goodsDetail/goodsDetail?goodsId=${goodsId}`,
				});
			}
		}
	}
</script>

<style lang="scss">
.goods-container{
	display: flex;
	flex-wrap: wrap;
	justify-content: space-between;
	padding-top: 10px;
}

.goods-content{
	margin: 10px 0;
	background-color: #FFFFFF;
	width: 185px;
	height: 280px;
	border: 1px solid #ececec;
	border-radius: 8px;
}

.goods-cover{
	width: 185px;
	height: 185px;
	background-color: #eeeeee;
	border-radius: 8px;
}
.goods-title{
	margin: 10px;
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
.goods-price{
	font-size: 12px;
	color: #f69b53;
}
</style>
