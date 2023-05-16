<template>
	<view>
		<uni-section class="mb-10" title="已解决" type="line">
			<uni-collapse accordion v-for="(request, index) in solvedRequest">
				<uni-collapse-item :title="'请求'+(index+1)">
					<uni-card title="请求:" :thumbnail="image1">
						<view>
							<text class="text-black text-bold">{{request.requestContent}}</text>
						</view>
						<view>
							<text class="text-red">请求类型：{{request.requestType}}</text>
						</view>
						<view>
							<text class="text-red">时效要求：{{request.timeRequire}}</text>
						</view>
						<view>
							<uni-dateformat :date="request.requestTime" class="text-blue"></uni-dateformat>
						</view>
					</uni-card>
					<uni-card title="回复:" :thumbnail="image2">
						<text class="text-black text-bold">{{request.responseContent}}</text>
						<view>
							<uni-dateformat :date="request.responseTime" class="text-blue"></uni-dateformat>
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
			<view class="tag">
				<uni-tag text="紧急" type="error"/>
			</view>
			<uni-collapse v-for="(request, index) in unsolvedRequest" :key="request.requestTime">
				<uni-collapse-item :title="'请求'+(index+1)">
					<uni-card title="请求:" :thumbnail="image1">
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
				</uni-collapse-item>
			</uni-collapse>
			<view class="tag2">
				<uni-tag text="尽快" type="warning"/>
			</view>
			<uni-collapse v-for="(request, index) in unsolvedRequest2" :key="request.requestTime">
				<uni-collapse-item :title="'请求'+(index+1)">
					<uni-card title="请求:" :thumbnail="image1">
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
				</uni-collapse-item>
			</uni-collapse>
			<view class="tag3">
				<uni-tag text="正常" type="primary"/>
			</view>
			<uni-collapse v-for="(request, index) in unsolvedRequest3" :key="request.requestTime">
				<uni-collapse-item :title="'请求'+(index+1)">
					<uni-card title="请求:" :thumbnail="image1">
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
				</uni-collapse-item>
			</uni-collapse>
		</uni-section>
	</view>
	<view>
		<uni-section title="提出请求" padding titleFontSize="16px" type="square">
			<uni-easyinput type="textarea" v-model="value" placeholder="请输入内容"></uni-easyinput>
			<view class="requestType">
				<text>请求类型</text>
				<uni-data-select v-model="value2" :localdata="range2" @change="change2" :clear="false"></uni-data-select>
			</view>
			<view class="timeRequire">
				<text>时效要求</text>
				<uni-data-select v-model="value3" :localdata="range3" @change="change3" :clear="false"></uni-data-select>
			</view>
			<button class="btn-submit" type="primary" plain="true" size="mini" @click="submitRequest()">提交</button>
		</uni-section>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				image1: '',
				image2: '/static/images/user.png',
				value: '',
				value2: -1,
				value3: -1,
				name: '',
				username: '',
				solvedRequest: [],
				unsolvedRequest: [],
				unsolvedRequest2: [],
				unsolvedRequest3: [],
				range2: [
					{value: 0, text: '水'},
					{value: 1, text: '电'},
					{value: 2, text: '燃气'},
					{value: 3, text: '其他'}
				],
				range3: [
					{value: 0, text: '紧急'},
					{value: 1, text: '尽快'},
					{value: 2, text: '正常'}
				]
			}
		},
		onLoad() {
			this.name = uni.getStorageSync('name')
			this.username = uni.getStorageSync('username')
			this.image1 = uni.getStorageSync('image')
			const db = uniCloud.database()
			db.collection('request').get().then((res)=>{
				let len = res.result.data.length
				if (len > 0) {
					let solvedRequest2 = []
					let unsolvedRequest_ = []
					let unsolvedRequest_2 = []
					let unsolvedRequest_3 = []
					for (let i = 0; i < len; i++) {
						if (res.result.data[i].name == this.name) {
							if (res.result.data[i].solved) {
								solvedRequest2.push({
									requestContent: res.result.data[i].requestContent,
									requestTime: res.result.data[i].requestTime,
									responseContent: res.result.data[i].responseContent,
									responseTime: res.result.data[i].responseTime,
									requestType: res.result.data[i].requestType,
									timeRequire: res.result.data[i].timeRequire
								})
							} else {
								if (res.result.data[i].timeRequire === "紧急")
									unsolvedRequest_.push({
										requestContent: res.result.data[i].requestContent,
										requestTime: res.result.data[i].requestTime,
										requestType: res.result.data[i].requestType,
										timeRequire: res.result.data[i].timeRequire
									})
								else if (res.result.data[i].timeRequire === "尽快")
									unsolvedRequest_2.push({
										requestContent: res.result.data[i].requestContent,
										requestTime: res.result.data[i].requestTime,
										requestType: res.result.data[i].requestType,
										timeRequire: res.result.data[i].timeRequire
									})
								else
									unsolvedRequest_3.push({
										requestContent: res.result.data[i].requestContent,
										requestTime: res.result.data[i].requestTime,
										requestType: res.result.data[i].requestType,
										timeRequire: res.result.data[i].timeRequire
									})
							}
						}
					}
					this.solvedRequest = solvedRequest2
					this.unsolvedRequest = unsolvedRequest_
					this.unsolvedRequest2 = unsolvedRequest_2
					this.unsolvedRequest3 = unsolvedRequest_3
				}
			}).catch((err)=>{
				console.log(err.code)
				console.log(err.message)
			})
		},
		methods: {
			submitRequest() {
				if (this.value != "" && this.value2 != -1 && this.value3 != -1) {
					let timestamp = new Date().getTime()
					const db = uniCloud.database()
					db.collection('request').add({
						name: this.name,
						username: this.username,
						requestContent: this.value,
						requestTime: timestamp,
						solved: false,
						responseContent: "",
						responseTime: 0,
						requestType: this.range2[this.value2].text,
						timeRequire: this.range3[this.value3].text
					}).then((res)=>{
						uni.showToast({
							title: '请求提交成功',
							icon: 'success'
						})
						this.value = ""
						db.collection('request').get().then((res)=>{
							let len = res.result.data.length
							if (len > 0) {
								let solvedRequest2 = []
								let unsolvedRequest_ = []
								let unsolvedRequest_2 = []
								let unsolvedRequest_3 = []
								for (let i = 0; i < len; i++) {
									if (res.result.data[i].name == this.name) {
										if (res.result.data[i].solved) {
											solvedRequest2.push({
												requestContent: res.result.data[i].requestContent,
												requestTime: res.result.data[i].requestTime,
												responseContent: res.result.data[i].responseContent,
												responseTime: res.result.data[i].responseTime,
												requestType: res.result.data[i].requestType,
												timeRequire: res.result.data[i].timeRequire
											})
										} else {
											if (res.result.data[i].timeRequire === "紧急")
												unsolvedRequest_.push({
													requestContent: res.result.data[i].requestContent,
													requestTime: res.result.data[i].requestTime,
													requestType: res.result.data[i].requestType,
													timeRequire: res.result.data[i].timeRequire
												})
											else if (res.result.data[i].timeRequire === "尽快")
												unsolvedRequest_2.push({
													requestContent: res.result.data[i].requestContent,
													requestTime: res.result.data[i].requestTime,
													requestType: res.result.data[i].requestType,
													timeRequire: res.result.data[i].timeRequire
												})
											else
												unsolvedRequest_3.push({
													requestContent: res.result.data[i].requestContent,
													requestTime: res.result.data[i].requestTime,
													requestType: res.result.data[i].requestType,
													timeRequire: res.result.data[i].timeRequire
												})
										}
									}
								}
								this.solvedRequest = solvedRequest2
								this.unsolvedRequest = unsolvedRequest_
								this.unsolvedRequest2 = unsolvedRequest_2
								this.unsolvedRequest3 = unsolvedRequest_3
							}
						}).catch((err)=>{
							console.log(err.code)
							console.log(err.message)
						})
					})
				}
				else {
					uni.showToast({
						title: '请求提交失败',
						icon: 'error'
					})
				}
			},
			change2(e) {
				console.log(this.range2[e].text)
			},
			change3(e) {
				console.log(this.range3[e].text)
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
	
	.btn-submit {
		margin-top: 5px;
	}
	
	.requestType {
		margin-top: 5px;
	}
	
	.timeRequire {
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
