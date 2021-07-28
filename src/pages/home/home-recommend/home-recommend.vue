<template>
	<view>
		<!-- 推荐开始 -->
		<view class="recommend_wrap">
			<view class="recommend_item" :key="item.id" v-for="(item, index) in recommends">
				<image :src="item.thumb" mode="widthFix"></image>
			</view>
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
	</view>
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
					"https://img2.baidu.com/it/u=3218269182,174840221&fm=26&fmt=auto&gp=0.jpg"
				]
			}
		},
		methods: {
			// changeImgUrl(i) {
			// 	return i.map((item) => {
			// 		for (var i = 0; i < 4; i++) {
			// 			// item.img = "https://himg.bdimg.com/sys/portraitn/item/public.1.8dc58f3e.iBmQxMQKHwHSrIvoIZ47Yw"
			// 			item.img = this.myImg[i]
			// 		}
			// 	})
			// 	return i
			// }
			changeImg (iarray) {
				for(var i=0; i<iarray.length; i++) {
					var index = Math.floor(Math.random()*4)
					// iarray[i].img = "https://img2.baidu.com/it/u=1814565549,2954866278&fm=26&fmt=auto&gp=0.jpg"
					iarray[i].img = this.myImg[index]
					
				}
			}
		},
		mounted() {
			this.request({
				url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
				data: {
					limit: 30,
					order: "hot",
					skip: 0
				}
			}).then((res) => {
				console.log(res)
				// 头部推荐模块数据
				this.recommends = res.data.res.homepage[1].items
				// 月份模块数据
				this.months = res.data.res.homepage[2]
				// 将时间戳改为 18号/月  moment.js
				this.months.MM = moment(this.months.stime).format("MM")
				this.months.DD = moment(this.months.stime).format("DD")
				console.log(this.changeImg(this.months.items))
				// this.months.items = this.changeImgUrl(this.months.items)
				// console.log(this.months)
			}).catch((err) => {
				console.log(err)
			})
		}
	}
</script>

<style lang="scss">
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
</style>
