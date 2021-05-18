<template>
	<view>
		<view class="dialog-content">
			<view class="dialog-title">{{dialog.dialogTitle}}</view>
			<view class="dialog">{{dialog.content}}</view>
			<view class="dialog-image-container" v-if="dialog.image != '' " v-for="(img,index) in dialog.image.split(',')">
				<image class="dialog-image" :mode="aspectFill" :src="img"></image>
			</view>
			<view class="userinfo">
				<image class="dialog-avatar" :mode="aspectFill" :src="dialog.avatar"></image>
				<text class="dialog-username">{{dialog.username}}</text>
			</view>

			<view class="releaseTime"> {{dialog.releaseTime}}</view>
		</view>
		<view class="gray-line"></view>
		<view class="title-container">
			<uni-icons type="list" size="18" color="#ffca2c"></uni-icons>
			<text class="title">回答</text>
		</view>
		<view class="comments-container">
			<view class="comment" v-for="(comment,index) in comments" :key="index"  v-if="comments.length >0">
				<view class="comment-userinfo">
					<image class="comment-avatar" :mode="aspectFill" :src="comment.avatar"></image>
					<text class="comment-username">{{comment.username}}</text>
				</view>
				<view class="comment-content">{{comment.comment}}</view>
				<view class="comment-time">{{comment.commentTime}}</view>
			</view>
			<view class="no-comment" v-if="comments.length == 0">暂无回答</view>
		</view>

		<view class="add-comment">
			<view class="add-icon-container">
				<uni-icons type="eye" size="23" color="#ffca2c"></uni-icons>
				<text class="comment-num">{{dialog.seeNum}}</text>
			</view>

			<button class="add-btn" @click="toAnswer()">我要回答……</button>
			<uni-icons type="heart" size="23" color="#3e3e3e"></uni-icons>
			<view class="add-icon-container">
				<uni-icons type="chatbubble" size="23" color="#3e3e3e"></uni-icons>
				<text class="comment-num">{{dialog.comments.length}}</text>
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
	} from '../../../constant/constant.js'
	export default {
		data() {
			return {
				dialogId:'',
				images: [],
				indicatorDots: true,
				autoplay: true,
				interval: 2000,
				duration: 1000,
				dialog: {},
				comments: [],
				comment:'',
				userid:''
			}
		},
		mounted() {
			this.getUser();
		},
		methods: {
			getUser(){
				let _this = this;
				uni.getStorage({
					key: 'user',
					success: function(res) {
						_this.userid = res.data.id;
					}
				});
			},
			toAnswer(){
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
						userId:this.userid,
						sharesId:this.dialogId,
						category:2
					},
					success: (res) => {
						let resp = res.data.data;
						console.log(resp)
					if(res.data.code == 200){
						 this.$refs.popup.close()
					}			
					this.getdialogComments(this.dialogId)
					},
					fail() {
				
					}
				});
			},
			getdialogComments: function(id) {
				uni.request({
					url: `${BASEURL}selectdialogComments`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						id: id
					},
					success: (res) => {
						let resp = res.data.data;
						this.dialog = resp;
						this.comments = resp.comments;
						this.images.push(resp.image)
					},
					fail() {

					}
				});
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option); //打印出上个页面传递的参数。
			this.getdialogComments(option.dialogId);
			this.dialogId = option.dialogId;
		}
	}
</script>

<style lang="scss">
	.dialog-content {
		// width: 100%;
		padding: 18px;

		.dialog-title {
			font-size: 20px;
			font-weight: 700;
			padding: 5px 0;
		}

		.dialog {
			font-size: 14px;
			color: #666666;

		}

		.dialog-image-container {
			padding: 5px 0;
		}

		.userinfo {
			padding: 5px 0;
			display: inline-flex;
			align-items: center;

			.dialog-avatar {
				width: 30px;
				height: 30px;
				background-color: #eeeeee;
				border-radius: 15px;
			}

			.dialog-username {
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

		.releaseTime {
			padding: 5px 0;
			font-size: 14px;
			color: #999999;
		}
	}

	.gray-line {
		width: 100%;
		background: #f5f5f5;
		height: 10px;
	}

	.title-container {
		padding: 10px 18px;

		.title {
			font-size: 16px;
			font-weight: 700;
			padding-left: 10px;
		}
	}

	.comments-container {
		padding: 10px 18px 45px 18px;
		display: flex;
		flex-wrap: wrap;

		.no-comment {
			width: 100%;
			text-align: center;
			color: #999999;
			padding: 10px;
		}

		.comment {
			width: 100%;
			display: flex;
			flex-wrap: wrap;
			border-bottom: 1px solid #dedede;

			.comment-userinfo {
				padding: 8px 0;
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
					max-width: 120px;
					white-space: nowrap;
					text-overflow: ellipsis;
					overflow: hidden;
					word-break: break-all;
					font-size: 16px;
					font-weight: 700;
					color: #4566ad;
				}
			}

			.comment-content {
				padding-left: 36px;
				width: 100%;
			}

			.comment-time {
				font-size: 14px;
				color: #999999;
				padding: 10px 0 10px 36px;
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
			border: 1px solid rgba(0, 0, 0, .05);
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
