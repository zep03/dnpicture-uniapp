<template>
	<scroll-view @scrolltolower="handleToLower" class="recommend_view" scroll-y="" v-if="recommends.length > 0">
		<!-- 推荐开始 
		 :url="`/pages/album/album?id=${item.target}`" -->
		<view class="recommend_wrap">
			<navigator class="recommend_item"
			  url="/pages/album/album?id=5dea218ae7bce73965510333"
			 :key="item.id" v-for="(item, index) in recommends">
				<image :src="item.thumb" mode="widthFix"></image>
			</navigator>
		</view>
		<!-- 推荐结束 -->
		<!-- 月份开始 -->
		<view class="months_wrap">
			<view class="months_title">
				<view class="months_title_info">
					<view class="months_info">
						<text>{{ months.DD }}/</text>
						<text>{{ months.MM }} 月</text>
					</view>
					<view class="months_text">{{ months.title }}</view>
				</view>
				<view class="months_title_more"> > 更多 </view>
			</view>
			<view class="months_content">
				<view class="months_item" v-for="(item, index) in months.items" :key="item.id">
					<image :src="item.img" mode="a"></image>
				</view>
			</view>
		</view>
		<!-- 月份结束 -->
		<!-- 热门开始 -->
		<view class="hots_wrap">
			<view class="hots_title">
				<text> 热门 </text>
			</view>
			<view class="hots_content">
				<view class="hot_item" v-for="item in hots" :key="item.id">
					<image :src="item.img"></image>
				</view>
			</view>
		</view>
		<!-- 热门结束 -->
	</scroll-view>
</template>

<script>
	import moment from 'moment'
	export default {
		data() {
			return {
				// 头部推荐模块数据
				recommends: [],
				// 月份模块
				months: [],
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
				// 热门
				hots: [],
				params: {
					limit: 30,
					order: "hot",
					skip: 0
				},
				// 是否还有下一页
				hasMore: true
				
			}
		},
		methods: {
			// 获取接口的数据
			getList () {
				this.request({
					url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
					data: this.params
				}).then((res) => {
					console.log(res)
					// 判断是否还有下一页数据
					if(res.data.res.vertical.length === 0) {
						this.hasMore = false
						uni.showToast({
							title: "没有更多数据啦",
							icon: "none"
						})
						return
					}
					if(this.recommends.length ===0) {
						// 说明是第一次发送请求
						// 头部推荐模块数据
						this.recommends = res.data.res.homepage[1].items
						// 月份模块数据
						this.months = res.data.res.homepage[2]
						// 将时间戳改为 18号/月  moment.js
						this.months.MM = moment(this.months.stime).format("MM")
						this.months.DD = moment(this.months.stime).format("DD")
						this.changeImg(this.months.items)
					}
					
					// 获取热门数据的列表
					this.hots = [...this.hots, ...res.data.res.vertical]
					this.changeImg(this.hots)
				}).catch((err) => {
					console.log(err)
				})
			},
			// 改变接口的imgurl
			changeImg (iarray) {
				for(var i=0; i<iarray.length; i++) {
					var index = Math.floor(Math.random()*10)
					// iarray[i].img = "https://img2.baidu.com/it/u=1814565549,2954866278&fm=26&fmt=auto&gp=0.jpg"
					iarray[i].img = this.myImg[index]
					
				}
			},
			// 滚动条触底事件
			handleToLower () {
				if(this.hasMore) {
					// 1. 修改请求参数 skip+=limit
					this.params.skip += this.params.limit
					// 2. 重新发送请求 getList()
					this.getList()
					// 3. 请求回来成功了 hots数据进行叠加
					console.log('滚动条触底了')
				} else {
					uni.showToast({
						title: '没有数据啦',
						icon: "none"
					})
				}
			}
		},
		mounted() {
			// 修改页面的标题
			uni.setNavigationBarTitle({
				title: "推荐"
			})
			this.getList()
		}
	}
</script>

<style lang="scss">
	.recommend_view {
		height: calc( 100vh - 36px );
	}
	.recommend_wrap {
		display: flex;
		flex-wrap: wrap;

		.recommend_item {
			width: 50%;
			border: 5rpx solid #fff;
		}
	}

	.months_wrap {
		.months_title {
			display: flex;
			justify-content: space-between;
			padding: 20rpx;

			.months_title_info {
				color: $color;
				font-size: 30rpx;
				font-weight: 600;
				display: flex;

				.months_info {
					margin-right: 20rpx;

					text {
						font-size: 30rpx;
					}
				}

				.months_text {
					font-size: 30rpx;
					color: #666666;
					margin-left: 40rpx;
				}
			}

			.months_title_more {
				font-size: 24rpx;
				line-height: 34rpx;
				color: $color;
			}
		}
		.months_content {
			display: flex;
			flex-wrap: wrap;
			.months_item {
				width: 33.3%;
				border: 5rpx solid #FFFFFF;
			}
		}
	}
	
	.hots_wrap {
		// display: flex;
		.hots_title {
			padding: 20rpx;
			text {
				border-left: 8rpx solid $color;
				padding-left: 20rpx;
				font-size: 34rpx;
				font-weight: 600;
			}
		}
		.hots_content {
			display: flex;
			flex-wrap: wrap;
			.hot_item {
				width: 33.33%;
				border: 5rpx solid #FFFFFF;
				image {
					
				}
			}
		}
	}
</style>
