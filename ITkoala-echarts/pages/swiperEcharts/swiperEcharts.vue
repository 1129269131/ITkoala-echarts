<template>
	<view class="swiper-container">
		<scroll-view id="tab-bar" class="scroll-h" scroll-x="true" show-scrollbar="false" :scroll-into-view="scrollInto">
			<view class="tab-section">
				<view v-for="(tab, index) in tabList" :key="tab.id" class="uni-tab-item" :class="tabIndex == index ? 'uni-tab-item-active' : ''" :id="tab.id" :data-current="index">
					<text class="uni-tab-item-title" @click="ontabtap(index)">{{ tab.name }}</text>
				</view>
			</view>
		</scroll-view>
		<swiper :style="{ height: scrollH + 'px' }" :current="tabIndex" @change="swiperChange">
			<swiper-item v-for="(tab, i) in tabList" :key="i" :item-id="'a' + i">
				<category1Echarts v-if="tabIndex === 0"></category1Echarts>
				<category2Echarts v-if="tabIndex === 1"></category2Echarts>
				<category3Echarts v-if="tabIndex === 2"></category3Echarts>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
import category1Echarts from '@/pages/swiperEcharts/modules/category1Echarts.vue'
import category2Echarts from '@/pages/swiperEcharts/modules/category2Echarts.vue'
import category3Echarts from '@/pages/swiperEcharts/modules/category3Echarts.vue'

export default {
	components: {
		category1Echarts,
		category2Echarts,
		category3Echarts
	},
	data() {
		return {
			loading: false,
			scrollInto: '',
			tabList: [{ name: '类目1', id: '1' }, { name: '类目2', id: '2' }, { name: '类目3', id: '3' }],
			tabIndex: 0, // 当前tab的下标
			scrollH: 600 // 需要固定swiper的高度
		}
	},
	methods: {
		// 轮播菜单
		swiperChange(e) {
			this.tabIndex = e.detail.current
		},
		// 切换页签
		ontabtap(index) {
			if (this.loading) {
				return
			}
			this.tabIndex = index
		},
		// tab页切换
		change(index) {
			this.tabIndex = index
		}
	}
}
</script>

<style lang="scss" scoped>
.scroll-h {
	flex-direction: row;
	white-space: nowrap;
	background-color: #fff;
	border: 1rpx solid #e0f6fe;
	border-radius: 15rpx;
	/deep/.u-scroll-view {
		width: auto !important;
	}
	/deep/.uni-scroll-view::-webkit-scrollbar {
		display: none;
	}
}

.uni-tab-item {
	width: 33.33%;
	height: 90rpx !important;
	line-height: 90rpx !important;
	text-align: center;
	display: inline-block;
	flex-wrap: nowrap;
	font-size: 12px;
}
.uni-tab-item-title {
	color: #000;
	font-size: 14px;
	font-weight: bold;
	flex-wrap: nowrap;
	/* #ifndef APP-PLUS */
	white-space: nowrap;
	/* #endif */
}
.uni-tab-item-active {
	width: 33.33%;
	background: #e0f6fe;
	border-radius: 15rpx;
}
.uni-tab-item-active .uni-tab-item-title {
	color: #1084db;
}

.tab-section {
	display: flex;
	justify-content: space-around;
	align-items: center;
}
</style>
