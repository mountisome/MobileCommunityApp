<template>
	<view>
		<view class="normal-register-container">
			<view class="register-form-content">
				<view class="input-item flex align-center">
					<view class="iconfont icon-user icon"></view>
					<input v-model="name" class="input" type="text" placeholder="请输入账号" maxlength="30" />
				</view>
				<view class="input-item flex align-center">
					<view class="iconfont icon-password icon"></view>
					<input v-model="password" type="password" class="input" placeholder="请输入密码" maxlength="20" />
				</view>
				<view class="input-item flex align-center">
					<view class="cuIcon-phone icon"></view>
					<input v-model="phone" class="input" type="number" placeholder="请输入手机号" maxlength="30" />
				</view>
				<view class="action-btn">
					<button @click="handleRegister" class="register-btn cu-btn block bg-blue lg round">注册</button>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				name: '',
				password: '',
				phone: ''
			}
		},
		methods: {
			// 注册方法
			handleRegister() {
				if (this.name === '') {
					uni.showToast({
						title: '账号不能为空',
						icon: 'error'
					})
				} else if (this.password === '') {
					uni.showToast({
						title: '密码不能为空',
						icon: 'error'
					})
				} else if (this.phone === '') {
					uni.showToast({
						title: '手机号不能为空',
						icon: 'error'
					})
				} else {
					const db = uniCloud.database();
					db.collection('user').where({name: this.name}).get().then((res)=>{
						if (res.result.data[0] !== undefined || this.name === "ALL") {
							uni.showToast({
								title: '用户已存在',
								icon: 'error'
							})
						} else {
							db.collection('user').add({
								name: this.name,
								password: this.password,
								username: '孙先生',
								phone: this.phone,
								image: "",
								building: "1栋",
								household: "1801",
								fee: 0
							}).then((res) => {
								uni.showToast({
									title: '注册成功',
									icon: 'success'
								})
								uni.navigateBack()
							}).catch((err) => {
								console.log(err.message)
								uni.showToast({
									title: '注册失败',
									icon: 'error'
								})
							})
						}
					}).catch(err => {
						console.log(err.message)
					})
				}
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #ffffff;
	}
	
	.normal-register-container {
		width: 100%;
		
		.register-form-content {
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
				margin-top: 40px;
				height: 45px;
			}
			
			.register-btn {
				margin-top: 20px;
				height: 45px;
			}
			
			.easyinput {
			  width: 100%;
			}
		}
	}
</style>
