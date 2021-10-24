<template>
	<view>
		<view class="search-box">
			<input type="text" v-model="keyword" placeholder="请输入关键字..." confirm-type="搜索"/>
			<button type="primary" @click="search">搜索</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				keyword: '',
				page: 1
			}
		},
		methods: {
			getSearch(str) {
				uni.request({
							// #ifdef H5
							url: '/api/novel' + '/api/novel/searchapi',
							//  #endif
							// #ifndef H5
							url: 'https://book.zhoubaiwl.club/api/novel/api/novel/searchapi',
							// #endif
							method: 'GET',
							data: {
								keyword: str,
								page: this.page
							},
							success: res => {
								console.log(res.data)
								
							},
							fail: err => {
								console.log('失败')
							}
						})
			
			},
			// 点击搜索
			search() {
				this.getSearch(this.keyword)
			}
		}
	}
</script>

<style lang="scss">
.search-box {
	height: 120rpx;
	width: 100%;
	background-color: pink;
}
</style>
