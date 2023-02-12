<template>
	<view>
		<uni-list>
			<uni-list-item title="用户名" :rightText="username"></uni-list-item>
			<uni-list-item title="手机号" :rightText="phone"></uni-list-item>
			<uni-list-item title="小区名" :rightText="community"></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: '',
				phone: '',
				community: ''
			}
		},
		onLoad() {
			if (uni.getStorageSync('username') !== '') {
				this.username = uni.getStorageSync('username')
				this.phone = uni.getStorageSync('phone')
				this.community = uni.getStorageSync('community')
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
