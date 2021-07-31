<template>
	<view class="video_play">
		<image :src="videoObj.img" mode=""></image>
		<!-- 工具栏 开始-->
		<view class="video_tool">
			<view @click="handleMuted" :class="['iconfont', muted ? 'icon-voice_stop':'icon-shengyin']"></view>
			<view class="iconfont icon-zhuanfa">
				<button open-type="share"></button>
			</view>
			
		</view>
		<!-- 工具栏 结束-->
		<!-- 视频开始 -->
		<view class="video_wrap">
			<video :muted="muted" :src="videoObj.video" objectFit="fill"></video>
		</view>
		<!-- 视频结束 -->
		<!-- 下载开始 -->
		<view class="download">
			<view class="download_btn" @click="handleDownload">下载高清视频</view>
		</view>
		<!-- 下载结束 -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				videoObj: {},
				muted: false // 是否静音
			}
		},
		onLoad() {
			console.log(getApp().globalData)
			this.videoObj = getApp().globalData.video
		},
		methods: {
			// 开关声音
			handleMuted () {
				this.muted = !this.muted
			},
			// 下载视频
			async handleDownload () {
				await uni.showLoading({
					title: '下载中'
				})
				
				// 1. 将视频下载到小程序内存中
				const { tempFilePath } = (await uni.downloadFile({
					url: this.videoObj.video
				}))[1]
				// 2. 将内存中的文件 下载到本地
				await uni.saveVideoToPhotosAlbum({
					filePath: tempFilePath
				})
				
				uni.hideLoading()
				
				await uni.showToast({
					title: '下载成功'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.video_play {
		position: relative;
		image {
			position: absolute;
			width: 100vw;
			height: 100vh;
			z-index: -1;
			// css3滤镜
			filter: blur(10px);
		}
		.video_tool {
			height: 80rpx;
			display: flex;
			justify-content: flex-end;
			.iconfont {
				width: 80rpx;
				color: #FFFFFF;
				font-size: 50rpx;
				border-radius: 40rpx;
				background-color: rgba(0,0,0,.2);
				display: flex;
				align-items: center;
				justify-content: center;
				margin-right: 20rpx;
			}
			.icon-zhuanfa {
				position: relative;
				button {
					position: absolute;
					width: 100%;
					height: 100%;
					opacity: 0;
				}
			}
		}
		.video_wrap {
			display: flex;
			justify-content: center;
			align-items: center;
			video {
				width: 360rpx;
				height: 600rpx;
			}
		}
		
		.download {
			display: flex;
			justify-content: center;
			align-items: center;
			margin-top: 30rpx;
			.download_btn {
				width: 360rpx;
				height: 80rpx;
				border-radius: 40rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				color: #FFFFFF;
				border: 3rpx solid #FFFFFF;
				background-color: rgba(0,0,0,.2);
			}
		}
	}
</style>
