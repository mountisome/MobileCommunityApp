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
				<view class="input-item flex align-center">
					<view class="iconfont icon-user icon"></view>
					<input v-model="username" class="input" type="text" placeholder="请输入姓名" maxlength="30" />
				</view>
				<view>
					<text>楼号</text>
					<uni-data-select v-model="building" :localdata="range" :clear="false"></uni-data-select>
				</view>
				<view class="household">
					<text>户号</text>
					<uni-data-select v-model="household" :localdata="range2" :clear="false"></uni-data-select>
				</view>
				<view class="action-btn">
					<button @click="addUser" class="register-btn cu-btn block bg-blue lg round">添加</button>
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
				phone: '',
				username: '',
				building: -1,
				household: -1,
				range: [
					{value: 0, text: '1栋'},
					{value: 1, text: '2栋'},
					{value: 2, text: '3栋'},
					{value: 3, text: '4栋'},
					{value: 4, text: '5栋'}
				],
				range2: [
					{value: 0, text: '201'},
					{value: 1, text: '202'},
					{value: 2, text: '301'},
					{value: 3, text: '302'},
					{value: 4, text: '401'},
					{value: 5, text: '402'},
					{value: 6, text: '501'},
					{value: 7, text: '502'}
				]
			}
		},
		methods: {
			isTelCode(str) {
			    var reg= /^((0\d{2,3}-\d{7,8})|(1[3584]\d{9}))$/;
			    return reg.test(str);
			},
			addUser() {
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
				}
				else if (!this.isTelCode(this.phone)) {
					uni.showToast({
						title: '手机号输入有误',
						icon: 'error'
					})
				}
				else if (this.username === '') {
					uni.showToast({
						title: '姓名不能为空',
						icon: 'error'
					})
				} 
				else if (this.building === -1) {
					uni.showToast({
						title: '楼号不能为空',
						icon: 'error'
					})
				} 
				else if (this.household === -1) {
					uni.showToast({
						title: '户号不能为空',
						icon: 'error'
					})
				}
				else {
					const db = uniCloud.database();
					db.collection('user').where({name: this.username}).get().then((res)=>{
						if (res.result.data[0] !== undefined || this.username === "ALL") {
							uni.showToast({
								title: '用户已存在',
								icon: 'error'
							})
						} else {
							db.collection('user').add({
								name: this.name,
								password: this.password,
								username: this.username,
								phone: this.phone,
								image: "",
								building: this.range[this.building].text,
								household: this.range2[this.household].text,
								fee: 0
							}).then((res) => {
								uni.showToast({
									title: '成功添加新用户',
									icon: 'success'
								})
								uni.navigateBack()
							}).catch((err) => {
								console.log(err.message)
								uni.showToast({
									title: '添加失败',
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
		
	.register-form-content {
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
		
		.household {
			margin-top: 5px;
		}
	}
</style>
