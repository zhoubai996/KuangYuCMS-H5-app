<template>
	<view class="content">
		<button type="default" @click="getToSearch">跳转到搜索页面</button>
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
			// 跳转到搜索页面
			getToSearch() {
				uni.navigateTo({
					url: '../searchbooks/searchbooks'
				})
			},
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
