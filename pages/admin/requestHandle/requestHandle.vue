<template>
	<view>
		<uni-section class="mb-10" title="未解决">
			<template v-slot:decoration>
			  <view class="decoration"></view>
			</template>
			<uni-list v-for="(request, index) in requestList">
				<uni-card :title="request.name">
					<text>{{request.requestContent}}</text>
					<view>
						<uni-dateformat :date="request.requestTime"></uni-dateformat>
					</view>
				</uni-card>
				<view class="reply">
					<uni-easyinput type="textarea" v-model="value" placeholder="请输入内容"></uni-easyinput>
					<button type="primary" size="mini" @click="reply(request.name, request.requestTime)" class="btn-reply">回复</button>
				</view>
			</uni-list>
		</uni-section>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				requestList: [],
				value: '',
			}
		},
		methods: {
			reply(name, requestTime) {
				var timestamp = new Date().getTime()
				const db = uniCloud.database()
				db.collection('request').where({
					name: name,
					requestTime: requestTime
				}).update({
				    solved: true,
					responseContent: this.value,
					responseTime: timestamp
				})
				uni.showToast({
					title: '回复请求成功',
					icon: 'success'
				})
				db.collection('request').get().then((res)=>{
					let requestList2 = []
					let k = 0;
					let len = res.result.data.length
					for (let i = 0; i < len; i++) {
						if (!res.result.data[i].solved) {
							this.$set(this.requestList2, k, {
								name: res.result.data[i].name,
								requestContent: res.result.data[i].requestContent,
								requestTime: res.result.data[i].requestTime
							})
							k++
						}
					}
					this.requestList = requestList2
				})
			}
		},
		onLoad() {
			const db = uniCloud.database()
			db.collection('request').get().then((res)=>{
				let len = res.result.data.length
				if (len > 0) {
					let k = 0
					for (let i = 0; i < len; i++) {
						if (!res.result.data[i].solved) {
							this.$set(this.requestList, k, {
								name: res.result.data[i].name,
								requestContent: res.result.data[i].requestContent,
								requestTime: res.result.data[i].requestTime
							})
							k++
						}
					}
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
	
	.decoration{
		width: 6px;
		height: 6px;
		margin-right: 4px;
		border-radius: 50%;
		background-color: $uni-success;
	}
	
	.reply {
		padding-left: 15px;
		padding-right: 15px;
		padding-bottom: 15px;
	}
	
	.btn-reply {
		margin-top: 5px;
	}
</style>
