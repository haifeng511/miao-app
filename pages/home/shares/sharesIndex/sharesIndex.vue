<template>
	<view>
		<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="120">
			<view v-for="sharesCategory in sharesCategories" :key="sharesCategory.category" class="scroll-view-item_H ">
				<view class="image-content" @click="toSharesCategory(sharesCategory.category)">
					<image style="width: 100px; height: 130px; background-color: #eeeeee;border-radius: 8px;" :mode="aspectFill" :src="imageUrl + sharesCategory.image"></image>
					<view class="image-title">{{sharesCategory.title}}</view>
				</view>
			</view>
		</scroll-view>
		<view class="gray-line"></view>
		<view class="read-text">推荐文章</view>
		<sharesItem :shares="shares"></sharesItem>
		<view class="isOver" v-if="isOver">页面到底了</view>
	</view>
</template>

<script>
	import {
		BASEURL,BASRIMAGEURL
	} from '../../../../constant/constant.js'
	import sharesItem from '../sharesItem.vue'
	export default {
		components: {
			sharesItem
		},
		data() {
			return {
				isOver:false,
				page: 1,
				shares: [],
				imageUrl:BASRIMAGEURL,
				sharesCategories: [{
						category: 0,
						image: 'article/2.png',
						title: '新手养宠'
					},
					{
						category: 1,
						image: 'article/3.png',
						title: '品种百科'
					},
					{
						category: 2,
						image:'article/4.png',
						title: '日常护理'
					},
					{
						category: 3,
						image: 'article/5.png',
						title: '宠物喂养'
					},
					{
						category: 4,
						image: 'article/6.png',
						title: '行为纠正'
					},
					{
						category: 5,
						image: 'article/1.png',
						title: '种草测评'
					},
				]
			}
		},
		mounted() {
			this.getHotShares();
		},
		methods: {
			getHotShares() {
				let oldshares = this.shares;
				uni.request({
					url: `${BASEURL}selectCatSharesPage`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						page: this.page 
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
			toSharesCategory: function(category) {
				uni.navigateTo({
					url: `/pages/home/shares/sharesCategory/sharesCategory?category=${category}`,
				});
			},
		},
		onReachBottom() {
			console.log('页面触底了')
			if (this.shares.length < this.page * 6) {
				return this.isOver = true;
			}
			this.page = this.page + 1;
			this.getHotShares();
		},
	}
</script>

<style lang="scss">
	.scroll-view_H {
		white-space: nowrap;
		width: 100%;
	}

	.scroll-view-item_H {
		padding: 10px;
		display: inline-block;
		// width: 120px;
		// height: 150px;
	}
	
	.image-content{
		text-align: center;
	}
	.gray-line{
			height: 20px;
			background-color: #f5f5f5;
		}
	.read-text {
		margin: 10px 0;
		font-size: 20px;
		font-weight: 700;
	}
	
	.isOver {
		width: 100%;
		text-align: center;
		color: #999999;
		margin: 10px;
	}
</style>
