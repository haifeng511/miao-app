<template>
	<view>
		首页hk9
		<hotTopicNav></hotTopicNav>
		<moment :moments="moments"></moment>
	</view>

</template>

<script>

</script>
<script>
	import {
		BASEURL
	} from '../../constant/constant.js'
	import hotTopicNav from './hotTopic/hotTopicNav.vue'
	import moment from './moment/moment.vue'
	export default {
		components: {
			hotTopicNav,
			moment
		},
		data() {
			return {
				moments: []
			}
		},
		mounted() {
			uni.request({
				url: `${BASEURL}selectMomentPage`,
				headers: {
					'Content-Type': 'application/json'
				},
				method: 'GET',
				success: (res) => {
					// console.log(res.data)
					this.moments = res.data.data;
					 console.log(this.moments)
				},
			});
		},
		methods: {

		}
	}
</script>

<style>

</style>
