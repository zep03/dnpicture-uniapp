<template>
	<view>
		<!-- 用户信息开始 -->
		<view class="user_info">
			<view class="user_icon">
				<image :src="imgDetail.user.avatar" mode="widthFix"></image>
			</view>
			<view class="user_desc">
				<view class="user_name">{{ imgDetail.user.name }}</view>
				<view class="user_time">{{ imgDetail.cnTime }}</view>
			</view>
		</view>
		<!-- 用户信息结束 -->
		<!-- 高清大图开始 -->
		<view class="high_img">
			<swiper-action @swiperAction="handleSwiperAcion">
				<image :src="imgDetail.img" mode="widthFix"></image>
			</swiper-action>
		</view>
		<!-- 高清大图结束 -->
		<!-- 点赞 开始 -->
		<view class="user_rank">
			<view class="rank">
				<text class="iconfont icon-dianzan">{{ imgDetail.rank }}</text>
			</view>
			<view class="user_collect">
				<text class="iconfont icon-shoucang">收藏</text>
			</view>
		</view>
		<!-- 点赞 结束 -->
		<!-- 最新评论 开始 -->
		<view class="comment_hot">
			<view class="comment_title">
				<text class="iconfont icon-hot1"></text>
				<text class="comment_text">最新评论</text>
			</view>
			<view class="comment_list">
				<view class="comment_item" :key="item.id" v-for="item in comment">
					<!-- 用户信息 -->
					<view class="comment_user">
						<!-- 用户头像 -->
						<view class="user_icon">
							<image :src="item.user.avatar" mode="widthFix"></image>
							<!-- 用户名称 -->
							<view class="user_name">
								<view class="user_nickname">{{item.user.name}}</view>
								<view class="user_time">{{item.cnTime}}</view>
							</view>
							<!-- 用户徽章 -->
							<view class="user_badge">
								<image v-for="item2 in item.user.title" :key="item2.icon" :src="item2.icon" mode="">
								</image>
							</view>
						</view>
					</view>
					<!-- 评论数据 -->
					<view class="comment_desc">
						<view class="comment_content">{{ item.content }}</view>
						<view class="comment_like">
							<text class="iconfont icon-dianzan">{{ item.size }}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 最新评论 结束 -->
		<!-- 下载按钮 开始 -->
		<view class="download">
			<view class="download_btn" @click="handleDownload">下载图片</view>
		</view>
		<!-- 下载按钮 结束 -->
	</view>
</template>

