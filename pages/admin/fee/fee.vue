<template>
	<view>
		<uni-section class="mb-10" title="已缴费" type="line">
			<uni-list v-for="(user, index) in paidList">
				<uni-list-item :title="user.name" :rightText="user.paid"></uni-list-item>
			</uni-list>
		</uni-section>
	</view>
	<view>
		<uni-section class="mb-10" title="未缴费">
			<template v-slot:decoration>
			  <view class="decoration"></view>
			</template>
			<uni-list v-for="(user, index) in unpaidList">
				<uni-list-item :title="user.name" :note="'物业费:'+user.fee">
					<template v-slot:footer>
						<view>
							<button size="mini" type="primary" plain="true" 
								@click="charge(user.name)">催缴</button>
							<button size="mini" type="primary" plain="true"
								@click="paid(user.name)" class="btn-paid">已缴</button>	
						</view>
					</template>
				</uni-list-item>
			</uni-list>
		</uni-section>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				paidList: [],
				unpaidList: []
			}
		},
		methods: {
			charge(name) {
				var timestamp = new Date().getTime()
				const db = uniCloud.database()
				db.collection('notice').add({
					info: "请尽快缴纳物业费",
					time: timestamp,
					name: name
				}).then((res)=>{
				    uni.showToast({
				    	title: '催缴成功',
						icon: 'success'
				    })
				}).catch((err)=>{
				    console.log(err.code)
					console.log(err.message)
				})
			},
			paid(name) {
				const db = uniCloud.database()
				db.collection('user').where({
				    name: name
				}).update({
				    fee: 0
				}).then((res)=>{
					db.collection('notice').where({
						info: '请尽快缴纳物业费',
						name: name
					}).remove().then((res)=>{
						uni.showToast({
							title: '用户物业费缴纳成功',
							icon: 'success'
						})
						db.collection('notice').where({
							info: name + '已缴费',
							name: 'admin'
						}).remove()
						db.collection('user').get().then((res)=>{
							let len = res.result.data.length
							if (len > 0) {
								let paidList2 = []
								let unpaidList2 = []
								for (let i = 0; i < len; i++) {
									if (res.result.data[i].fee === 0) {
										paidList2.push({
											name: res.result.data[i].name,
											paid: '已缴'
										})
									} else {
										unpaidList2.push({
											name: res.result.data[i].name,
											fee: res.result.data[i].fee.toString()
										})
									}
								}
								this.paidList = paidList2
								this.unpaidList = unpaidList2
							}
						})
					}).catch((err)=>{
						console.log(err.code)
						console.log(err.message)
					})
				}).catch((err)=>{
					console.log(err.code)
					console.log(err.message)
				})
			}
		},
		onLoad() {
			const db = uniCloud.database()
			db.collection('user').get().then((res)=>{
				let len = res.result.data.length
				if (len > 0) {
					let paidList2 = []
					let unpaidList2 = []
					for (let i = 0; i < len; i++) {
						if (res.result.data[i].fee === 0) {
							paidList2.push({
								name: res.result.data[i].name,
								paid: '已缴'
							})
						} else {
							unpaidList2.push({
								name: res.result.data[i].name,
								fee: res.result.data[i].fee.toString()
							})
						}
					}
					this.paidList = paidList2
					this.unpaidList = unpaidList2
				}
			}).catch((err)=>{
				console.log(err.code)
				console.log(err.message)
			})
		}
	}
</script>

<style lang="scss">
	$uni-success: #18bc37 !default;
	
	.mb-10 {
		margin-bottom: 10px;
	}
	
	.decoration{
		width: 6px;
		height: 6px;
		margin-right: 4px;
		border-radius: 50%;
		background-color: $uni-success;
	}
	
	.btn-paid {
		margin-left: 5px;
	}
</style>
