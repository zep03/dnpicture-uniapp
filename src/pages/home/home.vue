<template>
	<view class="home">
		<view class="home_tab"> 
			<view class="home_tab_title">
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
			<view class="home_tab_content">
				<view v-if="current === 0">
					<home-recommend></home-recommend>
				</view>
				<view v-if="current === 1">
					<home-category></home-category>
				</view>
				<view v-if="current === 2">
					<home-new></home-new>
				</view>
				<view v-if="current === 3">
					<home-album></home-album>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import homeAlbum from './home-album/home-album.vue'
	import homeCategory from './home-category/home-category.vue'
	import homeNew from './home-new/home-new.vue'
	import homeRecommend from './home-recommend/home-recommend.vue'
	import {
		uniSegmentedControl
	} from '@dcloudio/uni-ui'
	export default {
		data() {
			return {
				items: [{
						title: '推荐'
					},
					{
						title: '分类'
					},
					{
						title: '最新'
					},
					{
						title: '专辑'
					},
				],
				current: 1
			}
		},
		components: {
			homeAlbum,
			homeCategory,
			homeNew,
			homeRecommend,
			uniSegmentedControl
		},
		computed: {

		},
		onLoad() {
			
	
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

<style lang="scss">
	.home_tab {
		.home_tab_title {
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
		.home_tab_content {
			
		}
	}
</style>
