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
			<view class="comment" v-for="(comment,index) in comments" :key="index">
				<view class="comment-userinfo">
					<image class="comment-avatar" :mode="aspectFill" :src="comment.avatar"></image>
					<text class="comment-username">{{comment.username}}</text>
				</view>
				<view class="comment-content">{{comment.comment}}</view>
				<view class="comment-time">{{comment.commentTime}}</view>
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
				images: [],
				indicatorDots: true,
				autoplay: true,
				interval: 2000,
				duration: 1000,
				moment: {},
				comments: [],
			}
		},
		methods: {
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
						console.log(this.moment.image)
					},
					fail() {

					}
				});
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option); //打印出上个页面传递的参数。
			this.getMomentComments(option.momentId);
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
			margin: 10px 18px;
			display: flex;
			flex-wrap: wrap;
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
</style>
