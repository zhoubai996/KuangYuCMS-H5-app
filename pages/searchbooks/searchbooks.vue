<template>
	<view>
		<view class="search-box">
			<!-- v-model 用法 -->
			<uni-search-bar 
			@cancel="cancel" 
			@clear="clear"
			v-model="keyword" 
			>
			</uni-search-bar>
			<button type="primary" @click="search">搜索</button>
		</view>
		<!-- 小说列表页 -->
		<view class="box-books">
			<view class="tips" v-if="flag">
				请输入内容
			</view>
			<view class="books-box" v-for="item in books" :key="item.id" @click="goToParticulars(item.id)">
				<image :src="item.pic | links"></image>
				<view class="right">
					<view class="title">
						{{ item.title }}
					</view>
					<view class="info">
						<text>{{ item.author }} · {{ item.word / 10000 }}万字</text><br>
						<rich-text :nodes="item.content" class="books-content"></rich-text>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				keyword: '',
				page: 1,
				books: [],
				flag: false
			}
		},
		onReachBottom () {
			// console.log('触底了')
			this.page++
			this.getSearch()
		},
		methods: {
			getSearch() {
				uni.request({
							// #ifdef H5
							url: '/api/novel' + '/searchapi',
							//  #endif
							// #ifndef H5
							url: 'https://book.zhoubaiwl.club/api/novel/searchapi',
							// #endif
							method: 'GET',
							data: {
								keyword: this.keyword,
								page: this.page
							},
							success: res => {
								// console.log(res.data)
								this.books = [...this.books, ...res.data]								
							},
							fail: err => {
								console.log('失败')
							}
						})
			
			},
			// 点击搜索
			search() {
				this.books = []
				if(this.keyword === '') {
					this.flag = true
				} else {
					this.getSearch()	
				}
				
			},
			// 跳转至小说详情页
			goToParticulars(val) {
				uni.navigateTo({
					url: '../particulars/particulars?id=' + val
				})
			},
			// 点击取消后清空小说列表
			cancel() {
				this.books = []
			},
			// 点击清除按钮清空输入的文字
			clear() {
				this.keyword = ''
			}
		},
		filters: {
			links(val) {
				if(val.indexOf('/uploads/novel/') != -1) {
					return 'https://img.codestu.cn/2021/10/23/a404521de7105.png'
				} else {
					return val
				}
			}
		}
	}
</script>

<style lang="scss">
.search-box {
	height: 120rpx;
	width: 100%;
	background-color: #fff;
}
.box-books {
	margin-top: 90rpx;
	width: 100%;
	.tips {
		height: 800rpx;
		width: 100%;
		text-align: center;
		font-size: 36rpx;
		line-height: 800rpx;
		color: red;
	}
	.books-box {
			display: flex;
			height: 280rpx;
			width: 730rpx;
			margin: 10rpx auto;
			border-radius: 5px;
			background-color: #FFFFFF;
			image {
				height: 260rpx;
				min-width: 210rpx;
				max-height: 280rpx;
				width: 210rpx;
				margin: 10rpx 10rpx;
				border-radius: 5px;
				background-color: #B3E5FC;
			}
			.right {
				margin-left: 15rpx;
				.title {
					margin-top: 10rpx;
					font-size: 33rpx;
				}
				.info {
					margin-top: 10rpx;
					font-size: 28rpx;
					text:nth-child(2) {
						margin-left: 30rpx;
					}
					.books-content {
						text-indent: 2em;
						margin-top: 10rpx;
						overflow: hidden;
						text-overflow: ellipsis;
						display: -webkit-box;
						-webkit-line-clamp: 4;
						-webkit-box-orient: vertical;
					}
				}
			}
	}
}

</style>
