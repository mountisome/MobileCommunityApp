<template>
	<view>
		<uni-section class="mb-10" title="已解决" type="line">
			<uni-collapse  accordion v-model="accordionVal" v-for="(request, index) in solvedRequest">
				<uni-collapse-item :title="'请求'+(index+1)">
					<uni-card title="请求:" :thumbnail="image1">
						<text>{{request.requestContent}}</text>
						<view>
							<uni-dateformat :date="request.requestTime"></uni-dateformat>
						</view>
					</uni-card>
					<uni-card title="回复:" :thumbnail="image2">
						<text>{{request.responseContent}}</text>
						<view>
							<uni-dateformat :date="request.responseTime"></uni-dateformat>
						</view>
					</uni-card>
				</uni-collapse-item>
			</uni-collapse>
		</uni-section>
	</view>
	<view>
		<uni-section class="mb-10" title="未解决">
			<template v-slot:decoration>
			  <view class="decoration"></view>
			</template>
			<uni-collapse v-for="(request, index) in unsolvedRequest">
				<uni-collapse-item :title="'请求'+(index+1)">
					<uni-card title="请求:" :thumbnail="image1">
						<text>{{request.requestContent}}</text>
						<view>
							<uni-dateformat :date="request.requestTime"></uni-dateformat>
						</view>
					</uni-card>
				</uni-collapse-item>
			</uni-collapse>
		</uni-section>
	</view>
	<view>
		<uni-section class="mb-10" title="提出请求" padding>
			<uni-easyinput type="textarea" v-model="value" placeholder="请输入内容"></uni-easyinput>
			<button type="primary" plain="true" size="mini" @click="submitRequest()">提交</button>
		</uni-section>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				accordionVal: '',
				image1: '',
				image2: '/static/images/user.png',
				value: '',
				username: '',
				solvedRequest: [],
				unsolvedRequest: []
			}
		},
		onLoad() {
			this.username = uni.getStorageSync('username')
			this.image1 = uni.getStorageSync('image')
			const db = uniCloud.database()
			db.collection('request').get().then((res)=>{
				let len = res.result.data.length
				if (len > 0) {
					let k = 0
					let t = 0
					for (let i = 0; i < len; i++) {
						if (res.result.data[i].name == this.username) {
							if (res.result.data[i].solved) {
								this.$set(this.solvedRequest, k, {
										requestContent: res.result.data[i].requestContent,
										requestTime: res.result.data[i].requestTime,
										responseContent: res.result.data[i].responseContent,
										responseTime: res.result.data[i].responseTime	
								})
								k++
							} else {
								this.$set(this.unsolvedRequest, t, {
									requestContent: res.result.data[i].requestContent,
									requestTime: res.result.data[i].requestTime
								})
								t++
							}
						}
					}
				}
			}).catch((err)=>{
				console.log(err.code)
				console.log(err.message)
			})
		},
		methods: {
			submitRequest() {
				let timestamp = new Date().getTime()
				const db = uniCloud.database()
				db.collection('request').add({
					name: this.username,
					requestContent: this.value,
					requestTime: timestamp,
					solved: false,
					responseContent: "",
					responseTime: 0
				}).then((res)=>{
					uni.showToast({
						title: '请求提交成功',
						icon: 'success'
					})
				}).catch((err)=>{
					console.log(err.code)
					console.log(err.message)
				})
			}
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
	
	.content {
		padding: 15px;
	}
</style>
