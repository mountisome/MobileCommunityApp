<template>
	<view>
		<uni-list>
			<uni-list-item title="市场名称" :rightText="marketname"></uni-list-item>
			<uni-list-item title="手机号" :rightText="phone"></uni-list-item>
			<uni-list-item title="市场地址" :rightText="address"></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				marketname: '',
				phone: '',
				address: ''
			}
		},
		methods: {
			
		},
		onLoad() {
			if (uni.getStorageSync('marketname') !== '') {
				this.marketname = uni.getStorageSync('marketname')
				this.phone = uni.getStorageSync('phone')
				this.address = uni.getStorageSync('address')
			}
		}
	}
</script>

<style>

</style>
