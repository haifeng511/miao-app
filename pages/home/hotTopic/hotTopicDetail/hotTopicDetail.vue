<template>
	<view>
		<view class="hotTopic-container">
			<image class="hotTopic-image" :mode="aspectFill" :src="hotTopic.image"></image>
			<view class="hotTopic-content">
				<view class="hotTopic">#{{hotTopic.content}}#</view>
				<view class="hotTopic-num">{{hotTopic.num}}人参与</view>
				<view class="hotTopic-intro">{{hotTopic.introduction}}</view>
			</view>
		</view>
		<moment :moments="moments"></moment>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../../constant/constant.js'
	import moment from '../../moment/moment.vue'
	export default {
		components: {
			moment
		},
		data() {
			return {
				topicId:'',
				hotTopic:{},
				moments: [],
				page:1
			}
		},
		mounted(){
			console.log(this.topicId)
			this.getTopic(this.topicId);
			this.getMoments(this.topicId);
		},
		methods: {
			getTopic(topicId){
				uni.request({
					url: `${BASEURL}selectHotTopicById`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data:{
						id:topicId
					},
					success: (res) => {
						let resp = res.data.data;
						this.hotTopic = resp
					}
				});
			},
			getMoments(topicId){
				let oldMoments = this.moments;
				uni.request({
					url: `${BASEURL}selectMomentPage`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data:{
						topicId:topicId,
						page:this.page
					},
					success: (res) => {
						let resp = res.data.data;
						this.moments = [...this.moments,...resp]
						console.log('hotTopicDetail')
						console.log(resp)
					},
					fail() {
						this.moments = oldMoments;
					}
				});
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option); //打印出上个页面传递的参数。
			this.topicId = option.topicId;
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
	.hotTopic-container{
		 position: relative;
		.hotTopic-image{
			width: 100%; 
			height: 180px;
			 background-color: #eeeeee;
			
		}
		.hotTopic-image::before {
				content: "";
				position: absolute;
				left: 0;
				right: 0;
				bottom: 0;
				top: 0;
				background-color: rgba(0, 0, 0, .3);
				z-index: 2;
			}
			.hotTopic-content{
				position: absolute;
				top: 0;
				bottom: 0;
				right: 0;
				left: 0;
				color: #FFFFFF;
				z-index: 3;
				text-align: center;
				margin: 0 10px;
		}
		.hotTopic{
			margin-top: 20px;
			font-size: 20px;
		}
		.hotTopic-num{
			margin-top: 5px;
			font-size: 12px;
		}
		.hotTopic-intro{
			margin-top: 10px;
			font-size: 14px;
		}
	}

</style>
