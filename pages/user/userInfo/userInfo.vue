<template>
	<view>
		<uni-list>
			<uni-list-item title="用户名" :rightText="username"></uni-list-item>
			<uni-list-item title="手机号" :rightText="phone"></uni-list-item>
			<uni-list-item title="小区名" :rightText="community"></uni-list-item>
			<uni-list-item title="楼号" :rightText="building"></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: '',
				phone: '',
				community: '',
				building: ''
			}
		},
		onLoad() {
			if (uni.getStorageSync('username') !== '') {
				this.username = uni.getStorageSync('username')
				this.phone = uni.getStorageSync('phone')
				this.community = uni.getStorageSync('community')
				this.building = uni.getStorageSync('building')
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
