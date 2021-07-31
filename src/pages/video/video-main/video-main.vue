<template>
	<scroll-view @scrolltolower="handleScrollToLower" scroll-y enable-flex class="video_main">
		<view class="video_item" v-for="item in videowp" :key="item.id"
		@click="handleGoVideo(item)">
			<image :src="item.img" mode="widthFix"></image>
		</view>
	</scroll-view>
</template>

<script>
	export default {
		data () {
			return {
				videowp: [],
				hasMore: true // 下一页是否还有数据
			}
		},
		props: {
			urlObj: Object
		},
		watch: {
			urlObj () {
				// console.log('urlObj发生数据了变化')
				// console.log(this.urlObj)
				// 标签页点击切换时，重置数据
				this.videowp = []
				this.getList()
			}
		},
		// mounted只执行一次
		mounted() {
			// console.log(this.urlObj)
			this.getList()
		},
		methods: {
			getList () {
				this.request({
					url: this.urlObj.url,
					data: this.urlObj.params
				}).then(res => {
					console.log(res)
					if(res.data.res.videowp.length === 0) {
						this.hasMore = false
						uni.showToast({
							title: "没有更多数据啦",
							icon: "none"
						})
						return
					}
					this.videowp = [...this.videowp, ...res.data.res.videowp]
				})
			},
			// 滚动条触底事件
			handleScrollToLower () {
				if(this.hasMore) {
					this.urlObj.params.skip += this.urlObj.params.limit
					this.getList()
				} else {
					uni.showToast({
						title: "没有更多数据啦",
						icon: "none"
					})
				}
			},
			handleGoVideo(item){
				// 1. 将数据存到全局共享中
				getApp().globalData.video = item
				// 2. 页面跳转
				uni.navigateTo({
					url: '/pages/videoPlay/videoPlay'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.video_main {
		display: flex;
		flex-wrap: wrap;
		height: calc( 100vh - 36px );
		.video_item {
			width: 33.33%;
			border: 5rpx solid #FFFFFF;
		}
	}
</style>
