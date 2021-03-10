<template>
	<view>
		<view class="swiper-containner">
			<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
				<swiper-item v-for="image in images" :key=index>
					<image class="swiper-item" :mode="aspectFill" :src="image"></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="moment-content">
			<view class="userinfo">
				<image class="moment-avatar" :mode="aspectFill" :src="moment.avatar"></image>
				<text class="moment-username">{{moment.username}}</text>
			</view>
			<view class="moment">{{moment.content}}</view>
			<view v-if="moment.topic" class="hotTopic">
					<uni-icons type="chat" size="18" color="#ffca2c"></uni-icons>
					<view class="hotTopic-title">{{moment.topic}}</view>
			</view>
			<view class="releaseTime"> {{moment.releaseTime}}</view>
		</view>
		<view class="gray-line"></view>
		<view class="title-container">
				<uni-icons type="list" size="18" color="#ffca2c"></uni-icons>
				<text class="title">评论</text>
		</view>
		<view class="comments-container">
			<view class="comment" v-for="(comment,index) in comments" :key="index" v-if="comments.length >0">
				<view class="comment-userinfo">
					<image class="comment-avatar" :mode="aspectFill" :src="comment.avatar"></image>
					<text class="comment-username">{{comment.username}}</text>
				</view>
				<view class="comment-content">{{comment.comment}}</view>
				<view class="comment-time">{{comment.commentTime}}</view>
			</view>
			<view class="no-comment" v-if="comments.length == 0">暂无评论</view>
		</view>
		<view class="add-comment">
			<view class="add-icon-container">
				<uni-icons type="eye" size="23" color="#ffca2c"></uni-icons>
				<text class="comment-num">{{moment.seeNum}}</text>
			</view>
		
			<button class="add-btn" @click="toComment()">请赐评</button>
			<view class="add-icon-container">
			<uni-icons type="heart" size="23" color="#3e3e3e"></uni-icons>
			<text class="comment-num">{{moment.likeNum}}</text>
			</view>
			<view class="add-icon-container">
				<uni-icons type="chatbubble" size="23" color="#3e3e3e"></uni-icons>
				<text class="comment-num">{{moment.comments.length}}</text>
			</view>
		</view>
		
		<uni-popup ref="popup" type="bottom">
			<view class="popcomment">
				<view class="pop-top">
					<text class="cancle" @click="close()">取消</text>
					<text class="comment-title">评论</text>
					<text class="send" @click="send()">发送</text>
				</view>
				<textarea class="text-comment" v-model="comment" auto-height placeholder="添加你的评论帮助他" />
				</view>
		</uni-popup>
	</view>
</template>
<script>
	import {
		BASEURL
	} from '../../../../constant/constant.js'
	export default {
		data() {
			return {
				images: [],
				indicatorDots: true,
				autoplay: true,
				interval: 2000,
				duration: 1000,
				moment: {},
				comments: [],
				comment:'',
				momentId:''
			}
		},
		methods: {
			toComment(){
				  // 通过组件定义的ref调用uni-popup方法
			 this.$refs.popup.open()
			},
			// 弹出层点击取消按钮触发
			 close(){
			     // TODO 做一些其他的事情，before-close 为true的情况下，手动执行 done 才会关闭对话框
			     this.comment = '';
			      this.$refs.popup.close()
			},
			send(){
				uni.request({
					url: `${BASEURL}addComment`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'POST',
					data: {
						comment:this.comment,
						userId:1,
						sharesId:this.momentId,
						category:1
					},
					success: (res) => {
						let resp = res.data.data;
						console.log(resp)
					if(res.data.code == 200){
						 this.$refs.popup.close()
					}
					this.getMomentComments(this.momentId);
					},
					fail() {
				
					}
				});
			},
			getMomentComments: function(id) {
				uni.request({
					url: `${BASEURL}selectMomentComments`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						id: id
					},
					success: (res) => {
						let resp = res.data.data;
						this.moment = resp;
						this.comments = resp.comments;
						this.images.push(resp.image)
						console.log(resp)
					
					},
					fail() {

					}
				});
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option); //打印出上个页面传递的参数。
			this.getMomentComments(option.momentId);
			this.momentId = option.momentId;
		}
	}