<script>
	import moment from "moment"
	// 设置语言为中文
	moment.locale("zh-cn")
	import swiperAction from "@/components/swiperAction.vue"
	export default {
		data() {
			return {
				// 图片信息对象，包含用户头像
				imgDetail: {},
				hot: [], // 热门评论
				comment: [], //最新评论
				imgIndex: 0 //高清大图的索引
			}
		},
		components: {
			swiperAction
		},
		onLoad() {
			console.log(getApp().globalData.imgList)
			const {
				imgList,
				imgIndex
			} = getApp().globalData
			this.imgIndex = imgIndex
			// this.imgDetail = imgList[this.imgIndex]
			// // xxx 年前
			// this.imgDetail.cnTime = moment(this.imgDetail.atime * 1000).fromNow()

			// // 获取图片详情的id
			// // this.imgDetail.id
			// this.getComments(this.imgDetail.id)
			this.getData()
		},
		methods: {
			getComments(id) {
				this.request({
					url: `http://157.122.54.189:9088/image/v2/wallpaper/wallpaper/${id}/comment`
				}).then((res) => {
					console.log(res)
					this.hot = res.data.res.hot
					this.comment = res.data.res.comment
					res.data.res.comment.forEach(v => {
						v.cnTime = moment(v.atime*1000).fromNow()
					})
				})
			},
			
			
			getData() {
				const {
					imgList
				} = getApp().globalData
				this.imgDetail = imgList[this.imgIndex]
				// xxx 年前
				this.imgDetail.cnTime = moment(this.imgDetail.atime * 1000).fromNow()
				// 获取图片详情的id
				// this.imgDetail.id
				this.getComments(this.imgDetail.id)
			},
			// 自定义的组件的滑动事件
			handleSwiperAcion (e) {
				console.log(e)
				/* 
				 1. 用户左滑， index++
				 2. 用户右滑  index--
				 3. 判断数组是否越界的问题
				 */
				const {
					imgList
				} = getApp().globalData
				if (e.direction === "left" && this.imgIndex < imgList.length-1) {
					// 可以进行 左滑 ，加载下一张图片
					this.imgIndex++
					this.getData()
				} else if (e.direction === "right" && this.imgIndex > 0) {
					// 可以进行 右滑 ，加载上一张图片
					this.imgIndex--
					this.getData()
				} else {
					uni.showToast({
						title: "没有数据啦",
						icon: "none"
					})
				}
			},
			
			// 点击下载图片
			async handleDownload () {
				await uni.showLoading({
					title: "下载中"
				})
				
				// 1. 将图片下载到小程序的内存中
				const res = await uni.downloadFile({
					url: this.imgDetail.img
				})
				console.log(res)
				const {tempFilePath} = res[1]
				// 2. 将小程序内存中的临时文件下载到本地上
				const res2 = await uni.saveImageToPhotosAlbum({
					filePath: tempFilePath
				})
				// 3. 提示用户下载成功
				console.log("下载成功", res2)
				uni.hideLoading()
				uni.showToast({
					title: "下载成功"
					// icon
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.user_info {
		display: flex;
		align-items: center;
		padding: 20rpx;

		.user_icon {
			padding: 0 20rpx;

			image {
				width: 88rpx;
				border-radius: 50%;
			}
		}

		.user_desc {
			.user_name {
				color: #000000;
				font-weight: 600;
			}

			.user_time {
				color: #CCCCCC;
				font-size: 24rpx;
				padding: 10rpx 0;
			}
		}
	}

	.user_rank {
		display: flex;
		height: 80rpx;
		border-bottom: 5rpx solid #EEEEEE;

		.rank {
			display: flex;
			justify-content: center;
			align-items: center;
			flex: 1;

			.iconfont {}
		}

		.user_collect {
			display: flex;
			justify-content: center;
			align-items: center;
			flex: 1;

			.iconfont {}
		}
	}

	.comment_hot {
		margin-bottom: 120rpx;
		.comment_title {
			padding: 15rpx;

			.iconfont {
				color: red;
				font-size: 40rpx;
			}

			.comment_text {
				font-weight: 600;
				font-size: 28rpx;
				color: #666666;
				margin-left: 10rpx;
			}
		}

		.comment_list {
			.comment_item {
				border-bottom: 15rpx solid #EEEEEE;
				// 用户信息
				.comment_user {
					display: flex;
					padding: 20rpx 10rpx;
					.user_icon {
						display: flex;
						width: 35%;
						justify-content: center;
						align-items: center;
						
						image {
							border-radius: 50%;
							width: 40%;
							height: 90%;
						}
					}

					.user_name {
						flex: 1;
						margin-left: 10rpx;
						.user_nickname {
							color: #777;
						}

						.user_time {
							color: #ccc;
							font-size: 24rpx;
							padding: 5rpx;
						}
					}

					.user_badge {
						image {
							width: 40rpx;
							height: 40rpx;
							display: inline-block;
						}
					}
				}
				// 评论数据
				.comment_desc {
					display: flex;
					padding: 10rpx 0;
					.comment_content {
						flex: 1;
						padding-left: 15%;
						color: #000000;
					}
					.comment_like {
						text-align: right;
						margin-right: 15rpx;
						.icon-dianzan {
							
						}
					}
				}
			}
		}
	}
	
	.download {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		height: 120rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		.download_btn {
			width: 90%;
			height: 80%;
			background-color: $color;
			color: #FFFFFF;
			font-size: 40rpx;
			font-weight: 600;
			display: flex;
			justify-content: center;
			align-items: center;
		}
	}
</style>
