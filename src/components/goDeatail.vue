<template>
	<view @click="handleClick">
		组件
		<slot></slot>
	</view>
</template>

<script>
	export default {
		props: {
			itemList: {
				type: Array,
				required: true
			},
			index: Number
		},
		methods: {
			handleClick() {
				// 1.将数据缓存下来
				getApp().globalData.imgList = this.itemList
				getApp().globalData.imgIndex = this.index
				// 2.再实现点击跳转页面
				uni.navigateTo({
					url: "/pages/imgDetail/imgDetail"
				})
			}
		}
		// mounted() {
		// 	console.log(itemList)
		// 	console.log(index)
		// }
	}
</script>

<style>
</style>
