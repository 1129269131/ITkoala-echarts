<template>
	<view class="content">
		<view style="margin: 15rpx;">
			<view style="margin: 15rpx;">
				<button type="primary" @click.stop="navigateTo('/pages/swiperEcharts/swiperEcharts')">swiper+echarts</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		methods: {
			// 跳转页面
			navigateTo(url){
				uni.navigateTo({
					url
				})
			}
		}
	}
</script>

<style>
</style>
