<template>
	<view class="releasedialog-container">
		<input class="dialog-title" v-model="dialogTitle" placeholder="输入提问标题"/>
		<view class="uni-textarea">
			<textarea class="text-area" v-model="content" auto-height placeholder="输入提问具体内容" />
		</view>
		<view class="dialog-image-containner">
			<view class="dialog-image" v-if="chooseImages.length >0" v-for="(chooseImage,index) in chooseImages" :key="index">
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
		<view class="release-btn-container">
			<button class="release-btn" @click="adddialog()">发布</button>
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
				chooseImages:[],
				dialogTitle:'',
				content:'',
				images:[],
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
			cancleImage(index){
				this.chooseImages.splice(index, 1); 
				this.images.splice(index, 1); 
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
			adddialog(){
				let image = '';
				for(let i=0;i<this.images.length;i++){
					image = this.images[i]+' '
				}
				uni.request({
					url: `${BASEURL}addDialog`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'POST',
					data: {
						"content":this.content,
						"dialogTitle":this.dialogTitle,
						"userId":this.userid,
						"image":image
					},
					success: (res) => {
						let resp = res.data.data;
						if(resp === 1){
							uni.showToast({
							    title: '提问成功！',
								icon:'success',
							    duration: 2000
							});
							setTimeout(function(){
								uni.navigateBack({
								    delta: 1
								});
							},2500)
						}
					},
					fail() {
						
					}
				});
			},
		}
	}
</script>

<style lang="scss">
.releasedialog-container{
	padding: 10px;
	.dialog-title{
		margin: 10px 0;
		border-bottom: 1px solid #dedede;
		height: 35px;
		line-height: 35px;
		font-size: 18px;
	}
	.uni-textarea{
		width: 100%;
		padding: 5px 0;
		border-bottom: 1px solid #dedede;
		.text-area{
			width: 100%;
			min-height: 200px;
		}
	}
	.dialog-image-containner{
		display: flex;
		// justify-content: space-around;
		.dialog-image{
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
