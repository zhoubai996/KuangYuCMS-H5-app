<template>
	<!-- 小说详情页 -->
	<view class="particulars" :style="'background: transparent url('+ bookParticulars.pic + ') repeat-x;'">
		<view class="books-top">
			<image :src="bookParticulars.pic | links"></image>
			<view class="right">
				<view class="title">
					{{ bookParticulars.title }}
				</view>
				<view class="info">
					<view class="author">
						{{ bookParticulars.author }}
					</view>
					<view>
						类型：{{ bookParticulars.ctitle }}
					</view>
					<view>
						状态：{{ bookParticulars.serialize_text }}
					</view>
					<view>
						阅读：{{ bookParticulars.hits }}
					</view>
					<view>
						字数：{{ bookParticulars.word / 10000 }}万字
					</view>
				</view>
			</view>
			<view class="statistical">
				<view class="downs">
					<text>{{ bookParticulars.digg.up }}</text>
					<text>顶</text>
				</view>
				<view class="partitions">
					|
				</view>
				<view class="ups">
					<text>{{ bookParticulars.digg.down }}</text>
					<text>踩</text>
				</view>
				<view class="partitions">
					|
				</view>
				<view class="hots">
					<text>{{ bookParticulars.recommend }}</text>
					<text>推荐票</text>
				</view>
			</view>
		</view>
		<view class="books-middle">
			<view class="synopsis"></view>
			<view class="synopsis-content" v-html="bookParticulars.content"></view>
		</view>
		<view class="books-bottom">
			<view class="latest-chapter" @click="readbook(bookParticulars.source_id, bookParticulars.chapter_id)">
				<view class="chapter-box">
					<view class="newest">
						最新
					</view>
					<view class="updataTime">
						更新于：{{ bookParticulars.update_time | updateTime }}
					</view>
				</view>
				<view class="newest-title">
					{{ bookParticulars.chapter_title }}
				</view>
			</view>
			<view class="bottom-nav">
				<view class="add-bookcase">
					<button type="primary" disabled>加入书架</button>
				</view>
				<view class="view-directory">
					<button type="primary" @click="goTodirectories(bookId)">查看目录</button>
				</view>
				<view class="click-readbook">
					<button type="primary" disabled>点击阅读</button>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				bookId: 0,
				bookParticulars: {},
				updataTime: ''
			}
		},
		onLoad(options) {
			this.bookId = options.id
			this.getParticulars()
		},
		methods: {
			// 获取数据
			getParticulars() {
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
								// console.log(res.data)
								this.bookParticulars = res.data
							},
							fail: err => {
								// console.log(err)
							}
				})	
			},
			// 最新章节点击跳转阅读
			readbook(id, key) {
				uni.navigateTo({
					url: '../readbook/readbook?id=' + id + '&key=' + key
				})
			},
			// 前往目录页面
			goTodirectories(val) {
				uni.navigateTo({
					url: '../directories/directories?id=' + val
				})
			}
		},
		filters: {
			// 过滤封面
			links(val) {
				if(val.indexOf('/uploads/novel/') != -1) {
					return 'https://img.codestu.cn/2021/10/23/a404521de7105.png'
				} else {
					return val
				}
			},
			// 时间过滤
			updateTime(val) {
				let time = new Date(val * 1000)
				let nowTime = new Date()
				let year = time.getFullYear()
				let nowYear = nowTime.getFullYear()
				let month = time.getMonth()
				let nowMonth = nowTime.getMonth()
				let day = time.getDate()
				let nowDay = nowTime.getDate()
				let hours = time.getHours()
				let nowHours = nowTime.getHours()
				let min = time.getMinutes()
				let nowMin = nowTime.getMinutes()
				let nMonth = month.toString().padStart(2, 0)
				let nDay = day.toString().padStart(2, 0)
				let nHours = hours.toString().padStart(2, 0)
				let nMin = min.toString().padStart(2, 0)
				if (nowYear === year && nowMonth === month && nowDay === day && nowHours === hours) {
					return (nowMin - min) + '分钟前'
				} else if (nowYear === year && nowMonth === month && nowDay === day) {
					return (nowHours - hours) + '小时前'
				} else if (nowYear === year && nowMonth === month) {
					if (nowDay - day === 1) {
						return '昨天'
					} else if (nowDay - day === 2) {
						return '前天'
					}
				} else {
					// 检查是否小于两位数，小于就在前面补0
					return `${year}-${nMonth}-${nDay}  ${nHours}:${nMin}`
				}
				
			}
		}
	}
	
