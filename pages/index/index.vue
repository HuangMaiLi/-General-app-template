<template>
	<view class="center">
		<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
			<swiper-item v-for="(item,index) in list" :key="index">
				<view class="swiper-item" @click="goDetail(item)">
					<image :src="item.img_src" class="swiper-item-img"></image>
				</view>
			</swiper-item>
		</swiper>
		<view class="uni-grid-12 uni-common-mt">
			<view class="uni-grid-12-item" hover-class="uni-grid-12-item-hover" v-for="(item,index) in grids" :key="index" @tap="tapIcon(item)">
				<image class="uni-grid-12-image tag-img" :src="item.icon"></image>
				<text class="uni-grid-12-text icon-text">{{item.name}}</text>
			</view>
		</view>
		<view class="uni-title uni-common-mt">
			<view class="uni-flex uni-row">
				<view class="flex-item" @tap="openLeftList()">
					<image class="flex-item-img" src="../../static/dgdfh.png"></image>
				</view>
			</view>
			<view class="uni-flex uni-column">
				<view class="flex-item flex-item-V" @tap="openMore()">
					<image class="flex-item-v-img" src="../../static/yu.png"></image>
				</view>
				<view class="flex-item flex-item-V" @tap="openPicture()">
					<image class="flex-item-v-img" src="../../static/lvyou.png"></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [],
				Newlist: [],
				refreshing: false,
				fetchPageNum: 1,
				indicatorDots: true,
				autoplay: true,
				interval: 2000,
				duration: 500,
				newsList: [],
				tabIndex: 0,
				grids: [{
					name: '最新',
					id: 0,
					ref: 'new',
					icon: 'http://placehold.it/150x150'
				}, {
					name: '大公司',
					id: 23,
					ref: 'company',
					icon: 'http://placehold.it/150x150'
				}, {
					name: '内容',
					id: 223,
					ref: 'content',
					icon: 'http://placehold.it/150x150'
				}, {
					name: '消费',
					id: 221,
					ref: 'xiaofei',
					icon: 'http://placehold.it/150x150'
				}, {
					name: '娱乐',
					id: 225,
					ref: 'yule',
					icon: 'http://placehold.it/150x150'
				}, {
					name: '区块链',
					id: 208,
					ref: 'qukuailian',
					icon: 'http://placehold.it/150x150'
				}, ],
			}
		},
		onLoad: function() {
			this.getImgData();
			this.getNewData()
		},
		methods: {
			getImgData() {
				uni.request({
					url: this.$serverUrl + '/api/picture/posts.php?page=' + (this.refreshing ? 1 : this.fetchPageNum) +
						'&per_page=5',
					success: (ret) => {
						if (ret.statusCode !== 200) {
							console.log("失败!");
						} else {
							this.list = ret.data;
						}
					},
					fail: (ret) => {
						uni.onNetworkStatusChange(function(res) {
							console.log(res.isConnected);
							console.log(res.networkType);
						});
					},
					complete: (ret) => {
						this.loading = false;
					}
				});
			},
			changeIndicatorDots(e) {
				this.indicatorDots = !this.indicatorDots
			},
			changeAutoplay(e) {
				this.autoplay = !this.autoplay
			},

			getNewData() {
				uni.request({
					url: 'https://unidemo.dcloud.net.cn/api/news',
					success: (result) => {
						if (result.statusCode == 200) {
							this.Newlist = result.data;
						}
					}
				})
			},
			tapIcon(e) {
				uni.navigateTo({
					url: '../newlist/newlist?iconId=' + e.id + "&name=" + e.name,
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			openLeftList() {
				uni.navigateTo({
					url: '../leftlist/leftlist',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			openMore() {
				uni.navigateTo({
					url: '../moreList/moreList',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			openPicture() {
				uni.navigateTo({
					url: '../picture/picture',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	view {
		display: flex;
	}

	.uni-flex {
		width: 100%;
		background: #FFFFFF;
	}

	.icon-text {
		margin-top: 26upx;
	}

	.tag-img {
		border-radius: 60upx;
		width: 110upx;
		height: 110upx;
	}

	.title {
		font-size: 36upx;
		color: #8f8f94;
	}

	.swiper {
		height: 300upx;
		margin-bottom: 26upx;
	}

	.swiper-item {
		display: block;
		height: 300upx;
		line-height: 300upx;
		text-align: center;
	}

	.swiper-item-img {
		width: 100%;
		/* width: 750upx; */
		display: block;
		height: 100%;
	}

	.swiper-list {
		margin-top: 40upx;
		margin-bottom: 0;
	}

	.uni-padding-wrap {
		padding: 0;
		width: 100%;
	}

	.uni-common-mt {
		margin: 15upx 0upx;
		position: relative;
	}

	.flex-item {
		width: 100%;
		height: 300upx;
		text-align: center;
		line-height: 300upx;
	}

	.flex-item-V {
		width: 100%;
		height: 150upx;
		text-align: center;
		line-height: 150upx;
	}

	.flex-item-img {
		width: 100%;
		height: 300upx;
	}
	.flex-item-v-img {
		width: 100%;
		height: 150upx;
	}

	.scroll-Y {
		height: 300upx;
	}

	.scroll-view_H {
		white-space: nowrap;
		width: 100%;
	}

	.scroll-view-item {
		height: 300upx;
		line-height: 300upx;
		text-align: center;
		font-size: 36upx;
	}

	.scroll-view-item_H {
		display: inline-block;
		width: 100%;
		line-height: 300upx;
		text-align: center;
		font-size: 36upx;
	}
</style>
