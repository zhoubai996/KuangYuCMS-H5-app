<template>
	<!-- 全部章节页 -->
	<view class="books-directories">
		<view class="directories" v-for="(val, key) in bookParticulars" :key="key" @click="readBooks(bookPar.source_id, key)">
			{{ val.title }}
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				bookId: 0,
				bookParticulars: {},
				bookPar: {}	
			}
		},
		onLoad(options) {
			this.bookId = options.id
			this.getDirectories()
		},
		// 监听下拉刷新
		onPullDownRefresh () {
		    this.getDirectories(this.stop)
		},
		methods: {
			getDirectories(callback) {
				uni.request({
							// #ifdef H5
							url: '/api/novel' + '/contentapi',
							//  #endif
							// #ifndef H5
							url: 'https://book.zhoubaiwl.club/api/novel/contentapi',
							// #endif
							method: 'GET',
							data: {
								id: this.bookId
							},
							success: res => {
								// console.log(res.data.chapter)
								this.bookParticulars = res.data.chapter
								this.bookPar = res.data
								callback()
							},
							fail: err => {
								// console.log('失败' + err)
							}
				})
			},
			// 点击跳转到阅读页面
			readBooks(id, key) {
				uni.navigateTo({
					url: '../readbook/readbook?id=' + id + '&key=' + key
				})
			},
			// 停止下拉刷新
			stop() {
				uni.stopPullDownRefresh()
			}
		}
	}
</script>

<style lang="scss">
.books-directories {
	padding: 10rpx;
	.directories {
		margin-bottom: 10rpx;
		padding: 10rpx;
		height: 80rpx;
		width: 710rpx;
		line-height: 80rpx;
		background-color: #fff;
		border-radius: 5rpx;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}
}

</style>
