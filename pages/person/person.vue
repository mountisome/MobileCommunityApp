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
				<view class="cu-item arrow" @tap="userInfo">
					<view class="content">
						<text class="lg text-gray cuIcon-friendaddfill"></text>
						<text class="text-grey">个人信息</text>
					</view>
				</view>
				<view class="cu-item arrow" @tap="changePwd">
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
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: 'user',
				image: '/static/images/user.png',
				identity: ''
			}
		},
		onLoad() {
			this.identity = uni.getStorageSync('identity')
			if (uni.getStorageSync('username') !== '') {
				this.username = uni.getStorageSync('username')
				this.image = uni.getStorageSync('image')
			}
		},
		methods: {
			changeImage() {
				uni.chooseImage({
					count: 1,
					sizeType: ['compressed'],
					success(res) {
						console.log(res)
						if (res.tempFilePaths.length > 0) {
							let filePath = res.tempFilePaths[0]
							uniCloud.uploadFile({
								filePath: filePath,
								cloudPath: uni.getStorageSync('username') + '.jpg',
								onUploadProgress:function(progressEvent){
									console.log(progressEvent)
									var percentCompleted = Math.round(
										(progressEvent.loaded * 100) / progressEvent.total
									)
								},
								success(res) {
									let imageUrl = res.fileID
									const db = uniCloud.database()
									let collection = db.collection("user")
									let ans = collection.where({name: uni.getStorageSync('username')}).update({
										image: imageUrl
									})
									uni.showToast({
										title: '图片上传成功',
										icon: 'success'
									})
									const page = getCurrentPages()[0];
									const vm = page.$vm;
									vm.$data.image = imageUrl
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
				uni.navigateTo({
					url: '/pages/user/login/login'
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
