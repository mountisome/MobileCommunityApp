<template>
	<view class="normal-login-container">
		<view class="login-form-content">
			<view class="input-item flex align-center">
				<view class="iconfont icon-user icon"></view>
				<input v-model="loginForm.username" class="input" type="text" placeholder="请输入账号" maxlength="30" />
			</view>
			<view class="input-item flex align-center">
				<view class="iconfont icon-password icon"></view>
				<input v-model="loginForm.password" type="password" class="input" placeholder="请输入密码" maxlength="20" />
			</view>
			<view class="action-btn">
				<button @click="handleLogin()" class="login-btn cu-btn block bg-blue lg round">登录</button>
			</view>
			<view class="action-btn">
				<button @click="goRegister()" class="register-btn cu-btn block bg-blue lg round">注册</button>
			</view>
			<view>
				<button @click="adminLogin()" class="check-btn cu-btn block bg-green lg round">物业登录</button>
				<button @click="marketLogin()" class="check-btn cu-btn block bg-green lg round">市场登录</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				loginForm: {
				  username: '',
				  password: ''
				},
				pwd: '',
				image: ''
			}
		},
		methods: {
			// 登录方法
			handleLogin() {
				if (this.loginForm.username === '') {
					uni.showToast({
						title: '账号不能为空',
						icon: 'error'
					})
				} else if (this.loginForm.password === '') {
					uni.showToast({
						title: '密码不能为空',
						icon: 'error'
					})
				} else {
					const db = uniCloud.database()
					db.collection('user').where({name: this.loginForm.username}).get().then((res)=>{
						if (res.result.data[0] === undefined) {
							uni.showToast({
								title: '用户不存在',
								icon: 'error'
							})
						} else {
							this.pwd = res.result.data[0].password
							if (this.pwd !== this.loginForm.password) {
								uni.showToast({
									title: '密码错误',
									icon: 'error'
								})
							} else {
								uni.setStorage({
									key: 'username',
									data: this.loginForm.username
								})
								uni.setStorage({
									key: 'password',
									data: res.result.data[0].password
								})
								uni.setStorage({
									key: 'image',
									data: res.result.data[0].image
								})
								uni.setStorage({
									key: 'phone',
									data: res.result.data[0].phone
								})
								uni.setStorage({
									key: 'community',
									data: res.result.data[0].community
								})
								uni.setStorage({
									key: 'building',
									data: res.result.data[0].building
								})
								uni.setStorage({
									key: 'fee',
									data: res.result.data[0].fee
								})
								uni.setStorage({
									key: 'identity',
									data: 'user'
								})
								uni.showToast({
									title: '登录成功',
									icon: 'success'
								})
								uni.switchTab({
									url: '/pages/index/index'
								})
							}
						}
					}).catch((err)=>{
						console.log(err.code)
						console.log(err.message)
					})
				}
			},
			
			// 注册
			goRegister() {
				uni.navigateTo({
					url: '/pages/user/register/register'
				})
			},
			
			// 物业登录
			adminLogin() {
				uni.redirectTo({
					url: '/pages/admin/login/login'
				})
			},
			
			// 市场登录
			marketLogin() {
				uni.redirectTo({
					url: '/pages/market/login/login'
				})
			},
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #ffffff;
	}
	
	.normal-login-container {
		width: 100%;
		
		.login-form-content {
			text-align: center;
			margin: 20px auto;
			margin-top: 15%;
			width: 80%;
			
			.input-item {
				margin: 20px auto;
				background-color: #f5f6f7;
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
			
			.login-btn {
				margin-top: 10px;
				height: 45px;
			}
			
			.register-btn {
				margin-top: 10px;
				height: 45px;
			}
			
			.check-btn {
				margin-top: 15px;
				height: 45px;
			}
			
			.easyinput {
			  width: 100%;
			}
		}
	}
</style>
