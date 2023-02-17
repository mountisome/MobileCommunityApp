<template>
	<view class="fee">
		<uni-list border-full>
			<uni-list-item title="用户名:"  :rightText="username"/>
			<uni-list-item title="物业费:" :rightText="fee"/>
		</uni-list>
	</view>
	<view class="pay">
		<button class="pay-btn cu-btn block bg-blue lg round" @click="pay()">缴纳</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				range: ['', ''],
				date1: '',
				date2: '',
				username: 'user',
				fee: ''
			}
		},
		onLoad() {
			if (uni.getStorageSync('username') !== '') {
				this.username = uni.getStorageSync('username')
				this.fee = uni.getStorageSync('fee').toString()
			}
		},
		methods: {
			pay() {
				var timestamp = new Date().getTime()
				const db = uniCloud.database()
				db.collection('notice').add({
				    info: this.username + '已缴费',
				    time: timestamp,
					name: 'admin'
				}).then((res)=>{
				    uni.showToast({
				    	title: '缴费成功',
						icon: 'success'
				    })
				}).catch((err)=>{
				    console.log(err.code)
					console.log(err.message)
				})
			}
		}
	}
</script>

<style lang="scss">
	.fee {
		margin-top: 15px;
	}
	
	.pay {
		margin: 20px auto;
		width: 80%;
	}
	
	.pay-btn {
		margin-top: 10px;
		height: 45px;
	}
</style>
