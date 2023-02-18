<template>
	<view class="input-item flex align-center">
		<view class="iconfont icon-password icon"></view>
		<input v-model="oldPassword" type="password" class="input" placeholder="请输入原密码" maxlength="20" />
	</view>
	<view class="input-item flex align-center">
		<view class="iconfont icon-password icon"></view>
		<input v-model="newPassword" type="password" class="input" placeholder="请输入新密码" maxlength="20" />
	</view>
	<view class="input-item flex align-center">
		<view class="iconfont icon-password icon"></view>
		<input v-model="newPassword2" type="password" class="input" placeholder="请再次输入新密码" maxlength="20" />
	</view>
	<view class="action-btn">
		<button @click="changePwd()" class="change-btn cu-btn block bg-red lg round">确认修改</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				adminname: '',
				password2: '',
				oldPassword: '',
				newPassword: '',
				newPassword2: ''
			}
		},
		methods: {
			changePwd() {
				if (this.oldPassword === "" || this.newPassword === "" || this.newPassword2 === "") {
					uni.showToast({
						title: '密码不能为空',
						icon: 'error'
					})
				} else if (this.password !== this.oldPassword) {
					uni.showToast({
						title: '原密码错误',
						icon: 'error'
					})
				} else if (this.newPassword !== this.newPassword2) {
					uni.showToast({
						title: '两次新密码不一致',
						icon: 'error'
					})
				} else {
					const db = uniCloud.database()
					db.collection('admin').where({
					    name: this.adminname
					}).update({
					    password: this.newPassword
					}).catch((err)=>{
					    console.log(err.code)
						console.log(err.message)
					})
					uni.showToast({
						title: '密码修改成功',
						icon: 'success'
					})
					uni.navigateTo({
						url: '/pages/admin/login/login'
					})
				}
			}
		},
		onLoad() {
			this.adminname = uni.getStorageSync('adminname')
			this.password2 = uni.getStorageSync('password2')
		}
	}
</script>

<style lang="scss">
	.input-item {
		margin: 20px auto;
		background-color: #f5f6f7;
		width: 80%;
		height: 45px;
		border-radius: 20px;
	
		.input {
		  width: 100%;
		  font-size: 14px;
		  line-height: 20px;
		  text-align: left;
		  padding-left: 15px;
		}
		
		.icon {
		  font-size: 38rpx;
		  margin-left: 10px;
		  color: #999;
		}
	}
	
	.change-btn {
		margin: 20px auto;
		margin-top: 10px;
		width: 80%;
		height: 45px;
	}
</style>