</script>

<style lang="scss">
	.swiper-containner {
		width: 100%;
		.swiper {
			height: 280px;
			.swiper-item {
				width: 100%;
				display: block;
				height: 280px;
				line-height: 280px;
				text-align: center;
			}
		}
	}


	.moment-content{
		width: 100%;
		margin: 18px;
		.userinfo {
			display: inline-flex;
			align-items: center;
			.moment-avatar {
				width: 30px;
				height: 30px;
				background-color: #eeeeee;
				border-radius: 15px;
			}
		
			.moment-username {
				margin-left: 10px;
				max-width: 150px;
				white-space: nowrap;
				text-overflow: ellipsis;
				overflow: hidden;
				word-break: break-all;
				font-size: 16px;
				font-weight: 700;
			}
		}
		.moment{
			font-size: 14px;
			color: #666666;
		}
		.hotTopic{
			display: inline-flex;
			background: #fff6ea;
			border-radius: 20px;
			margin: 10px 0;
			padding: 5px 20px;
			.hotTopic-title{
				font-size: 14px;
				color: #ffc974;
				margin-left: 5px;
			}
		}
		.releaseTime{
			margin: 10px 0;
			font-size: 14px;
			color: #999999;
		}
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
		.comments-container{
			padding: 10px 18px 45px 18px;
			display: flex;
			flex-wrap: wrap;
			.no-comment {
				width: 100%;
				text-align: center;
				color: #999999;
				padding: 10px;
			}
			.comment{
				width: 100%;
				display: flex;
				flex-wrap: wrap;
				.comment-userinfo {
					margin: 8px 0;
					width: 100%;
					display: flex;
					align-items: center;
					.comment-avatar {
						width: 30px;
						height: 30px;
						background-color: #eeeeee;
						border-radius: 15px;
					}
				
					.comment-username {
						margin-left: 10px;
						max-width: 90px;
						white-space: nowrap;
						text-overflow: ellipsis;
						overflow: hidden;
						word-break: break-all;
						font-size: 16px;
						font-weight: 700;
						color:#4566ad;
					}
				}
				.comment-content{
					margin-left: 36px;
					width: 100%;
				}
				.comment-time{
					margin-left: 36px;
					font-size: 14px;
					color: #999999;
					padding: 10px 0;
				}
			}
			
		}
		
		
		.add-comment {
			position: fixed;
			bottom: 0;
			width: 100%;
			border-top: 1px solid #dedede;
			display: inline-flex;
			justify-content: space-around;
			align-items: center;
			background-color: #FFFFFF;
		
			.add-icon-container {
				display: flex;
				align-items: center;
				line-height: 32px;
				height: 32px;
		
				.comment-num {
					padding-left: 3px;
					font-size: 12px;
					color: #808080;
				}
			}
		
			.add-btn {
				background-color: #f2f2f2;
				border: 1px solid rgba(40, 44, 53, .05);
				line-height: 32px;
				height: 32px;
				margin: 5px 0;
				border-radius: 16px;
				width: 160px;
				font-size: 12px;
				color: #808080;
			}
		}
		
		.popcomment{
			padding: 10px 18px;
			background-color: #FFFFFF;
			border-radius:  10px 10px 0 0;
			.pop-top{
				display:flex;
				justify-content: space-between;
				.cancle{
					color: #808080;
				}
				.comment-title{
					font-size: 16px;
				}
				
				.send{
					color: #007AFF;
				}
				
			}
			.text-comment{
				width: 100%;
				margin: 15px 0;
				min-height: 80px;
				background-color: #dddddd;
				color: #808080;
			}
			
		}
</style>
