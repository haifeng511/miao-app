<template>
	<view class="hotTopicList-content ">
		<view v-for="hotTopic in hotTopics" :key="hotTopic.id" >
			<view class="hotTopic-content" @click="toHotTopic(hotTopic.id)">
				<image style="width: 185px; height: 115px; background-color: #eeeeee;border-radius: 8px;" class="hotTopic-image" :mode="aspectFill" :src="hotTopic.image"
				 @error="imageError"></image>
				<view class="hotTopic-title">#{{hotTopic.content}}#</view>
				<view class="hotTopic-num">{{hotTopic.num}}人参与</view>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../../constant/constant.js'
	export default {
		data() {
			return {
				hotTopics:[]
			};
		},
		mounted() {
			uni.request({
				url: `${BASEURL}selectAllHotTopics`,
				headers: {
					'Content-Type': 'application/json'
				},
				method: 'GET',
				success: (res) => {
					this.hotTopics = res.data.data;
				},
			});
		},
		methods:{
			toHotTopic:function(topicId){
				console.log("toHotTopic")
				uni.navigateTo({
				    url: `/pages/home/hotTopic/hotTopicDetail/hotTopicDetail?topicId=${topicId}`,
				});
			},
		}
	}
</script>

<style lang="scss">
	.hotTopicList-content{
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
		.hotTopic-content{
			margin: 8px;
			width: 185px;
			// height: 185px;
			border: 1px solid #ececec;
			border-radius: 8px;
			.hotTopic-title{
				margin: 8px 10px 5px 10px;
				font-weight: 700;
			}
			.hotTopic-num{
				color: #666666;
				margin: 0px 10px 8px 10px;
			}
		}
	}
	
</style>
