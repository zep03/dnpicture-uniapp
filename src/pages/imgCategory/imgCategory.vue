<template>
	<view class="category">
		<view class="category_tab"> 
			<view class="category_tab_title">
				<view class="title_inner">
					<uni-segmented-control :current="current"
					 :values="items.map((item) => { return item.title})" @clickItem="onClickItem"
					  styleType="text"
						activeColor="#d4237a">
					</uni-segmented-control>
				</view>
				<view class="search_img">
					<image src="../../static/icon/_search.png"></image>
				</view>
			</view>
			<scroll-view @scrolltolower="handleScrolltolower" scroll-y enable-flex class="category_tab_content">
				<view class="cate_item"
				v-for="item in vertical"
				:key="item.id">
					<image :src="item.img" mode="aspectFill"></image>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	import {
		uniSegmentedControl
	} from '@dcloudio/uni-ui'
	export default {
		data() {
			return {
				items: [{
						title: '最新',
						order: 'new'
					},
					{
						title: '热门',
						order: 'hot'
					}
				],
				current: 0, // 当前激活的标签页索引
				// 请求参数
				params: {
					limit: 30,
					skip: 0,
					order: "new"
				},
				id: 0,
				vertical: [], // 页面显示数据
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
				],
				hasMore: true // 是否还有下一页数据
			}
		},
		components: {
			uniSegmentedControl
		},
		onLoad(options) {
			this.id = options.id
			this.getList()
		},
		methods: {
			onClickItem(e) {
				/* 
				 1. 根据被点击的标题，切换标题
				 2. 其替换order
				 3. 重新发送请求
				 */
				if (this.current !== e.currentIndex) {
					this.current = e.currentIndex
				} else {
					// 当点击的是相同的标签时
					return
				}
				// 重置数据
				this.params.skip = 0
				this.vertical = []
				this.params.order = this.items[e.currentIndex].order
				this.getList()
			},
			getList () {
				this.request({
					url: `http://157.122.54.189:9088/image/v1/vertical/category/${this.id}/vertical`,
					data: this.params
				}).then((res) => {
					if (res.data.res.vertical.length ===0) {
						this.hasMore = false
						uni.showToast({
							title: "没有更多数据啦",
							icon: "none"
						})
						return
					}
					this.vertical = [...this.vertical, ...res.data.res.vertical]
					// console.log(res)
					this.changeImg(this.vertical)
				})
			},
			// 改变接口的imgurl
			changeImg (iarray) {
				for(var i=0; i<iarray.length; i++) {
					var index = Math.floor(Math.random()*10)
					// iarray[i].img = "https://img2.baidu.com/it/u=1814565549,2954866278&fm=26&fmt=auto&gp=0.jpg"
					iarray[i].img = this.myImg[index]
					
				}
			},
			// 加载下一页数据
			handleScrolltolower () {
				if(this.hasMore) {
					this.params.skip += this.params.limit
					this.getList()
				} else {
					uni.showToast({
						title: '没有更多数据啦',
						icon: "none"
					})
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.category_tab {
		.category_tab_title {
			display: flex;
			justify-content: center;
			align-items: center;
			.title_inner {
				width: 70%;
			}
			.search_img {
				width: 17px;
				height: 20px;
				padding-bottom: 1%;
				margin-left: 10rpx;
				image {
					width: 100%;
					height: 100%;
				}
			}
		}
		.category_tab_content {
			display: flex;
			flex-wrap: wrap;
			height: calc( 100vh - 36px );
			.cate_item {
				width: 33.33%;
				border: 5rpx solid #FFFFFF;
				image {
					width: 100%;
					height: 300rpx;
				}
			}
		}
	}
</style>
