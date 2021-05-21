<template>
	<view>
		<moment :moments="moments"></moment>
		<view v-if="ifShow" class="no-data">暂无数据</view>
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
				userid:'',
				moments: [],
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
				uni.request({
					url: `${BASEURL}selectCollectMoments`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						userId:this.userid
					},
					success: (res) => {
						let resp = res.data.data;
						this.moments = resp;
						if(this.moments.length === 0){
							this.ifShow = true;
						}else{
							this.ifShow = false;
						}
						
					},
					fail() {
						
					}
				});
			}
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
</style>
