<template>
	<view class="content">
		<!-- 图文卡片模式 -->
		<uni-card
		    title="更新记录"
		    mode="style"
		    :is-shadow="true"
		    thumbnail="https://pic.imgdb.cn/item/5fba828db18d627113b185cc.jpg"
		    extra="昼白读书网 1.4.0"
		    note="Tips: 期待每一次的更新"
		>
				<view>优化了小说阅读页的加载动画，提升体验</view>
				<view>优化了搜索功能</view>
		</uni-card>
		<button type="default" @click="getToClassify">跳转到分类页面</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {
			// 跳转到分类页面
			getToClassify() {
				uni.navigateTo({
					url: '../classify/classify'
				})
			}
		}
	}
</script>

<style>
	
</style>
