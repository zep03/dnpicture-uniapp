<template>
	<view>
		<!-- 专辑背景开始 -->
		<view class="album_bg">
			<image :src="album.cover" mode="widthFix"></image>
			<view class="album_info">
				<view class="album_name">{{ album.name }}</view>
				<view class="album_btn">关注专辑</view>
			</view>
		</view>
		<!-- 专辑背景结束 -->
		<!-- 专辑作者开始 -->
		<view class="album_author">
			<view class="album_author_info">
				<image :src="album.user.avatar" mode="widthFix"></image>
				<view class="author_name">{{ album.user.name }}</view>
			</view>
			<view class="album_author_desc">
				<text>{{ album.desc }}</text>
			</view>
		</view>
		<!-- 专辑作者结束 -->
		<!-- 列表开始 -->
		<view class="album_list">
			<view class="album_item" v-for="(item, index) in wallpaper" :key="item.id">
				<go-detail :itemList="wallpaper" :index="index">
					<image :src="item.img"></image>
				</go-detail>
			</view>
		</view>
		<!-- 列表结束 -->
	</view>
</template>

<script>
	import goDetail from '@/components/goDeatail.vue'
	export default {
		data() {
			return {
				params: {
					limit: 30,
					order: "new",
					skip: 0,
					first: 1 // “1” 表示第一次请求 “0”表示不是第一次请求
				},
				id: -1,
				album: {},
				wallpaper: [],
				hasMore: true, // 是否还有下一页数据
				// 模拟的图片数组
				myImg: [
					"https://img2.baidu.com/it/u=3589748713,2051752919&fm=26&fmt=auto&gp=0.jpg",
					"https://img1.baidu.com/it/u=2629213230,2545258637&fm=26&fmt=auto&gp=0.jpg",
					"https://img0.baidu.com/it/u=1817143901,1168063195&fm=26&fmt=auto&gp=0.jpg",
					"https://img2.baidu.com/it/u=1814565549,2954866278&fm=26&fmt=auto&gp=0.jpg",
					"https://img2.baidu.com/it/u=3218269182,174840221&fm=26&fmt=auto&gp=0.jpg",
					"https://img1.baidu.com/it/u=3457474027,2173279348&fm=26&fmt=auto&gp=0.jpg",
					"https://img0.baidu.com/it/u=1985214588,1769404971&fm=26&fmt=auto&gp=0.jpg",
					"https://img0.baidu.com/it/u=334019971,2556320270&fm=26&fmt=auto&gp=0.jpg",
					"https://img1.baidu.com/it/u=1432740930,315068824&fm=26&fmt=auto&gp=0.jpg",
					"https://img1.baidu.com/it/u=2364337684,71284638&fm=26&fmt=auto&gp=0.jpg"
				]
			}
		},
		onLoad(options) {
			console.log(options)
			this.id = options.id
			// this.id = "5e59075ae7bce73965512bd6"
			this.getList()
		},
		// 页面触底， 上拉加载下一页事件
		onReachBottom() {
			console.log('我也是有底线的！！！')
			if(this.hasMore) {
				this.params.first = 0
				this.params.skip += this.params.limit
				this.getList()
			} else {
				uni.showToast({
					title: "没有更多数据啦",
					icon: "none"
				})
			}
		},
		components: {
			goDetail
		},
		methods: {
			getList() {
				this.request({
					url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
					data: this.params
				}).then((res) => {
					console.log(res)
					if(Object.keys(this.album).length === 0) {
						this.album = res.data.res.album
						this.album.cover = this.myImg[0]
					}
					if(res.data.res.wallpaper.length === 0) {
						this.hasMore = false
						uni.showToast({
							title: "没有更多数据啦",
							icon: "none"
						})
						return
					}
					this.wallpaper = [...this.wallpaper, ...res.data.res.wallpaper]
					this.changeImg2(this.wallpaper)
				})
			},
			// 改变接口的imgurl
			changeImg (iarray) {
				for(var i=0; i<iarray.length; i++) {
					var index = Math.floor(Math.random()*10)
					// iarray[i].img = "https://img2.baidu.com/it/u=1814565549,2954866278&fm=26&fmt=auto&gp=0.jpg"
					iarray[i].cover = this.myImg[index]
					
				}
			},
			changeImg2 (iarray) {
				for(var i=0; i<iarray.length; i++) {
					var index = Math.floor(Math.random()*10)
					// iarray[i].img = "https://img2.baidu.com/it/u=1814565549,2954866278&fm=26&fmt=auto&gp=0.jpg"
					iarray[i].img = this.myImg[index]
					
				}
			},
		}
	}
</script>

<style lang="scss" scoped>
	.album_bg {
		position: relative;
		image {
			
		}
		
		.album_info {
			position: absolute;
			width: 100%;
			left: 0;
			bottom: 0;
			display: flex;
			justify-content: space-between;
			height: 80rpx;
			align-items: center;
			color: #FFFFFF;
			padding: 0 15rpx;
			.album_name {
				font-size: 40rpx;
				
			}
			.album_btn {
				background-color: $color;
				width: 152rpx;
				height: 60rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				border-radius: 10rpx;
				
			}
		}
	}

	.album_author {
		padding: 10rpx;
		.album_author_info {
			padding: 10rpx 0;
			display: flex;
			align-items: center;
			image {
				width: 50rpx;
			}
			.author_name {
				color: #000000;
				margin-left: 15rpx;
			}
		}
		.album_author_desc {
			
		}
	}

	.album_list {
		display: flex;
		flex-wrap: wrap;
		.album_item {
			width: 33.33%;
			border: 3rpx solid #FFFFFF;
			image {
				width: 100%;
				height: 200rpx;
			}
		}
	}
</style>
