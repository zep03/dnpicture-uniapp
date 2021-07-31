<template>
	<view class="video">
		<view class="video_tab"> 
			<view class="video_tab_title">
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
			<view class="video_tab_content">
				<view v-if="current < 4">
					<video-main :urlObj="{url: this.items[current].url, params: this.items[current].params}"></video-main>
				</view>
				<view v-if="current === 4">
					<video-category></video-category>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		uniSegmentedControl
	} from '@dcloudio/uni-ui'
	import videoMain from './video-main/video-main.vue'
	import videoCategory from './video-category/video-category.vue'
	export default {
		data() {
			return {
				items: [{
						title: '推荐',
						url: 'http://157.122.54.189:9088/videoimg/v1/videowp/featured',
						params: {
							limit: 30,
							skip: 0,
							order: 'hot'
						}
					},
					{
						title: '娱乐',
						url: 'http://157.122.54.189:9088/videoimg/v1/videowp/category/59b25abbe7bce76bc834198a',
						params: {
							limit: 30,
							skip: 0,
							order: 'new'
						}
					},
					{
						title: '最新',
						url: 'http://157.122.54.189:9088/videoimg/v1/videowp/videowp',
						params: {
							limit: 30,
							skip: 0,
							order: 'current'
						}
					},
					{
						title: '热门',
						url: 'http://157.122.54.189:9088/videoimg/v1/videowp/videowp',
						params: {
							limit: 30,
							skip: 0,
							order: 'hot'
						}
					},
					{
						title: '分类',
						url: 'http://157.122.54.189:9088/videoimg/v1/videowp/category',
						params: {}
					}
				],
				current: 0
			}
		},
		components: {
			uniSegmentedControl,
			videoMain,
			videoCategory
		},
		onLoad() {
			console.log('页面开始加载')
		},
		methods: {
			onClickItem(e) {
				if (this.current !== e.currentIndex) {
					this.current = e.currentIndex
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.video_tab {
		.video_tab_title {
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
		.video_tab_content {
			
		}
	}
</style>
