<template>
	<view>
		<uni-section title="商品通知" padding type="circle" titleFontSize="17px">
			<uni-list v-for="(commodityNotice, index) in commodityNoticeList">
				<uni-list-item :title="commodityNotice.info">
					<template v-slot:footer>
						<view>
							<uni-icons type="trash-filled" size="25" color="red" @click="deleteCommodityNotice(commodityNotice.info)"></uni-icons>
							<text class="text-red">删除</text>
						</view>
					</template>
				</uni-list-item>
				<uni-dateformat :date="commodityNotice.time" style="margin-left: 15px; margin-bottom: 10px;"></uni-dateformat>
			</uni-list>
		</uni-section>
	</view>
	<view class="commodityNoticeSubmit">
		<uni-section title="发布商品通知" padding type="square" titleFontSize="17px">
			<uni-easyinput type="textarea" v-model="value" placeholder="请输入通知内容"></uni-easyinput>
			<button class="btn-submit" type="primary" plain="true" size="mini" @click="submitNotice()">提交</button>
		</uni-section>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				value: '',
				commodityNoticeList: []
			}
		},
		methods: {
			deleteCommodityNotice(info) {
				const db = uniCloud.database()
				db.collection('notice').where({
				    info: info
				}).remove().then((res)=>{
				    uni.showToast({
				        title: '商品通知删除成功',
				        icon: 'success'
				    })
					db.collection('notice').get().then((res)=>{
						let commodityNoticeList2 = []
						let len = res.result.data.length
						for (let i = 0; i < len; i++) {
							if (res.result.data[i].name === 'market') {
								commodityNoticeList2.push({
									info: res.result.data[i].info,
									time: res.result.data[i].time
								})
							}
						}
						this.commodityNoticeList = commodityNoticeList2
					})
				}).catch((err)=>{
				    console.log(err.code)
					console.log(err.message)
				})
			},
			submitNotice() {
				if (this.value === '') {
					uni.showToast({
						title: '通知不能为空',
						icon: 'error'
					})
				} else {
					let timestamp = new Date().getTime()
					const db = uniCloud.database()
					db.collection('notice').add({
					    info: this.value,
					    time: timestamp,
						name: 'market'
					}).then((res)=>{
					    uni.showToast({
					    	title: '商品通知发送成功',
							icon: 'success'
					    })
						this.value = ""
						db.collection('notice').get().then((res)=>{
							let commodityNoticeList2 = []
							let len = res.result.data.length
							for (let i = 0; i < len; i++) {
								if (res.result.data[i].name === 'market') {
									commodityNoticeList2.push({
										info: res.result.data[i].info,
										time: res.result.data[i].time
									})
								}
							}
							this.commodityNoticeList = commodityNoticeList2
						})
					}).catch((err)=>{
					    console.log(err.code)
						console.log(err.message)
					})
				}
			}
		},
		onLoad() {
			const db = uniCloud.database()
			db.collection('notice').get().then((res)=>{
				let commodityNoticeList2 = []
				let len = res.result.data.length
				for (let i = 0; i < len; i++) {
					if (res.result.data[i].name === 'market') {
						commodityNoticeList2.push({
							info: res.result.data[i].info,
							time: res.result.data[i].time
						})
					}
				}
				this.commodityNoticeList = commodityNoticeList2
			})
		}
	}
</script>

<style lang="scss">
	.btn-submit {
		margin-top: 5px;
	}
	
	.commodityNoticeSubmit {
		margin-top: 10px;
	}
</style>
