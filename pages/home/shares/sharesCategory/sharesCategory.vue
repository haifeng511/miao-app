<template>
	<view>
		<!-- 设置圆角 -->
		<view class="search-bar">
			<uni-search-bar placeholder="请输入标题关键词搜索" :radius="100" @confirm="search"></uni-search-bar>
		</view>
		<shares-item :shares="shares"></shares-item>
		<view class="isOver" v-if="isOver">页面到底了</view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../../constant/constant.js'
	import sharesItem from '../sharesItem.vue'
	export default {
		components: {
			sharesItem
		},
		data() {
			return {
				shares:[],
				category:'',
				title:'',
				isOver:false,
				page:1
			}
		},
		mounted() {
			this.getShares();
		},
		methods: {
			search(e) {
				this.shares = [];
				this.title = e.value.trim();
				this.getShares();
			},
			getShares(){
				let oldshares = this.shares;
				uni.request({
					url: `${BASEURL}selectCatSharesPage`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						title:this.title,
						page: this.page,
						category:this.category,
						limit:8
					},
					success: (res) => {
						let resp = res.data.data;
						console.log(resp);
						this.shares = [...this.shares, ...resp];
						console.log(this.shares)
					},
					fail() {
						this.shares = oldshares;
					}
				});
			},
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option); //打印出上个页面传递的参数。
			this.category = option.category;
		},
		onReachBottom() {
			console.log('页面触底了')
			if (this.shares.length < this.page * 6) {
				return this.isOver = true;
			}
			this.page = this.page + 1;
			this.getShares();
		},
	}
</script>

<style lang="scss">
	.search-bar{
		margin: 10px 20px;
		background-color: #FFFFFF;
	}
.isOver {
				width: 100%;
				text-align: center;
				color: #999999;
				margin: 10px;
	}
</style>
