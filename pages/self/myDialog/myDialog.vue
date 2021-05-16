<template>
	<view>
		<dialogCompent :dialogs="dialogs"></dialogCompent>
		<view v-if="ifShow" class="no-data">暂无数据</view>
		<view class="isOver" v-if="isOver">页面到底了</view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../constant/constant.js'
	import dialogCompent from '../../dialog/dialogCompment.vue'
	export default {
		components: {
			dialogCompent
		},
		data() {
			return {
				ifShow: false,
				userid: '',
				dialogs: [],
				page: 1,
				isOver: false
			}
		},
		mounted() {
			this.getUser();
		},
		methods: {
			getUser() {
				let _this = this;
				uni.getStorage({
					key: 'user',
					success: function(res) {
						_this.userid = res.data.id;
						_this.getDialogs();
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
						page: this.page,
						userId: this.userid
					},
					success: (res) => {
						let resp = res.data.data;
						this.dialogs = [...this.dialogs, ...resp];
						if (this.dialogs.length === 0) {
							this.ifShow = true;
						} else {
							this.ifShow = false;
						}
					},
					fail() {
						this.dialogs = oldDialogs;
					}
				});
			}
		},
		onPullDownRefresh() {
			console.log('refresh');
			this.page = 1;
			this.dialogs = [];
			this.getDialogs();
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
			this.getDialogs();

		}
	}
</script>

<style lang="scss">
	.no-data {
		padding: 15px;
		text-align: center;
		color: #999999;
		font-size: 18px;
	}

	.isOver {
		width: 100%;
		text-align: center;
		color: #999999;
		margin: 10px;
	}
</style>
