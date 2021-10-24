<template>
	<view class="books-content">
		<view class="books-box" v-for="item in booksLists" :key="item.id" @click="goToParticulars(item.id)">
			<image :src="item.pic | links"></image>
			<view class="right">
				<view class="title">
					{{ item.title }}
				</view>
				<view class="info">
					<text>{{ item.author }} · {{ classIfy }}</text><br>
					<rich-text :nodes="item.content" class="books-content"></rich-text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				booksLists: [],
				category: 0,
				classIfy: '',
				pageIndex: 1,
				bookImage: ''
			}
		},
		onReachBottom () {
		            // console.log('触底了')
					this.pageIndex++
					this.getBooksList()
		},
		onLoad(options) {
			this.category = options.id,
			this.classIfy = options.title
			this.getBooksList()
		},
		methods: {
			getBooksList() {
				uni.request({
							// #ifdef H5
							url: '/api/novel' + '/listsapi',
							//  #endif
							// #ifndef H5
							url: 'https://book.zhoubaiwl.club/api/novel/listsapi',
							// #endif
							method: 'GET',
							data: {
								category: this.category,
								page: this.pageIndex
							},
							success: res => {
								// console.log(res.data.pic)
								this.bookImage = res.data.pic
								this.booksLists = [...this.booksLists, ...res.data]
							},
							fail: err => {
								// console.log('失败'+err)
							}
						})
			
			},
			// 跳转至小说详情页
			goToParticulars(val) {
				uni.navigateTo({
					url: '../particulars/particulars?id=' + val
				})
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
.books-content {
	background-color: #F8F8F8;
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
