<template>
	<view>
		<uni-section class="mb-10" title="未解决">
			<template v-slot:decoration>
			  <view class="decoration"></view>
			</template>
			<view class="tag">
				<uni-tag text="紧急" type="error"/>
			</view>
			<uni-list v-for="(request, index) in requestList">
				<uni-card :title="request.username">
					<view>
					<text class="text-black text-bold">{{request.requestContent}}</text>
					</view>
					<view>
						<text class="text-red">请求类型：{{request.requestType}}</text>
					</view>
					<view>
						<uni-dateformat :date="request.requestTime" class="text-blue"></uni-dateformat>
					</view>
				</uni-card>
				<view class="reply">
					<uni-easyinput type="textarea" v-model="request.value" placeholder="请输入内容"></uni-easyinput>
					<button type="primary" size="mini" @click="reply(request.name, request.requestTime, request.value)" class="btn-reply">回复</button>
				</view>
			</uni-list>
			<view class="tag2">
				<uni-tag text="尽快" type="warning"/>
			</view>
			<uni-list v-for="(request, index) in requestList2">
				<uni-card :title="request.username">
					<view>
					<text class="text-black text-bold">{{request.requestContent}}</text>
					</view>
					<view>
						<text class="text-red">请求类型：{{request.requestType}}</text>
					</view>
					<view>
						<uni-dateformat :date="request.requestTime" class="text-blue"></uni-dateformat>
					</view>
				</uni-card>
				<view class="reply">
					<uni-easyinput type="textarea" v-model="request.value" placeholder="请输入内容"></uni-easyinput>
					<button type="primary" size="mini" @click="reply(request.name, request.requestTime, request.value)" class="btn-reply">回复</button>
				</view>
			</uni-list>
			<view class="tag3">
				<uni-tag text="正常" type="primary"/>
			</view>
			<uni-list v-for="(request, index) in requestList3">
				<uni-card :title="request.username">
					<view>
					<text class="text-black text-bold">{{request.requestContent}}</text>
					</view>
					<view>
						<text class="text-red">请求类型：{{request.requestType}}</text>
					</view>
					<view>
						<uni-dateformat :date="request.requestTime" class="text-blue"></uni-dateformat>
					</view>
				</uni-card>
				<view class="reply">
					<uni-easyinput type="textarea" v-model="request.value" placeholder="请输入内容"></uni-easyinput>
					<button type="primary" size="mini" @click="reply(request.name, request.requestTime, request.value)" class="btn-reply">回复</button>
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
				requestList2: [],
				requestList3: []
			}
		},
		methods: {
			reply(name, requestTime, value) {
				var timestamp = new Date().getTime()
				const db = uniCloud.database()
				db.collection('request').where({
					name: name,
					requestTime: requestTime
				}).update({
				    solved: true,
					responseContent: value,
					responseTime: timestamp
				}).then((res)=>{
					db.collection('request').get().then((res)=>{
						let requestList_ = []
						let requestList_2 = []
						let requestList_3 = []
						let len = res.result.data.length
						for (let i = 0; i < len; i++) {
							if (!res.result.data[i].solved) {
								if (res.result.data[i].timeRequire === "紧急")
									requestList_.push({
										name: res.result.data[i].name,
										username: res.result.data[i].username,
										requestContent: res.result.data[i].requestContent,
										requestTime: res.result.data[i].requestTime,
										value: '',
										requestType: res.result.data[i].requestType
									})
								else if (res.result.data[i].timeRequire === "尽快")
									requestList_2.push({
										name: res.result.data[i].name,
										username: res.result.data[i].username,
										requestContent: res.result.data[i].requestContent,
										requestTime: res.result.data[i].requestTime,
										value: '',
										requestType: res.result.data[i].requestType
									})
								else
									requestList_3.push({
										name: res.result.data[i].name,
										username: res.result.data[i].username,
										requestContent: res.result.data[i].requestContent,
										requestTime: res.result.data[i].requestTime,
										value: '',
										requestType: res.result.data[i].requestType
									})
							}
						}
						this.requestList = requestList_
						this.requestList2 = requestList_2
						this.requestList3 = requestList_3
					})
				})
				uni.showToast({
					title: '回复请求成功',
					icon: 'success'
				})
			}
		},
		onLoad() {
			const db = uniCloud.database()
			db.collection('request').get().then((res)=>{
				let requestList_ = []
				let requestList_2 = []
				let requestList_3 = []
				let len = res.result.data.length
				for (let i = 0; i < len; i++) {
					if (!res.result.data[i].solved) {
						if (res.result.data[i].timeRequire === "紧急")
							requestList_.push({
								name: res.result.data[i].name,
								username: res.result.data[i].username,
								requestContent: res.result.data[i].requestContent,
								requestTime: res.result.data[i].requestTime,
								value: '',
								requestType: res.result.data[i].requestType
							})
						else if (res.result.data[i].timeRequire === "尽快")
							requestList_2.push({
								name: res.result.data[i].name,
								username: res.result.data[i].username,
								requestContent: res.result.data[i].requestContent,
								requestTime: res.result.data[i].requestTime,
								value: '',
								requestType: res.result.data[i].requestType
							})
						else
							requestList_3.push({
								name: res.result.data[i].name,
								username: res.result.data[i].username,
								requestContent: res.result.data[i].requestContent,
								requestTime: res.result.data[i].requestTime,
								value: '',
								requestType: res.result.data[i].requestType
							})
					}
				}
				this.requestList = requestList_
				this.requestList2 = requestList_2
				this.requestList3 = requestList_3
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
	
	.tag {
		margin-bottom: 5px;
	}
	
	.tag2 {
		margin-top: 10px;
		margin-bottom: 5px;
	}
	
	.tag3 {
		margin-top: 10px;
		margin-bottom: 5px;
	}
</style>
