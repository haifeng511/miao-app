<template>
	<view>
		<view v-for="(food,index) in foods" :key="index">
			<view class="foodCompment"  @click="toEatFoodDetail(food.id)">
				<view class="food-image-container" v-if="food.image != '' ">
					<image class="food-image" :mode="aspectFill" :src="food.image"></image>
				</view>
				<view class="food-content">
					<view class="food-title">
						{{food.food}}
					</view>
					<view class="food-text">
						{{food.content}}
					</view>
					<view class="food-eat-info">
						<view class="eat-content">
						<image v-if="food.child == 0" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/no.png'"></image>
						<image v-if="food.child == 1" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/warn.png'"></image>
						<image v-if="food.child == 2" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/yes.png'"></image>
						<text>奶猫</text>
						</view>
						<view class="eat-content">
						<image v-if="food.youth == 0" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/no.png'"></image>
						<image v-if="food.youth == 1" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/warn.png'"></image>
						<image v-if="food.youth == 2" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/yes.png'"></image>
						<text>幼年</text>
						</view>
						<view class="eat-content">
						<image v-if="food.adult == 0" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/no.png'"></image>
						<image v-if="food.adult == 1" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/warn.png'"></image>
						<image v-if="food.adult == 2" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/yes.png'"></image>
						<text>成年</text>
						</view>
						<view class="eat-content">
						<image v-if="food.old == 0" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/no.png'"></image>
						<image v-if="food.old == 1" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/warn.png'"></image>
						<image v-if="food.old == 2" class="eat-icon" :mode="aspectFill" :src="imageUrl+ 'food-category/yes.png'"></image>
						<text>老年</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		BASEURL,BASRIMAGEURL
	} from '../../../constant/constant.js'
	export default {
		props: {
			foods: {
				type: Array,
				required: true,
				default: function() {
					return []
				}
			}
		},
		data() {
			return {
				imageUrl:BASRIMAGEURL,
			};
		},
		methods: {
			toEatFoodDetail: function(id) {
				uni.navigateTo({
					url: `/pages/home/eat/eatFoodDetail/eatFoodDetail?foodId=${id}`,
				});
			}
		},
		 watch:{
			 // 父子组件传值时，父组件从接口获取数据，通过props传递给子组件。实际情况下：父组件获取数据有时间延迟，传递的props值为空，子组件接收的数据为props默认值
			 // 给子组件设置延时加载，判断父组件传递的值，是否为空，为空则不渲染子组件，否则执行相应方法；
			foods(newVal,oldVal){
				if(newVal.length>0){
					console.log('我接收到了！');
					//执行。。。。。。
					console.log(this.foods);
					this.foods = newVal;
					console.log('after foods');
					console.log(this.foods);
				}
			}
		}
	}
</script>

<style lang="scss">
	.foodCompment {
		padding: 10px;
		display: flex;
		background-color: #FFFFFF;

		.food-image-container {
			// height: 100px;
			// width: 100px;
			.food-image {
				height: 100px;
				width: 100px;
			}
		}

		.food-content {
			padding-left: 10px;
			
			.food-title {
				font-size: 20px;
				font-weight: 700;
			}

			.food-text {
				margin: 5px 0;
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

			.food-eat-info {
				display: flex;
				justify-content: space-around;
				.eat-content{
					display: flex;
					justify-content: space-around;
					align-items: center;
					.eat-icon{
						padding: 3px;
						width: 12px;
						height: 12px;
						border-radius: 50%;
					}
				}
			}
		}
	}
</style>
