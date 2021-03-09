<template>
	<view class="releaseMoment-container">
		<view class="uni-textarea">
			<textarea class="text-area" v-model="content" auto-height placeholder="和大家分享点什么吧" />
		</view>
		<view class="moment-image-containner">
			<view class="moment-image" v-if="chooseImages.length >0" v-for="(chooseImage,index) in chooseImages" :key="index">
				<image style="width: 110px; height: 110px; background-color: #eeeeee;" :mode="aspectFill" :src="chooseImage"></image>
				<view class="cancle-image" @click="cancleImage(index)">
					<uni-icons type="closeempty" size="13" color="#d3dbdb"></uni-icons>
				</view>
			</view>
			<view class="add-image-btn" @click="addImage()">
				<view>
					<uni-icons type="plusempty" size="40" color="#cdcdcd"></uni-icons>
					<view class="add-image-text">添加图片</view>
				</view>
			</view>
		</view>
		
		<view class="add-topic">
			<text class="topic-text">#添加话题</text>
			<view class="topic-input-container">
				<input class="topic-input" v-model="topic" placeholder="添加话题获得更多赞"/>
				<uni-icons type="arrowright" size="16" color="#a4a4a4"></uni-icons>
			</view>
		</view>
		<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="120">
			<view v-for="hotTopic in hotTopics" :key="hotTopic.id" class="scroll-view-item_H ">
				<view class="hotTopic-container" @click="addHotTopic(hotTopic)">
					<uni-icons type="flag" size="18" color="#4c8fd9"></uni-icons>
					<view class="hotTopic-title">#{{hotTopic.content}}#</view>
				</view>
			</view>
		</scroll-view>
		
		<view class="release-btn-container">
			<button class="release-btn" @click="addMoment()">发布</button>
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
				chooseImages:[],
				hotTopics:[],
				topic:'',
				topicId:'',
				content:'',
				images:[]
			}
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
		methods: {
			cancleImage(index){
				this.chooseImages.splice(index, 1); 
				this.images.splice(index, 1); 
			},
			addHotTopic(topic){
				this.topic = topic.content;
				this.topicId = topic.id;
			},
			addImage(){
				uni.chooseImage({
				    success: (chooseImageRes) => {
				        const tempFilePaths = chooseImageRes.tempFilePaths;
						this.chooseImages = [...this.chooseImages,...tempFilePaths];
						for(let i =0; i<this.chooseImages.length; i++){
							uni.uploadFile({
							    url:  `${BASEURL}imageUpload`, 
							    filePath: tempFilePaths[i],
							    name: 'file',
							    success: (uploadFileRes) => {
									let resp = JSON.parse(uploadFileRes.data)
									this.images.push(resp.data)
							    }
							}); 
						}
				       
				    }
				});
			},
			addMoment(){
				let image = '';
				for(let i=0;i<this.images.length;i++){
					image = this.images[i]+' '
				}
				uni.request({
					url: `${BASEURL}addMoment`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'POST',
					data: {
						"content":this.content,
						"topicId":this.topicId,
						"userId":1,
						"image":image
					},
					success: (res) => {
						let resp = res.data.data;
						console.log(resp)
					},
					fail() {
						
					}
				});
			},
		}
	}
</script>

<style lang="scss">
.releaseMoment-container{
	padding: 10px;
	.uni-textarea{
		width: 100%;
		padding: 5px 0;
		border-bottom: 1px solid #dedede;
		.text-area{
			width: 100%;
			min-height: 200px;
		}
	}
	.moment-image-containner{
		display: flex;
		// justify-content: space-around;
		.moment-image{
			margin: 10px 5px;
			width: 110px;
			height: 110px;
			position: relative;
			.cancle-image{
				position: absolute;
				top: 2px;
				right: 2px;
				width: 20px;
				height: 20px;
				border-radius: 10px;
				background:rgba(0, 0, 0, .3);
				text-align: center;
				line-height: 20px;
				color: #FFFFFF;
			}
		}
		.add-image-btn{
			margin: 10px 5px;
			width: 110px;
			height: 110px;
			border: 1px solid #dedede;
			display: flex;
			align-items: center;
			justify-content: center;
			border-radius: 5px;
			color: #cdcdcd;
			text-align: center;
		}
	}
	
	
	.add-topic{
		margin: 5px 0;
		font-size: 16px;
		font-weight: 700;
		display: flex;
		justify-content: space-between;
		.topic-input-container{
			font-size: 12px;
			font-weight: 400;
			display: flex;
			align-items: center;
		}
	}
	
	.scroll-view_H {
		white-space: nowrap;
		width: 100%;
		.scroll-view-item_H {
			display: inline-block;
			margin: 5px 15px 5px 0;
			text-align: center;
			.hotTopic-container{
				padding: 0 10px;
				display: flex;
				align-items: center;
				background-color: #f5f5f5;
				color: #4c8fd9;
				height: 28px;
				line-height: 28px;
				border-radius: 14px;
			}
			
		}
	}
	
	.release-btn-container{
		width: 100%;
		display: flex;
		justify-content: center;
		.release-btn{
			position: fixed;
			bottom: 20px;
			background-color: #fdd123;
			border-radius: 25px;
			width: 260px;
			height: 50px;
			line-height: 50px;
			font-weight: 500;
		}
	}
	
	
}
</style>
