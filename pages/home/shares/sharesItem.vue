<template>
	<view>
		<view v-for="(share,index) in shares" :key="index">
			<view class="shareCompment"  @click="toShareDetail(share.id)">
				<view class="share-image-container" v-if="share.image != '' ">
					<image class="share-image" :mode="aspectFill" :src="share.image"></image>
				</view>
				<view class="share-content">
					<view class="share-title">
						{{share.title}}
					</view>
					<view class="share-text">
						{{share.content}}
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			shares: {
				type: Array,
				required: true,
				default: function() {
					return []
				}
			}
		},
		methods: {
			toShareDetail: function(id) {
				uni.navigateTo({
					url: `/pages/home/shares/sharesDetail/sharesDetail?shareId=${id}`,
				});
			}
		},
		watch:{
			 // 父子组件传值时，父组件从接口获取数据，通过props传递给子组件。实际情况下：父组件获取数据有时间延迟，传递的props值为空，子组件接收的数据为props默认值
			 // 给子组件设置延时加载，判断父组件传递的值，是否为空，为空则不渲染子组件，否则执行相应方法；
			shares(newVal,oldVal){
				if(newVal.length>0){
					console.log('我接收到了！');
					console.log(this.shares);
					this.shares = newVal;
				}
			}
		}
		}
</script>

<style lang="scss">
	.shareCompment {
		padding: 10px;
		display: flex;
		background-color: #FFFFFF;
	
		.share-image-container {
			// height: 100px;
			// width: 100px;
			.share-image {
				height: 100px;
				width: 100px;
			}
		}
	
		.share-content {
			padding-left: 10px;
			
			.share-title {
				width: 285px;
				font-size: 20px;
				font-weight: 700;
				overflow: hidden;
				white-space: nowrap;
				text-overflow: ellipsis;
			}
	
			.share-text {
				margin: 5px 0;
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
	}
</style>
