<template>
	<view 
	@touchstart="handleTouchStart"
	@touchend="handleTouchEnd"
	>
		学习触屏事件
	</view>
</template>

<script>
	/*
	1给容器绑定两个触屏事件touchstart和Itouchend
	2用户按下屏幕事件
		1记录用户按下屏幕的时间Date.now（）时间戳返回1970-1-1到现在的亳秒数
		2记录用户按下屏幕的坐标x和y
	3用户离开屏幕事件
		1记录用户离开屏幕的时间Date.now（）
		2记录用户离开屏幕的坐标×和y
		3根据两个时间运算判断用户按下屏幕时长是否合法
		4根据两对坐标判断距离是否合法判断滑动的方向
	*/
	export default {
		data() {
			return {
				startTime: 0, // 按下的时间
				startX: 0, // 按下的坐标
				startY: 0
			}
		},
		methods: {
			// 用户按下屏幕
			handleTouchStart (event) {
				console.log("handleTouchStart 手指按下屏幕")
				// console.log("按下" + event.changedTouches[0].clientX)
				// console.log("按下" + event.changedTouches[0].clientY)
				this.startTime = Date.now()
				this.startX = event.changedTouches[0].clientX
				this.startY = event.changedTouches[0].clientY
			},
			handleTouchEnd (event) {
				console.log("handleTouchEnd 手指离开屏幕")
				// console.log("离开" + event.changedTouches[0].clientX)
				// console.log("离开" + event.changedTouches[0].clientY)
				const endTime = Date.now()
				const endX = event.changedTouches[0].clientX
				const endY = event.changedTouches[0].clientY
				// 判断按下的时长
				if(endTime - this.startTime > 2000) {
					return;
				}
				// 滑动的方向
				let direction = ""
				// 判断用户滑动的距离是否合法
				// 合法再判断滑动的方向
				if(Math.abs(endX-this.startX) > 10) {
					// 滑动方向
					direction = endX - this.startX > 0 ? "right" : "left"
				} else {
					return;
				}
				// 用户做了合法的滑动操作
				console.log(direction)
			}
		}
	}
</script>

<style>
	view {
		width: 100%;
		height: 500rpx;
		background-color: aqua;
	}
</style>
