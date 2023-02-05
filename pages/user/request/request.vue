<template>
	<view>
		<uni-section class="mb-10" title="已解决" type="line">
			<uni-collapse  accordion v-model="accordionVal">
				<uni-collapse-item title="请求1">
					<uni-card title="请求:" :thumbnail="image1">
						<text>第一个请求</text>
					</uni-card>
					<uni-card title="回复:" :thumbnail="image2">
						<text>第一个回复</text>
					</uni-card>
				</uni-collapse-item>
				<uni-collapse-item title="请求2">
					<uni-card title="请求:" :thumbnail="image1">
						<text>第二个请求</text>
					</uni-card>
					<uni-card title="回复:" :thumbnail="image2">
						<text>第二个回复</text>
					</uni-card>
				</uni-collapse-item>
				<uni-collapse-item title="请求3">
					<uni-card title="请求:" :thumbnail="image1">
						<text>第三个个请求</text>
					</uni-card>
					<uni-card title="回复:" :thumbnail="image2">
						<text>第三个回复</text>
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
			<uni-collapse>
				<uni-collapse-item title="未解决请求1">
					<uni-card title="请求:" :thumbnail="image1">
						<text>第一个请求</text>
					</uni-card>
					<uni-card title="回复:" :thumbnail="image2">
						<text>第一个回复，我希望不要有bug</text>
					</uni-card>
				</uni-collapse-item>
			</uni-collapse>
		</uni-section>
	</view>
	<view>
		<uni-section class="mb-10" title="提出请求">
			
		</uni-section>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				accordionVal: '',
				image1: '',
				image2: '/static/images/user.png'
			}
		},
		onLoad() {
			this.image1 = uni.getStorageSync('image')
		},
		methods: {
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
