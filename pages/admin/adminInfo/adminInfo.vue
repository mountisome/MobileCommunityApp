<template>
	<view>
		<uni-list>
			<uni-list-item title="物业名" :rightText="adminname"></uni-list-item>
			<uni-list-item title="手机号" :rightText="phone"></uni-list-item>
			<uni-list-item title="小区名" :rightText="community"></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				adminname: '社区物业',
				phone: '',
				community: ''
			};
		},
		onLoad() {
			if (uni.getStorageSync('adminname') !== '') {
				this.phone = uni.getStorageSync('phone')
				this.community = uni.getStorageSync('community')
			}
		},
		methods: {
			
		}
	}
</script>

<style lang="scss">

</style>