</script>

<style lang="scss">
.books-top {
	position: relative;
	display: flex;
	height: 500rpx;
	width: 100%;
	background-color: rgba(255, 255, 255, .8);
	image {
		height: 360rpx;
		min-width: 260rpx;
		max-height: 380rpx;
		width: 260rpx;
		margin: 20rpx 20rpx;
		border-radius: 5px;
		background-color: #eee;
	}
	.right {
		.title {
			font-size: 36rpx;
			margin-top: 20rpx;
		}
		.info {
			view {
				margin: 10rpx;
			}
			.author {
				color: #DEA19E;
			}
		}
	}
	.statistical {
		position: absolute;
		display: flex;
		align-items: center;
		justify-content: space-around;
		bottom: -60rpx;
		right: 5%;	
		height: 100rpx;
		width: 90%;
		border-radius: 5px;
		box-shadow: 0px 0px 5px -2px;
		background-color: #FFFFFF;
		.downs {
			height: 100%;
			width: 30%;
			text-align: center;
			line-height: 100rpx;
		}
		.ups {
			height: 100%;
			width: 30%;
			text-align: center;
			line-height: 100rpx;
		}
		.hots {
			height: 100%;
			width: 30%;
			text-align: center;
			line-height: 100rpx;
		}
		.partitions {
			height: 100%;
			width: 5%;
			text-align: center;
			line-height: 100rpx;
			font-size: 40rpx;
			color: #eee;
		}
	}
}
.books-middle {
	height: 360rpx;
	width: 100%;
	background-color: #ffffff;
	.synopsis {
		height: 60rpx;
		line-height: 60rpx;
		font-size: 33rpx;
		text-align: center;
	}
	.synopsis-content {
		margin: 20rpx;
		text-indent: 2em;
		font-size: 29rpx;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 7;
		-webkit-box-orient: vertical;
	}
}
.books-bottom {
	margin-top: 10rpx;
	width: 100%;
	.latest-chapter {
		margin: 10rpx 0;
		padding: 10rpx;
		height: 140rpx;
		width: 100%;
		line-height: 60rpx;
		background-color: #fff;
		box-shadow: 0 0 5rpx -3rpx;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		.chapter-box {
			display: flex;
			align-items: center;
			justify-content: space-between;
			height: 60rpx;
			.newest {
				font-size: 40rpx;
				font-weight: 900;
			}
			.updataTime {
				margin-right: 20rpx;
				font-size: 29rpx;
				color: #BDBDBD;
			}
		}
		.newest-title {
			position: relative;
			height: 100rpx;
			font-size: 36rpx;
			color: #E53935;
			line-height: 100rpx;			
		}
		.newest-title::after {
			position: absolute;
			top: 50%;
			margin-top: -25rpx;
			right: 20rpx;
			content: '最新';
			width: 80rpx;
			height: 45rpx;
			color: #fff;
			font-size: 33rpx;
			line-height: 45rpx;
			text-align: center;
			border-radius: 5px;
			background-color: #D32F2F;
		}
	}
	.bottom-nav {
		position: fixed;
		bottom: 0;
		display: flex;
		align-items: center;
		justify-content: space-around;
		height: 100rpx;
		width: 100%;
		box-shadow: 0 0 5rpx -3rpx;
		.add-bookcase {
			
		}
		.view-directory {
			
		}
	}
}
</style>
