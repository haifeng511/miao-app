<template>
	<view>
		<view class="dialogCompment" v-for="(dialog,index) in hotDialogs" :key="index" @click="toDialogDetail(dialog.id)">
			<view class="dialog-title-container">
				<view :class="index==0?'top-one':index==1?'top-two':index==2?'top-three':'top-common'">{{dialogNum(index)}}</view>
				<text class="dialog-title">{{dialog.dialogTitle}}</text>
			</view>
			<view class="dialog-container">
				<view class="dialog-content">
					<view class="dialog-user">
						<image class="dialog-avatar" :mode="aspectFill" :src="dialog.avatar"></image>
						<text class="dialog-username">{{dialog.username}}</text>
					</view>
					<view class="dialog">
						{{dialog.content}}
					</view>
				</view>
				<view class="dialog-image-container" v-if="dialog.image != '' ">
					<image class="dialog-image" :mode="aspectFill" :src="dialog.image.split(',')[0]"></image>
				</view>
			</view>
			<view class="dialog-other">
				<text class="dialog-like">{{dialog.releaseTime}}</text>
				<!-- 		<text class="dialog-see">{{dialog.seeNum}}个浏览</text> -->
			</view>
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
				hotDialogs: [],
				num: ''
			}
		},
		mounted() {
			this.getHotDialogs();
		},
		computed: {
			dialogNum: function(index) {
				return function(index) {
					let num = '';
					if (index < 3) {
						num = 'TOP' + (++index);
					} else if (index < 9) {
						num = '0' + (++index);
					} else {
						num = (++index)
					}
					return num;
				}


			},
		},
		methods: {
			toDialogDetail(id) {
				uni.request({
					url: `${BASEURL}clickAddDialogSeeNum`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'POST',
					data: {
						id: id
					},
					success: (res) => {
						if(res.data.data == 1){
							uni.navigateTo({
								url: `/pages/dialog/dialogDetail/dialogDetail?dialogId=${id}`,
							});
						}
					}
				});
			},
			getHotDialogs() {
				uni.request({
					url: `${BASEURL}selectHotDialogs`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					success: (res) => {
						let resp = res.data.data;
						this.hotDialogs = resp;
					}
				});
			},
		}
	}
</script>


<style lang="scss">
	.dialogCompment {
		width: 100%;
		padding: 10px;
		border-bottom: 1px solid #dedede;

		.dialog-title-container {
			padding: 8px;
			white-space: nowrap;
			text-overflow: ellipsis;
			overflow: hidden;
			word-break: break-all;
			display: inline-flex;
			align-items: center;

			.top-one {
				width: 36px;
				height: 18px;
				line-height: 18px;
				background-color: #fa4c4c;
				font-size: 10px;
				color: #FFFFFF;
				text-align: center;
				border-radius: 5px;
				font-family: "SimSun";
			}

			.top-two {
				width: 36px;
				height: 18px;
				line-height: 18px;
				background-color: #ff9d00;
				font-size: 10px;
				color: #FFFFFF;
				text-align: center;
				border-radius: 5px;
				font-family: "SimSun";
			}

			.top-three {
				width: 36px;
				height: 18px;
				line-height: 18px;
				background-color: #ffcf34;
				font-size: 10px;
				color: #FFFFFF;
				text-align: center;
				border-radius: 5px;
				font-family: "SimSun";
			}

			.top-common {
				width: 36px;
				height: 18px;
				line-height: 18px;
				background-color: #efeff8;
				font-size: 12px;
				color: #676767;
				text-align: center;
				border-radius: 5px;
				font-family: "SimSun";
			}

			.dialog-title {
				margin-left: 8px;
				font-size: 20px;
				font-weight: 700;
			}
		}

		.dialog-container {
			padding: 8px;
			display: flex;
			align-items: center;
			width: 94%;
			justify-content: space-between;

			.dialog-content {
				.dialog-user {
					display: inline-flex;
					align-items: center;

					.dialog-avatar {
						width: 30px;
						height: 30px;
						background-color: #eeeeee;
						border-radius: 15px;
					}

					.dialog-username {
						margin-left: 5px;
						max-width: 140px;
						white-space: nowrap;
						text-overflow: ellipsis;
						overflow: hidden;
						word-break: break-all;
					}
				}

				.dialog {
					overflow: hidden;
					text-overflow: ellipsis;
					display: -webkit-box;
					-webkit-line-clamp: 3;
					-webkit-box-orient: vertical;
					display: -moz-box;
					-moz-line-clamp: 3;
					-moz-box-orient: vertical;
					word-wrap: break-word;
					word-break: break-all;
					white-space: normal;
				}
			}

			.dialog-image-container {
				padding: 5px;
				width: 120px;
				height: 90px;
				background-color: #eeeeee;

				.dialog-image {
					width: 120px;
					height: 90px;
				}
			}

		}

		.dialog-other {
			color: #a7a7a7;

			.dialog-like {
				padding: 0 5px;
			}
		}
	}
</style>
