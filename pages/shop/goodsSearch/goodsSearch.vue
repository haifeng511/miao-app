<template>
	<view>
		<!-- 设置圆角 -->
		<view class="search-bar">
			<uni-search-bar placeholder="请输入商品关键词搜索" :radius="100" @confirm="search"></uni-search-bar>
		</view>
		<view class="searh-goods-container">
			<view  v-show="none" class="no-data">暂无数据</view>
			<view  v-show="isHistory" class="history-container">
				<view class="history-title-container">
					<view class="history-title">历史记录</view>
					<uni-icons  type="trash" size="20" color="#333333" @click="clearHistorySearch()"></uni-icons>
				</view>
				<view>
					<text class="history-btn" v-for="(h,index) in history" :key="index" @click="searchTitle(h)">{{h}}</text>
				</view>
			</view>
			<view v-for="goods in searchGoods" :key="goods.id">
				<view class="goods-content" @click="goodsDetail(goods.id)">
					<image class="goods-cover"  :mode="scaleToFill" :src="goods.carousels[0].image"></image>
					<view class="goods-detail">
						<view class="goods-title">{{goods.title}}</view>
						<view class="goods-price">
							<text class="new-price">￥{{goods.salePrice}}</text>
							<text class="old-price">￥{{goods.referencePrice}}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="isOver" v-if="isOver">页面到底了</view>
		</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../constant/constant.js'

	export default {
		data() {
			return {
				searchGoods: [],
				page:1,
				title:'',
				isOver: false,
				none:false,
				history:[],
				isHistory:true
			}
		},
		mounted() {
			this.getHistory();
		},
		methods: {
			searchTitle(h){
				this.isHistory = false;
				this.title = h;
				this.getSearchGoods();
			},
			getHistory(){
				 // 获取本地存储的数据，如果有则将其转换为js变量，如果没有则返回空数组
					 var data = uni.getStorageSync('searchData');
				    data = data ? JSON.parse(data) : [];
					this.history = data;
			},
			clearHistorySearch(){
				// 清除历史记录
				this.history = [];
				uni.setStorageSync('searchData', this.history);
			},
			getSearchGoods(){
				let oldSearchGoods = this.searchGoods;
				uni.request({
					url: `${BASEURL}selectGoodsPage`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						page: this.page,
						title:this.title
					},
					success: (res) => {
						let resp = res.data.data;
						this.searchGoods = [...this.searchGoods, ...resp];
						if(this.searchGoods.length == 0){
							this.none = true
						}else{
							this.none = false
						}
						console.log(this.searchGoods)
					},
					fail() {
						this.searchGoods = oldSearchGoods;
					}
				});
			},
			onReachBottom() {
				// 判断是否是最后一页
				if (this.goodsList.length < this.page * 6) {
					return this.isOver = true;
				}
				this.page = this.page + 1;
				// 获取数据
				this.getSearchGoods();
			
			},
			onPullDownRefresh() {
				this.title = '';
				this.page = 1;
				this.searchGoods = [];
				this.isHistory = false;
				this.getSearchGoods();
				// 停止刷新
				setTimeout(function() {
					uni.stopPullDownRefresh();
				}, 1000);
			},
			search(e) {
				// 搜索
				if(this.history.length > 10){
					uni.showToast({
						icon:'none',
					    title: '请先清除历史搜索记录',
					    duration: 2000
					});
				}else{
					this.searchGoods = [];
					this.title = e.value.trim();
					this.isHistory = false;
					
					  // 判断输入框的数据是否已存在
					for(var i = 0; i < this.history.length; i++){
						if(this.history[i] == this.title){
							// 删除已存在的数据 splice(开始删除的索引，删除个数)
							this.history.splice(i,1);
						}
					}
					// 将输入框的数据追加至原数据的前面
					this.history.unshift(this.title);
					// 将数据重新存储至本地,记得要将数据转为json格式字符串，否则数据将会丢失
					uni.setStorageSync('searchData', JSON.stringify(this.history));
					
					this.getSearchGoods();
				}
				
			},
			goodsDetail(goodsId) {
				uni.navigateTo({
					url: `/pages/shop/goodsDetail/goodsDetail?goodsId=${goodsId}`,
				});
			}
		}
	}
</script>

<style lang="scss">
	.history-container{
		padding: 10px 20px;
		.history-title-container{
			display: flex;
			justify-content: space-between;
			margin: 10px 0;
			.history-title{
				font-weight: 700;
				font-size: 18px;
			}
		}
		.history-btn{
			height: 24px;
			border-radius: 15px;
			border: 1px solid #e6e6e6;
			margin: 5px;
			padding: 6px;
		}
	}
	.search-bar{
		margin: 10px 20px;
		background-color: #FFFFFF;
	}
	.searh-goods-container {
		background-color: #f5f5f5;
		padding: 10px;
	}
	
	.no-data{
		text-align: center;
	}

	.goods-content {
		display: flex;
		height: 120px;
		padding: 10px;
		margin: 10px 0;
		background-color: #FFFFFF;

		.goods-cover {
			height: 120px;
			width: 120px;
		}
		
		.goods-detail{
			padding: 15px 0;
			width: auto;
			
			.goods-title{
				// width: 70%;
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
			.goods-price{
				padding: 10px;
				.new-price{
					font-weight: 700;
					font-size: 18px;
					color: #f69e5a;
				}
				.old-price{
					padding: 0 10px;
					text-decoration:line-through;
					font-size: 16px;
					color: #a1a1a1;
				}
			}
		}
	}
	.isOver {
		width: 100%;
		text-align: center;
		color: #999999;
		margin: 10px;
	}
</style>
