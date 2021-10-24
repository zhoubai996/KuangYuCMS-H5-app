<template>
	<view class="Box">	
			<view class="title">
				{{ section_title }}
			</view>
			<view class="content">
				<rich-text :nodes="content_text" class="content-box"></rich-text>
			</view>
			<button type="primary" @click="nextContent(nextKey)">点击加载下一页</button>
	</view>
</template>
<script>
export default{
	data(){
		return{
			section_title: '',
			content_text: '',
			id:'',
			key: '',
			nextKey: ''
			
		}
	},
	onLoad(e) {
		this.id = e.id
		this.key = e.key
		this.getArticle(this.getTop);
	},
	methods:{
		// 获取小说内容
		getArticle(callback) {
			uni.request({
						// #ifdef H5
						url: '/api/novel' + '/chapter',
						//  #endif
						// #ifndef H5
						url: 'https://book.zhoubaiwl.club/api/novel/chapter',
						// #endif
						method: 'GET',
						data: {
							id: this.id,
							key: this.key,
							api_key: 'zhoubai996'
						},
						success: res => {
							this.nextKey = res.data.next.id
							this.section_title = res.data.title
							this.content_text = res.data.content
							callback()
						},
						fail: err => {
							console.log('失败' + err)
						}
			})
		},
		// 加载下一页
		nextContent(id) {
			this.key = id
			this.getArticle(this.getTop)
		},
		// 返回顶部
		getTop() {
			uni.pageScrollTo({
			　　scrollTop: 0, 
				duration: 300,
				duration: 0
			})
		}
	}
}
</script>
<style lang="scss">
	.Box {
		padding: 30rpx;
		.title {
			height: 100rpx;
			max-height: 200rpx;
			font-size: 40rpx;
			text-align: center;
		}
		.content {
			margin-top: 20rpx;
			width: 100%;
			font-size: 40rpx;
			line-height: 70rpx;
		}
	}
</style>
