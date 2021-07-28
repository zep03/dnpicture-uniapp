<template>
	<scroll-view class="album_scroll_view" scroll-y @scrolltolower="handleToLower">
		<!-- 轮播图开始 -->
		<view class="album_swiper">
			<swiper :indicator-dots="true" :autoplay="true" :interval="2000" circular :duration="1000">
				<swiper-item v-for="item in banner" :key="item.id">
					<image :src="item.thumb" mode=""></image>
				</swiper-item>
				
			</swiper>
		</view>
		<!-- 轮播图结束 -->
		<!-- 列表开始 -->
		<view class="album_list">
			<navigator class="album_item" :url="`/pages/album/album?id=${item.id}`" v-for="item in album" :key="item.id">
				<view class="album_img">
					<image mode="aspectFill" :src="item.cover" ></image>
				</view>
				<view class="album_info">
					<view class="album_name">{{ item.name }}</view>
					<view class="album_desc">{{ item.desc }}</view>
					<view class="album_btn">
						<view class="album_attention">关注</view>
					</view>
				</view>
			</navigator>
		</view>
		<!-- 列表结束 -->
	</scroll-view>
</template>

<script>
	export default {
		data() {
			return {
				// 请求参数对象
				params: {
					limit: 30,
					order: "new",
					skip: 0
				},
				// 轮播图数组
				banner: [],
				// 专辑列表数据
				album: [],
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
				],
				// 是否还有分页数据
				hasMore: true
				
			}
		},
		mounted() {
			// 修改页面的标题
			uni.setNavigationBarTitle({
				title: "专辑"
			})
			this.getList()
		},
		methods: {
			// 获取接口的数据
			getList() {
				this.request({
					url: "http://157.122.54.189:9088/image/v1/wallpaper/album",
					data: this.params
				}).then((res) => {
					console.log(res)
					if(this.banner.length === 0) {
						this.banner = res.data.res.banner
					}
					if(res.data.res.album.length === 0) {
						this.hasMore = false
						uni.showToast({
							title: "没有更多数据啦",
							icon: "none"
						})
						return
					}
					this.album = [...this.album, ...res.data.res.album]
					this.changeImg(this.album)
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
			// 触底会触发这个函数
			handleToLower() {
				console.log('我触底啦')
				if(this.hasMore) {
					this.params.skip+=this.params.limit
					this.getList()
				} else {
					uni.showToast({
						title: "没有数据啦",
						icon: "none"
					})
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.album_scroll_view {
		height: calc( 100vh - 36px );
	}
	.album_swiper {
		swiper {
			height: calc( 750rpx / 2.5 );
			image {
				height: 100%;
			}
		}
	}
	.album_list {
		.album_item {
			display: flex;
			padding: 10rpx;
			border-bottom: 1rpx solid #ccc;
			.album_img {
				flex: 1;
				padding: 10rpx;
				image {
					width: 200rpx;
					height: 200rpx;
				}
			}
			.album_info {
				flex: 2;
				padding: 0 10rpx;
				overflow: hidden;
				.album_name {
					font-size: 30rpx;
					color: #000000;
					padding: 10rpx 0;
				}
				.album_desc {
					font-size: 24rpx;
					padding: 10rpx 0;
					text-overflow: ellipsis;
					overflow: hidden;
					white-space: nowrap;
				}
				.album_.btn {
					padding: 10rpx;
					display: flex;
					justify-content: flex-end;
					.album_attention {
						color: $color;
						border: 1rpx solid $color;
						padding: 10rpx;
					}
				}
			}
		}
	}
</style>
