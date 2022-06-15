<template>
	<view class="content">
		<!-- #ifdef APP-PLUS || H5 -->
		<view @click="category3Echarts.onClick" :prop="echartsData" :change:prop="category3Echarts.updateEcharts" id="category3Echarts" class="category3Echarts"></view>
		<!-- #endif -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				echartsData: []
			}
		},
		mounted() {
			this.dealData()
		},
		methods: {
			dealData() {
				this.echartsData = [
					{ name: '类目3-描述1', value: 0, percent: '0%' },
					{ name: '类目3-描述2', value: 0, percent: '0%' },
					{ name: '类目3-描述3', value: 0, percent: '0%' }
				]
			},
			onViewClick(options) {

			}
		}
	}
</script>

<script module="category3Echarts" lang="renderjs">
	let myEChart
	export default {
		name: 'category3Echarts',
		data() {
			return {
				currentData: {}
			}
		},
		mounted() {
			if (typeof window.echarts === 'function') {
				this.initEcharts()
			} else {
				// 动态引入较大类库避免影响页面展示
				const script = document.createElement('script')
				// view 层的页面运行在 www 根目录，其相对路径相对于 www 计算
				script.src = 'static/plugin/echarts.js'
				script.onload = this.initEcharts.bind(this)
				document.head.appendChild(script)
			}
		},
		methods: {
			initEcharts() {
				let option = {
					color: ['#f36544', '#f7b547', '#3f86e7', '#82ca69'],
					series: [{
						name: '图表3',
						type: 'pie',
						radius: ['15%', '30%'],
						center: ['50%', '50%'],
						data: this.echartsData,
						label: {
							color: 'rgba(51, 51, 51, 1)',
							formatter: (params) => {
								return params.data.name + '\n(' + params.data.percent + ')'
							}
						},
						labelLine: {
							lineStyle: {
								color: 'rgba(51, 51, 51, 0.8)'
							},
							smooth: 0.2,
							length: 5,
							length2: 10
						}
					}]
				}
				
				let myEcharts = document.getElementsByClassName('category3Echarts');
				myEChart = echarts.init(myEcharts[2])
				// 观测更新的数据在 view 层可以直接访问到
				myEChart.setOption(option)
				myEChart.on('click', (params) => {
					this.currentData = params.data
				})
			},
			updateEcharts(newValue, oldValue, ownerInstance, instance) {
				// 监听 service 层数据变更
				if (myEChart) {
					let option = {
						color: ['#f36544', '#f7b547', '#3f86e7', '#82ca69'],
						series: [{
							name: '图表3',
							type: 'pie',
							radius: ['15%', '30%'],
							center: ['50%', '50%'],
							data: newValue,
							label: {
								color: 'rgba(51, 51, 51, 1)',
								formatter: (params) => {
									return params.data.name + '\n(' + params.data.percent + ')'
								}
							},
							labelLine: {
								lineStyle: {
									color: 'rgba(51, 51, 51, 0.8)'
								},
								smooth: 0.2,
								length: 5,
								length2: 10
							}
						}]
					}
					
					myEChart.setOption(option)
				}
			},
			onClick(event, ownerInstance) {
				// 调用 service 层的方法
				ownerInstance.callMethod('onViewClick', {
					data: this.currentData
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.category3Echarts {
		width: 100%;
		height: 500rpx;
	}
</style>
