<template>
	<view>
		首页hk9
		<hotTopicNav></hotTopicNav>
		<view class="gray-line"></view>
		<view class="title-container">
				<uni-icons type="list" size="18" color="#ffca2c"></uni-icons>
				<text class="title">推荐动态</text>
		</view>
		<moment :moments="moments"></moment>
		<view class="addMoment">
			<view class="add-icon">
				<uni-icons  type="camera-filled" size="23" color="#ffffff"></uni-icons>
				<view>发布</view>
				</view>
		
		</view>
		<view class="isOver" v-if="isOver">页面到底了</view>
	</view>

</template>

<script>

</script>
<script>
	import {
		BASEURL
	} from '../../constant/constant.js'
	import hotTopicNav from './hotTopic/hotTopicNav.vue'
	import moment from './moment/moment.vue'
	export default {
		components: {
			hotTopicNav,
			moment
		},
		data() {
			return {
				moments: [],
				page:1,
				isOver:false
			}
		},
		mounted() {
			this.getMoments();
		},
		methods: {
			getMoments(){
				let oldMoments = this.moments;
				uni.request({
					url: `${BASEURL}selectMomentPage`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data:{
						page:this.page
					},
					success: (res) => {
						let resp = res.data.data;
						this.moments = [...this.moments,...resp]
						
						console.log("home")
						console.log(resp)
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
			if(this.moments.length < this.page*6){
				return this.isOver = true;
			}
			this.page = this.page+1;
			this.getMoments();
			
		}
	}
</script>

<style lang="scss">
.isOver{
	width: 100%;
	text-align: center;
	color: #999999;
	margin: 10px;
}
	.gray-line{
			width: 100%;
			background: #f5f5f5;
			height: 10px;
		}
		.title-container{
			margin: 10px 18px;
			padding: 10px 0;
			.title{
				font-size: 16px;
				font-weight: 700;
				margin-left: 10px;
			}
		}
		
		.addMoment{
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
			.add-icon{
				color: #FFFFFF;
				font-size: 14px;
			}
		}
</style>
