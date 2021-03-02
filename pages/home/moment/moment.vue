<template>
	<view class="moment-container">
		<view v-for="moment in moments" :key="moment.id">
			<view class="moment-content" @click="toMomentDetail(moment.id)">
				<image class="moment-image" :mode="scaleToFill" :src="moment.image"></image>
				<view class="topic-title" v-if="moment.topic !== null">#{{moment.topic}}#</view>
				<view class="moment-info">
					<view class="moment">{{moment.content}}</view>
					<view class="monent-user">
						<view class="userinfo">
							<image class="moment-avatar" :mode="aspectFill" :src="moment.avatar"></image>
							<text class="moment-username">{{moment.username}}</text>
						</view>

						<view class="moment-like">
							<uni-icons type="heart" size="14" color="#333333" class="like-icon"></uni-icons>
							<text class="moment-likeNum">{{moment.likeNum}}</text>
						</view>
					</view>

				</view>
			</view>
		</view>
	</view>
	</view>
</template>

<script>
	export default {
		props: {
			moments: {
				type: Array,
				required: true,
				default: function() {
					return []
				}
			}
		},
		data() {
			return {
				// 上方props定义了moments,data中不能再次定义，this.moments即可访问到父组件传来的值
				// moments: this.moments
			};
		},
		methods: {
			toMomentDetail: function(id) {
				console.log("toMomentDetail" + id)
				uni.navigateTo({
					url: `/pages/home/moment/momentDetail/momentDetail?momentId=${id}`,
				});
			}
		}
	}
</script>

<style lang="scss">
	.moment-container {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;

		.moment-content {
			margin: 8px 0;
			width: 190px;
			height: 295px;
			border: 1px solid #ececec;
			border-radius: 8px;
			position: relative;			

			.moment-image {
				width: 190px;
				height: 190px;
				background-color: #eeeeee;
				border-radius: 8px;
			}

			.topic-title {
				margin: 8px 10px 5px 10px;
				font-size: 12px;
				color: #007AFF;
			}

			.moment-info {
				font-weight: 700;
				color: #666666;
				margin: 0px 10px 8px 10px;

				.moment {
					overflow: hidden;
					text-overflow: ellipsis;
					display: -webkit-box;
					-webkit-line-clamp: 2;
					-webkit-box-orient: vertical;
					display: -moz-box;
					-moz-line-clamp: 2;
					-moz-box-orient: vertical;
					word-wrap: break-word;
					word-break: break-all;
					white-space: normal;
				}

				.monent-user {
					width: 90%;
					display: flex;
					justify-content: space-between;
					align-items: center;
					position: absolute;
					bottom: 8px;

					.userinfo {
						display: inline-flex;
						align-items: center;

						.moment-avatar {
							width: 20px;
							height: 20px;
							background-color: #eeeeee;
							border-radius: 10px;
						}

						.moment-username {
							margin-left: 5px;
							max-width: 100px;
							white-space: nowrap;
							text-overflow: ellipsis;
							overflow: hidden;
							word-break: break-all;
							font-size: 12px;
						}
					}

					.moment-like {
						display: inline-flex;
						align-items: center;

						.moment-likeNum {
							margin-left: 3px;
							font-size: 12px;
						}
					}

				}
			}
		}
	}
</style>
