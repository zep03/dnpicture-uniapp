<template>
	<view>
		<!-- 把 props 中的src看成是 data中的变量一样来使用 -->
		<image :src="mysrc" @click="handleClick"></image>
	</view>
</template>

<script>
	export default {
		// 声明一下要接收的父组件传递过来的属性
		props: {
			mysrc: {
				type: String,
				required: true
			}
		},
		methods: {
			handleClick () {
				console.log('11111')
				// 子向父传递数据 通过触发事件
				this.$emit('srcChange', this.mysrc)
			}
		}
	}
</script>

<style>
</style>
