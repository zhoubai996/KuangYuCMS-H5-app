<template>
	<view class="Box">
		<orange-fullloading 
		text="加载中..." 
		textcolor="#FFA726" 
		textsize=40 
		v-if="orangeFlag">
		</orange-fullloading>
		<view class="title">
			{{ section_title }}
		</view>
		<view class="content">
			<rich-text :nodes="content_text" class="content-box"></rich-text>
		</view>
		<button type="primary" :loading="flag" 
		@click="nextContent(nextKey)">
		{{ text }}
		</button>
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
			nextKey: '',
			flag: false,
			text: '点击加载下一页',
			orangeFlag: true,
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
							// console.log(res.data)
							if (res.data === null) {
								this.flag = false
								return this.text = '没有章节了'
							}
							this.nextKey = res.data.next.id
							this.section_title = res.data.title
							this.content_text = res.data.content
							this.orangeFlag = false
							callback()
						},
						fail: err => {
							console.log('失败' + err)
						}
			})
		},
		// 加载下一页
		nextContent(id) {
			this.flag = true
			this.text = '加载中'
			this.key = id
			this.getArticle(this.getTop)
		},
		// 返回顶部
		getTop() {
			this.flag = false
			this.text = '点击加载下一页'
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
