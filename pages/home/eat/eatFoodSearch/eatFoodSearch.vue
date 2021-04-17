<template>
	<view>
		<!-- 设置圆角 -->
		<view class="search-bar">
			<uni-search-bar placeholder="请输入食物关键词搜索" :radius="100" @confirm="search"></uni-search-bar>
		</view>
		<view class="searh-foods-container">
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
			<eatfoodItem :foods="searchfoods"></eatfoodItem>
		<view class="isOver" v-if="isOver">页面到底了</view>
		</view>
		</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../../constant/constant.js'
	import eatfoodItem from '../eatFoodItem.vue'
	export default {
		components: {
			eatfoodItem
		},
		data() {
			return {
				searchfoods: [],
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
				this.getSearchfoods();
			},
			getHistory(){
				 // 获取本地存储的数据，如果有则将其转换为js变量，如果没有则返回空数组
					 var data = uni.getStorageSync('searchFood');
				    data = data ? JSON.parse(data) : [];
					this.history = data;
					console.log(this.history)
			},
			clearHistorySearch(){
				this.history = [];
				uni.setStorageSync('searchFood', this.history);
			},
			getSearchfoods(){
				let oldSearchfoods = this.searchfoods;
				uni.request({
					url: `${BASEURL}selectCatEatPage`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						page: this.page,
						food:this.title
					},
					success: (res) => {
						let resp = res.data.data;
						this.searchfoods = [...this.searchfoods, ...resp];
						if(this.searchfoods.length == 0){
							this.none = true
						}else{
							this.none = false
						}
						console.log('=============');
						console.log(this.searchfoods)
					},
					fail() {
						this.searchfoods = oldSearchfoods;
					}
				});
			},
			onReachBottom() {
				if (this.foodsList.length < this.page * 6) {
					return this.isOver = true;
				}
				this.page = this.page + 1;
				this.getSearchfoods();
			
			},
			onPullDownRefresh() {
				this.title = '';
				this.page = 1;
				this.searchfoods = [];
				this.isHistory = false;
				this.getSearchfoods();
				// 停止刷新
				setTimeout(function() {
					uni.stopPullDownRefresh();
				}, 1000);
			},
			search(e) {
				if(this.history.length > 3){
					uni.showToast({
						icon:'none',
					    title: '请先清除历史搜索记录',
					    duration: 2000
					});
				}else{
					this.searchfoods = [];
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
					uni.setStorageSync('searchFood', JSON.stringify(this.history));
					
					this.getSearchfoods();
				}
				
			},
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
	.searh-foods-container {
		background-color: #f5f5f5;
		padding: 10px;
	}
	
	.no-data{
		text-align: center;
	}

	
	.isOver {
		width: 100%;
		text-align: center;
		color: #999999;
		margin: 10px;
	}
</style>
