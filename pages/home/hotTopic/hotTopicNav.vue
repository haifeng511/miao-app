<template>
	<view>
		<view class="nav-title-container">
			<view>
				<uni-icons type="chat-filled" size="18" color="#ffca2c"></uni-icons>
				<text class="nav-title">热门话题</text>
			</view>
			<view class="nav-more" @click="hotTopicMore()">
				<text>查看更多</text>
				<uni-icons type="arrowright" size="14"></uni-icons>
			</view>
		</view>
		<view>
			<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="120">
				<view v-for="hotTopic in hotTopics" :key="hotTopic.id" class="scroll-view-item_H ">
					<!-- {{hotTopic.content}} -->
					<view class="image-content" @click="toHotTopic(hotTopic.id)">
						<image style="width: 170px; height: 90px; background-color: #eeeeee;" class="nav-image" :mode="aspectFill" :src="hotTopic.image"
						 @error="imageError"></image>
						<view class="image-title">#{{hotTopic.content}}#</view>
					</view>
				</view>
			</scroll-view>
		</view>
	</view>

</template>

<script>
	import {
		BASEURL
	} from '../../../constant/constant.js'
	export default {
		data() {
			return {
				hotTopics: []
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
		onLoad() {
			console.log("nav--onload")
		},
		methods: {
			hotTopicMore: function(){
				uni.navigateTo({
				    url: '/pages/home/hotTopic/hotTopicList/hotTopicList',
				});
			},
			toHotTopic:function(topicId){
				console.log("toHotTopic")
				uni.navigateTo({
				    url: `/pages/home/hotTopic/hotTopicDetail/hotTopicDetail?topicId=${topicId}`,
				});
			},
			imageError: function(e) {
				console.error('image发生error事件，携带值为' + e.detail.errMsg)
			}
		}
	}
</script>

<style lang="scss">
	.nav-title-container {
		display: flex;
		width: 100%;
		margin: 10px 5px;
		align-items: center;
		
		.nav-title{
			color: #333333;
			font-size: 16px;
			margin-left: 8px;
			font-weight: 700;
			
		}
		.nav-more {
			position: absolute;
			right: 5px;
			margin-right: 0;
			color: #a1a1a1;
			font-size: 12px;
		}
	}

	.scroll-view_H {
		white-space: nowrap;
		width: 100%;
	}

	.scroll-view-item_H {
		position: relative;
		display: inline-block;
		width: 43%;
		height: 90px;
		line-height: 90px;
		text-align: center;

		.nav-image {
			position: relative;
		}

		.nav-image::before {
			content: "";
			position: absolute;
			left: 0;
			right: 0;
			bottom: 0;
			top: 0;
			background-color: rgba(0, 0, 0, .3);
			z-index: 2;
		}

		.image-title {
			position: absolute;
			top: 0;
			bottom: 0;
			right: 0;
			left: 0;
			font-size: 14px;
			color: #FFFFFF;
			z-index: 3;
			letter-spacing:3px;
			white-space: nowrap;
			text-overflow: ellipsis;
			overflow: hidden;
			word-break: break-all;
		}
	}
</style>
