<template>
	<view>
		<uni-list v-for="(user, index) in userList" :key="user.name" :border="true">
			<uni-list-chat :title="user.username" :avatar="user.image"
				:note="user.phone">
				<view class="chat-custom-right">
					<view>
						<uni-icons type="trash-filled" size="25" color="red" @click="deleteUser(user.name)"></uni-icons>
						<text class="text-red">删除</text>
					</view>
					<view class="phone">
						<uni-icons type="phone" size="25" color="blue" @click="callPhone(user.phone)"></uni-icons>
						<text class="chat-custom-text">呼叫</text>
					</view>
				</view>
			</uni-list-chat>
		</uni-list>
		<uni-fab ref="fab" :pattern="pattern" :horizontal="horizontal"
			:vertical="vertical" @fabClick="fabClick"></uni-fab>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userList: [],
				pattern: {
					color: '#7A7E83',
					backgroundColor: '#fff',
					selectedColor: '#007AFF',
					buttonColor: '#007AFF',
					iconColor: '#fff'
				},
				horizontal: 'right',
				vertical: 'bottom',
				direction: 'horizontal',
			}
		},
		methods: {
			fabClick() {
				uni.navigateTo({
					url: '/pages/admin/addUser/addUser'
				})
			},
			updateList() {
				const db = uniCloud.database()
				db.collection('user').get().then((res)=>{
					let userList2 = []
					let len = res.result.data.length
					for (let i = 0; i < len; i++) {
						userList2.push({
							name: res.result.data[i].name,
							username: res.result.data[i].building+'-'+res.result.data[i].household+'-'+res.result.data[i].username,
							phone: res.result.data[i].phone,
							image: res.result.data[i].image
						})
					}
					this.userList = userList2
				})
			},
			deleteUser(name) {
				uni.showModal({
					title: '提示',
					content: '确认删除？',
					success:function(res){
						if (res.confirm) {
							const db = uniCloud.database()
							db.collection('user').where({
								name: name
							}).remove().then((res)=>{
								uni.showToast({
									title: '用户删除成功',
									icon: 'success'
								})
								let len = getCurrentPages().length
								const page = getCurrentPages()[len - 1]
								const vm = page.$vm
								vm.updateList()
							}).catch((err)=>{
								console.log(err.code)
								console.log(err.message)
							})
						}
					}
				})
			},
			callPhone(phone) {
				uni.makePhoneCall({
					phoneNumber: phone
				})
			}
		},
		onLoad() {
			const db = uniCloud.database()
			db.collection('user').get().then((res)=>{
				let len = res.result.data.length
				let userList2 = []
				for (let i = 0; i < len; i++)
					userList2.push({
						name: res.result.data[i].name,
						username: res.result.data[i].building+'-'+res.result.data[i].household+'-'+res.result.data[i].username,
						phone: res.result.data[i].phone,
						image: res.result.data[i].image
					})
				this.userList = userList2
			})
		},
		onShow() {
			const db = uniCloud.database()
			db.collection('user').get().then((res)=>{
				let len = res.result.data.length
				let userList2 = []
				for (let i = 0; i < len; i++)
					userList2.push({
						name: res.result.data[i].name,
						username: res.result.data[i].building+'-'+res.result.data[i].household+'-'+res.result.data[i].username,
						phone: res.result.data[i].phone,
						image: res.result.data[i].image
					})
				this.userList = userList2
			})
		}
	}
</script>

<style lang="scss">
	.chat-custom-right {
		flex: 1;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		justify-content: space-between;
		align-items: flex-end;
		padding: 5px;
	}
	
	.phone {
		margin-left: 10px;
	}
</style>
