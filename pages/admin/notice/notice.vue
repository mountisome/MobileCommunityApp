<template>
	<view class="notice">
		<uni-list :border="true" v-for="(notice, index) in noticeList">
			<uni-list-item :title="notice.info">
			</uni-list-item>
			<uni-dateformat :date="notice.time" style="margin-left: 15px; margin-bottom: 10px;"></uni-dateformat>
		</uni-list>
	</view>
	<view>
		<uni-section title="发布通知" padding type="square" titleFontSize="17px">
			<uni-easyinput type="textarea" v-model="value" placeholder="请输入通知内容"></uni-easyinput>
		</uni-section>
		<uni-fab ref="fab" :pattern="pattern" :horizontal="horizontal"
			:vertical="vertical" @fabClick="fabClick()"></uni-fab>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				noticeList: [],
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
				value: ''
			}
		},
		methods: {
			fabClick() {
				if (this.value !== '') {
					var timestamp = new Date().getTime()
					const db = uniCloud.database()
					db.collection('notice').add({
					    info: this.value,
					    time: timestamp,
						name: 'ALL'
					}).then((res)=>{
					    uni.showToast({
					    	title: '通知发布成功',
							icon: 'success'
					    })
					}).catch((err)=>{
					    console.log(err.code)
						console.log(err.message)
					})
					db.collection('notice').where({name: 'ALL'}).get().then((res)=>{
						let len = res.result.data.length
						let noticeList2 = []
						for (let i = 0; i < len; i++) {
							noticeList2.push({
								info: res.result.data[i].info,
								time: res.result.data[i].time
							})
						}
						this.noticeList = noticeList2
					})
				} else {
					uni.showToast({
						title: '通知不能为空',
						icon: 'error'
					})
				}
			}
		},
		onLoad() {
			const db = uniCloud.database()
			db.collection('notice').where({name: 'ALL'}).get().then((res)=>{
				let len = res.result.data.length
				let noticeList2 = []
				for (let i = 0; i < len; i++) {
					noticeList2.push({
						info: res.result.data[i].info,
						time: res.result.data[i].time
					})
				}
				this.noticeList = noticeList2
			})
		}
	}
</script>

<style lang="scss">
	.notice {
		margin-bottom: 10px;
	}
</style>
