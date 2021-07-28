<template>
	<view class="form">
		<view class="form_title">标题</view>
		<view class="form_content">
			<slot name="first"></slot>
			<slot name="second"></slot>
		</view>
	</view>
</template>

<script>
	export default{
		mounted() {
			console.log('mounted   myForm组件挂载完毕  ')
		}
	}
</script>

<style>
</style>
