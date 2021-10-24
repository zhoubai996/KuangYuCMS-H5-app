<template>
	<view class="content">
		<view class="search-box"></view>
		<view class="pics">
			<scroll-view class="left" scroll-y>
				<view :class="active===index? 'active': ''" v-for="(item, index) in classIfy" :key="item.id" @click="actives(index)">{{item.title}}</view>
			</scroll-view>
			<scroll-view class="right" scroll-y>
				<view class="item" v-for="item in secondData" :key="item.id" @click="booksList(item.id, item.title)">
					<text>{{ item.title }}</text>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				classIfy: [],
				active: 0,
				secondData: []
			}
		},
		onLoad() {
			this.getClassIfy()
		},
		methods: {
			getClassIfy() {
				uni.request({
							// #ifdef H5
							url: '/api/novel' + '/category',
							//  #endif
							// #ifndef H5
							url: 'https://book.zhoubaiwl.club/api/novel/category',
							// #endif
							method: 'GET',
							success: res => {
								this.classIfy = res.data
								// 初次加载先执行一次
								this.actives(0)
							},
							fail: err => {
								// console.log(err)
							}
						})

			},
			actives(val) {
				this.active = val
				// 获取右侧数据
				uni.request({
							// #ifdef H5
							url: '/api/novel' + '/category',
							//  #endif
							// #ifndef H5
							url: 'https://book.zhoubaiwl.club/api/novel/category',
							// #endif
							method: 'GET',
							success: res => {
								this.secondData = res.data[val].subor
							},
							fail: err => {
								// console.log(err)
							}
						})
			},
			// 点击跳转到分类书籍列表
			booksList(val, title) {
				uni.navigateTo({
					url: '../booklists/booklists?id=' + val + '&title=' + title,
				})
			}
		}
	}
</script>

<style lang="scss">
	page {
		height: 100%;
		}
	.pics {
		height: 100%;
		display: flex;
		.left {
			position: fixed;
			top: 0rpx;
			left: 0;
			width: 200rpx;
			height: 100%;
			border-right: 1px solid #eee;
			background-color: #eee;
			view {
				height: 60px;
				text-align: center;
				line-height: 60px;
				color: #333333;
				font-size: 30rpx;
				border-top: 1px solid #eee;
			}
			.active {
				background-color: #b50e30;
				color: #FFFFFF;
			}
		}
		.right {
			position: fixed;
			top: 0;
			left: 210rpx;
			height: 100%;
			width: 530rpx;
			margin: 0 auto;
			.item {
				height: 100rpx;
				width: 520rpx;
				margin:10rpx 5rpx;
				border-radius: 5px;
				background-color: #eee;
				text-align: center;
				text {
					font-size: 40rpx;
					line-height: 100rpx;
					
				}
			}
		}
	}

</style>
