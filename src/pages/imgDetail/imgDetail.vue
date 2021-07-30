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
			<image :src="imgDetail.img" mode="widthFix"></image>
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
	</view>
</template>

<script>
	import moment from "moment"
	// 设置语言为中文
	moment.locale("zh-cn")
	export default {
		data() {
			return {
				// 图片信息对象，包含用户头像
				imgDetail: {},
				hot: [], // 热门评论
				comment: [] //最新评论
			}
		},
		onLoad() {
			console.log(getApp().globalData.imgList)
			const {
				imgList,
				imgIndex
			} = getApp().globalData
			this.imgDetail = imgList[imgIndex]

			// xxx 年前
			this.imgDetail.cnTime = moment(this.imgDetail.atime * 1000).fromNow()

			// 获取图片详情的id
			// this.imgDetail.id
			this.getComments(this.imgDetail.id)
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
</style>
