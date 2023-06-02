<template>
	<view v-if="identity === 'user'">
		<view class="user-container bg-white flex flex-direction align-center">
			<view class="cu-avatar xl round margin-left lage-avatar" :style="'background-image:url('+image+');'" @click="changeImage"></view>
			<view class="margin-top">
				<text>{{username}}</text>
			</view>
		</view>
		<view class="margin-top">
			<view class="cu-list menu">
				<view class="cu-item arrow" @tap="userInfo()">
					<view class="content">
						<text class="lg text-gray cuIcon-friendaddfill"></text>
						<text class="text-grey">个人信息</text>
					</view>
				</view>
				<view class="cu-item arrow" @tap="changePwd()">
					<view class="content">
						<text class="lg text-gray cuIcon-lock"></text>
						<text class="text-grey">修改密码</text>
					</view>
				</view>
			</view>
		</view>	
		<view class="margin-top">
			<view class="cu-list menu">
				<view class="cu-item " @tap="logout">
					<view class="content text-center">
						<text class="text-red">退 出</text>
					</view>
				</view>
			</view>
		</view>
	</view>
	
	<view v-else-if="identity === 'admin'">
		<view class="user-container bg-white flex flex-direction align-center">
			<view class="cu-avatar xl round margin-left lage-avatar" :style="'background-image:url('+image2+');'" @click="changeImage"></view>
			<view class="margin-top">
				<text>{{adminname}}</text>
			</view>
		</view>
		<view class="margin-top">
			<view class="cu-list menu">
				<view class="cu-item arrow" @tap="adminInfo()">
					<view class="content">
						<text class="lg text-gray cuIcon-friendaddfill"></text>
						<text class="text-grey">个人信息</text>
					</view>
				</view>
				<view class="cu-item arrow" @tap="changeAdminPwd()">
					<view class="content">
						<text class="lg text-gray cuIcon-lock"></text>
						<text class="text-grey">修改密码</text>
					</view>
				</view>
			</view>
		</view>	
		<view class="margin-top">
			<view class="cu-list menu">
				<view class="cu-item " @tap="logout">
					<view class="content text-center">
						<text class="text-red">退 出</text>
					</view>
				</view>
			</view>
		</view>
	</view>
	
	<view v-else-if="identity === 'market'">
		<view class="user-container bg-white flex flex-direction align-center">
			<view class="cu-avatar xl round margin-left lage-avatar" :style="'background-image:url('+image3+');'" @click="changeImage"></view>
			<view class="margin-top">
				<text>{{marketname}}</text>
			</view>
		</view>
		<view class="margin-top">
			<view class="cu-list menu">
				<view class="cu-item arrow" @tap="marketInfo()">
					<view class="content">
						<text class="lg text-gray cuIcon-friendaddfill"></text>
						<text class="text-grey">市场信息</text>
					</view>
				</view>
				<view class="cu-item arrow" @tap="changeMarketPwd()">
					<view class="content">
						<text class="lg text-gray cuIcon-lock"></text>
						<text class="text-grey">修改密码</text>
					</view>
				</view>
			</view>
		</view>	
		<view class="margin-top">
			<view class="cu-list menu">
				<view class="cu-item " @tap="logout">
					<view class="content text-center">
						<text class="text-red">退 出</text>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				name: 'user',
				username: '',
				building: '',
				household: '',
				image: '/static/images/user.png',
				identity: '',
				adminname: '社区物业',
				marketname: '',
				image2: '/static/images/user.png',
				image3: '/static/images/user.png'
			}
		},
		onLoad() {
			this.identity = uni.getStorageSync('identity')
			if (uni.getStorageSync('name') !== '') {
				this.username = uni.getStorageSync('username')
				this.image = uni.getStorageSync('image')
				this.building = uni.getStorageSync('building')
				this.household = uni.getStorageSync('household')
				this.username = this.building + '-' + this.household + '-' + this.username
			}
			if (uni.getStorageSync('marketname') !== '') {
				this.marketname = uni.getStorageSync('marketname')
				this.image3 = uni.getStorageSync('image3')
			}
		},
		methods: {
			changeImage() {
				uni.chooseImage({
					count: 1,
					sizeType: ['compressed'],
					success(res) {
						if (res.tempFilePaths.length > 0) {
							let filePath = res.tempFilePaths[0]
							uniCloud.uploadFile({
								filePath: filePath,
								cloudPath: uni.getStorageSync('name') + '.jpg',
								onUploadProgress:function(progressEvent){
									console.log(progressEvent)
									var percentCompleted = Math.round(
										(progressEvent.loaded * 100) / progressEvent.total
									)
								},
								success(res) {
									let imageUrl = res.fileID
									const db = uniCloud.database()
									db.collection("user").where({name: uni.getStorageSync('name')})
									.update({
										image: imageUrl
									}).then((res)=> {
										uni.showToast({
											title: '图片上传成功',
											icon: 'success'
										})
										const page = getCurrentPages()[0]
										const vm = page.$vm
										vm.$data.image = imageUrl
									})
								},
								fail() {
									uni.showToast({
										title: '图片上传失败',
										icon: 'error'
									})
								}
							})
						}
					},
					fail() {
						uni.showToast({
							title: '图片选择失败',
							icon: 'error'
						})
					}
				})
			},
			
			// 用户信息
			userInfo() {
				uni.navigateTo({
					url: '/pages/user/userInfo/userInfo'
				})
			},
			
			// 修改密码
			changePwd() {
				uni.navigateTo({
					url: '/pages/user/changePwd/changePwd'
				})
			},
			
			// 退出登录
			logout() {
				uni.reLaunch({
					url: '/pages/user/login/login'
				})
			},
			
			// 物业信息
			adminInfo() {
				uni.navigateTo({
					url: '/pages/admin/adminInfo/adminInfo'
				})
			},
			
			// 修改物业密码
			changeAdminPwd() {
				uni.navigateTo({
					url: '/pages/admin/changePwd/changePwd'
				})
			},
			
			// 市场信息
			marketInfo() {
				uni.navigateTo({
					url: '/pages/market/marketInfo/marketInfo'
				})
			},
			
			// 修改市场密码
			changeMarketPwd() {
				uni.navigateTo({
					url: '/pages/market/changePwd/changePwd'
				})
			}
			
			
		}
	}
</script>

<style>
	.user-container {
		padding: 60upx 0 40upx 0;
	}
	
	.lage-avatar {
		width: 200upx;
		height: 200upx;
	}
</style>
