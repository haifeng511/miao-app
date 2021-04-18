<template>
	<view>
		<view>
			<view>推荐动态</view>
			<view @click="toIfEat()">能不能吃</view>
			<view @click="toShares()">养宠知识</view>
		</view>
		<view class="home-container">
			<hotTopicNav></hotTopicNav>
			<view class="gray-line"></view>
			<view class="title-container">
				<uni-icons type="list" size="18" color="#ffca2c"></uni-icons>
				<text class="title">推荐动态</text>
			</view>
			<moment :moments="moments"></moment>
			<view class="addMoment" @click="toReleaseMoment()">
				<view class="add-icon">
					<uni-icons type="camera-filled" size="23" color="#ffffff"></uni-icons>
					<view>发布</view>
				</view>
			</view>
			<view class="isOver" v-if="isOver">页面到底了</view>
		</view>
	</view>
	
</template>

<script>
	import {
		BASEURL
	} from '../../constant/constant.js'
	import hotTopicNav from './hotTopic/hotTopicNav.vue'
	import moment from './moment/moment.vue'
	// import Tabs from '../../components/wiszx-tabs/tabs.vue'
	// import TabPane from '../../components/wiszx-tabs/tabPane.vue'
	// import eatIndex from './eat/eatIndex/eatIndex.vue'
	export default {
		components: {
			hotTopicNav,
			moment,
			// eatIndex,
			// Tabs,
			// TabPane
		},
		data() {
			return {
				// current:0,
				// TabList:[
				//     {title:'推荐动态'},
				//     {title:'能不能吃'},
				//     {title:'养宠知识'}
				// ],
				moments: [],
				page: 1,
				isOver: false
			}
		},
		mounted() {
			this.getMoments();
		},
		methods: {
			// tabsChange(index){
			//     this.current = index
			// },
			toShares(){
				uni.navigateTo({
					url: '/pages/home/shares/sharesIndex/sharesIndex',
				});
			},
			toIfEat() {
				console.log('toifeat');
				uni.navigateTo({
					url: '/pages/home/eat/eatIndex/eatIndex',
				});
			},
			toReleaseMoment() {
				uni.navigateTo({
					url: '/pages/home/moment/releaseMoment/releaseMoment',
				});
			},
			getMoments() {
				let oldMoments = this.moments;
				uni.request({
					url: `${BASEURL}selectMomentPage`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						page: this.page
					},
					success: (res) => {
						let resp = res.data.data;
						this.moments = [...this.moments, ...resp]
					},
					fail() {
						this.moments = oldMoments;
					}
				});
			}
		},
		onPullDownRefresh() {
			console.log('refresh');
			this.page = 1;
			this.moments = [];
			this.getMoments();
			// 停止刷新
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},
		onReachBottom() {
			console.log("页面触底了");
			if (this.moments.length < this.page * 6) {
				return this.isOver = true;
			}
			this.page = this.page + 1;
			this.getMoments();

		}
	}
</script>

<style lang="scss">
	// .home-container{
	// 	position:relative;
	// }
	.isOver {
		width: 100%;
		text-align: center;
		color: #999999;
		margin: 10px;
	}

	.gray-line {
		width: 100%;
		background: #f5f5f5;
		height: 10px;
	}

	.title-container {
		padding: 10px;
		border-bottom: 1px solid #dedede;
		.title {
			font-size: 16px;
			font-weight: 700;
			margin-left: 10px;
		}
	}

	.addMoment {
		background-color: #ffca2c;
		width: 54px;
		height: 54px;
		border-radius: 27px;
		text-align: center;
		position: fixed;
		bottom: 50px;
		right: 20px;
		display: flex;
		align-items: center;
		justify-content: center;
		box-shadow: 3px 3px 2px #d9d9d9;

		.add-icon {
			color: #FFFFFF;
			font-size: 14px;
		}
	}
</style>
