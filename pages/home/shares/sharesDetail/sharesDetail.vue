<template>
	<view class="share-container">
		<view class="share-title">{{share.title}}</view>
		<rich-text :nodes="share.content"></rich-text>
	</view>
</template>

<script>
	import {
		BASEURL
	} from '../../../../constant/constant.js'
	export default {
		data() {
			return {
				shareId:'',
				share:{
					title:'',
					content:''
				}
			}
		},
		mounted() {
			this.getShareDetail();
		},
		methods: {
			getShareDetail(){
				uni.request({
					url: `${BASEURL}selectCatSharesById`,
					headers: {
						'Content-Type': 'application/json'
					},
					method: 'GET',
					data: {
						id:this.shareId
					},
					success: (res) => {
						let resp = res.data.data;
						this.share = resp;
						console.log(this.share)
					},
					fail() {
						
					}
				});
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option); //打印出上个页面传递的参数。
			this.shareId = option.shareId;
		},
	}
</script>

<style lang="scss">
	
	.share-container{
		padding: 10px;
	}
	.share-title{
		margin: 15px 0;
		font-size: 20px;
		font-weight: 700;
	}
</style>
