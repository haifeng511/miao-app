<template>
	<view>
		<moment :moments="moments"></moment>
		<view v-if="ifShow" class="no-data">暂无数据</view>
		<view class="isOver" v-if="isOver">页面到底了</view>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../constant/constant.js'
	import moment from '../../home/moment/moment.vue'
	export default {
		components: {
			moment
		},
		data() {
			return {
				ifShow:false,
				userid:'1',
				moments: [],
				page: 1,
				isOver: false
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
						_this.getMoments();
					}
				});
			},
			getMoments() {
				let oldMoments = this.moments;
				uni.request({
					url: `${BASEURL}selectMomentPage`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						page: this.page,
						userId:this.userid
					},
					success: (res) => {
						let resp = res.data.data;
						this.moments = [...this.moments, ...resp];
						if(this.moments.length === 0){
							this.ifShow = true;
						}else{
							this.ifShow = false;
						}
					},
					fail() {
						this.moments = oldMoments;
					}
				});
			}
		},
		onPullDownRefresh() {
			console.log('refresh');
			this.page = 1;
			this.moments = [];
			this.getMoments();
			// 停止刷新
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},
		onReachBottom() {
			console.log("页面触底了");
			if (this.moments.length < this.page * 6) {
				return this.isOver = true;
			}
			this.page = this.page + 1;
			this.getMoments();
		
		}
	}
</script>

<style lang="scss">
.no-data{
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
