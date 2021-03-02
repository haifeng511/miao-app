<template>
	<view>
		<view class="top-dialog">
			<view class="title-container">
				<text class="title">问答热榜</text>
				<view class="title-more" @click="toHotDialogPage()"><text>查看更多</text>
					<uni-icons type="arrowright" size="14"></uni-icons>
				</view>
			</view>
			<view class="top-three-container" v-for="(dialog,index) in threeDialog" :key="index" @click="toDialogDetail(dialog.id)">
				<text :class="index==0?'top-one':index==1?'top-two':'top-three'">
					{{index==0?'TOP1':index==1?'TOP2':'TOP3'}}
				</text>
				<view class="top-three-dialog">{{dialog.dialogTitle}}</view>
			</view>
		</view>
		<dialogCompent :dialogs="dialogs"></dialogCompent>
		<view class="isOver" v-if="isOver">页面到底了</view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../constant/constant.js'
	import dialogCompent from './dialogCompment.vue'
	export default {
		components: {
			dialogCompent
		},
		data() {
			return {
				hotDialogs: [],
				threeDialog: [],
				dialogs: [],
				page: 1,
				isOver: false
			}
		},
		mounted() {
			this.getHotDialogs();
			this.getDialogs();
		},
		methods: {
			toHotDialogPage() {
				uni.navigateTo({
					url: `/pages/dialog/dialogHotList/dialogHotList`,
				});
			},
			toDialogDetail(id) {
				uni.navigateTo({
					url: `/pages/dialog/dialogDetail/dialogDetail?dialogId=${id}`,
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
						for (let i = 0; i < 3; i++) {
							this.threeDialog.push(this.hotDialogs[i]);
						}
					}
				});
			},
			getDialogs() {
				let oldDialogs = this.dialogs;
				uni.request({
					url: `${BASEURL}selectDialogPage`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						page: this.page
					},
					success: (res) => {
						let resp = res.data.data;
						this.dialogs = [...this.dialogs, ...resp]

						console.log("dialog")
						console.log(resp)
					},
					fail() {
						this.dialogs = oldDialogs;
					}
				});
			},
			onPullDownRefresh() {
				console.log('refresh');
				this.page = 1;
				this.dialogs = [];
				this.getDialogs()();
				// 停止刷新
				setTimeout(function() {
					uni.stopPullDownRefresh();
				}, 1000);
			},
			onReachBottom() {
				console.log("页面触底了");
				if (this.dialogs.length < this.page * 6) {
					return this.isOver = true;
				}
				this.page = this.page + 1;
				this.getDialogs()();

			}
		}
	}
</script>

<style lang="scss">
	.top-dialog {
		margin: 0 10px;
		background-color: #f8f8fa;
		padding: 10px 0;

		.title-container {
			width: 96%;
			margin: 0 10px;
			display: inline-flex;
			justify-content: space-between;
			align-items: baseline;

			.title {
				font-size: 26px;
				font-weight: 700;
				color: #fa4c4c;
			}

			.title-more {
				font-size: 14px;
				color: #b0b0b0;
			}
		}

		.top-three-container {
			padding: 5px 10px;
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

			.top-three-dialog {
				max-width: 340px;
				margin-left: 8px;
				white-space: nowrap;
				text-overflow: ellipsis;
				overflow: hidden;
				word-break: break-all;
				font-size: 14px;
			}
		}
	}

	.isOver {
		width: 100%;
		text-align: center;
		color: #999999;
		margin: 10px;
	}
</style>
