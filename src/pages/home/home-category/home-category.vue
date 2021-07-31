<template>
	<view class="home_category">
		<navigator class="cate_item"
		v-for="item in category"
		:url="`/pages/imgCategory/imgCategory?id=${item.id}`"
		:key="item.id">
			<image :src="item.cover" mode="aspectFill"></image>
			<view class="cate_name">{{ item.name }}</view>
		</navigator>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				category: [], // 分类数据
				// 模拟的图片数组
				myImg: [
					"https://img0.baidu.com/it/u=1000551505,2077899926&fm=26&fmt=auto&gp=0.jpg",
					"https://img0.baidu.com/it/u=1532752388,171944695&fm=26&fmt=auto&gp=0.jpg",
					"https://img2.baidu.com/it/u=2026215452,3463309435&fm=26&fmt=auto&gp=0.jpg",
					"https://img0.baidu.com/it/u=4255272445,3536412390&fm=26&fmt=auto&gp=0.jpg",
					"https://img2.baidu.com/it/u=3589748713,2051752919&fm=26&fmt=auto&gp=0.jpg",
					"https://img0.baidu.com/it/u=1156363733,1145018515&fm=26&fmt=auto&gp=0.jpg",
					"https://img1.baidu.com/it/u=1572607292,2004901445&fm=26&fmt=auto&gp=0.jpg",
					"https://img1.baidu.com/it/u=2629213230,2545258637&fm=26&fmt=auto&gp=0.jpg",
					"https://img0.baidu.com/it/u=1817143901,1168063195&fm=26&fmt=auto&gp=0.jpg",
					"https://img2.baidu.com/it/u=1814565549,2954866278&fm=26&fmt=auto&gp=0.jpg",
					"https://img0.baidu.com/it/u=103721101,4076571305&fm=26&fmt=auto&gp=0.jpg",
					"https://img1.baidu.com/it/u=3771357394,1518226081&fm=26&fmt=auto&gp=0.jpg",
					"https://img1.baidu.com/it/u=525722049,125546548&fm=26&fmt=auto&gp=0.jpg",
					"https://img2.baidu.com/it/u=3566088443,3713209594&fm=26&fmt=auto&gp=0.jpg",
					"https://img1.baidu.com/it/u=3886212450,854269223&fm=26&fmt=auto&gp=0.jpg"
				]
			}
		},
		mounted() {
			// 修改页面的标题
			uni.setNavigationBarTitle({
				title: "分类"
			})
			
			this.getList()
		},
		methods: {
			getList() {
				this.request({
					url: "http://157.122.54.189:9088/image/v1/vertical/category"
				}).then((res) => {
					console.log(res)
					this.category = res.data.res.category
					this.changeImg(this.category)
				})
			},
			// 改变接口的imgurl
			changeImg (iarray) {
				for(var i=0; i<iarray.length; i++) {
					var index = Math.floor(Math.random()*10)
					// iarray[i].img = "https://img2.baidu.com/it/u=1814565549,2954866278&fm=26&fmt=auto&gp=0.jpg"
					iarray[i].cover = this.myImg[index]
					
				}
			},
			
		}
	}
</script>

<style  lang="scss" scoped>
	.home_category {
		display: flex;
		flex-wrap: wrap;
		.cate_item {
			width: 33.33%;
			position: relative;
			border: 5rpx solid #FFFFFF;
			image {
				height: 240rpx;
			}
			
			.cate_name {
				position: absolute;
				width: 100%;
				height: 50rpx;
				left: 0;
				bottom: 10rpx;
				color: #FFFFFF;
				background-image: linear-gradient(to right top, rgba(0,0,0,.2), rgba(0,0,0,0));
				font-size: 40rpx;
				// display: flex;
				// align-items: center;
				text-align: center;
				// padding-left: 20rpx;
			}
		}
	}

</style>
