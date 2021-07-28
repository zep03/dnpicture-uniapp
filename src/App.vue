<script>
	export default {
		onLaunch: function() {
			console.log('App Launch 应用启动')
		},
		onShow: function() {
			console.log('App Show 应用显示')
		},
		onHide: function() {
			console.log('App Hide 应用隐藏')
		}
	}
</script>

<style>
	/*每个页面公共css */
	@import url("./style/base.wxss");
	@import url("./style/iconfont.wxss");
</style>
