<template>
	<view v-if="identity === 'user'">
		<view class="function">
			<uni-grid :column="4" :showBorder="false" :highlight="true" @change="change">
				<uni-grid-item v-for="(item, index) in list" :index="index" :key="index">
					<view class="grid-item-box" style="background-color: #fff;">
						<image :src="item.url" class="image" mode="aspectFill" />
						<text class="text">{{item.text}}</text>
					</view>
				</uni-grid-item>
			</uni-grid>
		</view>
		<view class="uni-margin-wrap">
			<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
				:duration="duration">
				<swiper-item>
					<view class="swiper-item">
						<image :src="image1" mode="aspectFill" />
					</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">
						<image :src="image2" mode="aspectFill" />
					</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">
						<image :src="image3" mode="aspectFill" />
					</view>
				</swiper-item>
			</swiper>
		</view>
		<view class="noticeList">
			<uni-notice-bar show-icon scrollable text="下面是社区的通知公告,请注意查看!" />
			<uni-list v-for="(notice, index) in importantNoticeList" :key="index">
				<uni-list-item>
					<template v-slot:header>
						<text class="importantInfo">{{notice.info}}!!</text>
					</template>
				</uni-list-item>
				<uni-dateformat :date="notice.time" style="margin-left: 15px; margin-bottom: 10px;"></uni-dateformat>
			</uni-list>
			<uni-list v-for="(notice, index) in noticeList" :key="index">
				<uni-list-item :title="notice.info">
				</uni-list-item>
				<uni-dateformat :date="notice.time" style="margin-left: 15px; margin-bottom: 10px;"></uni-dateformat>
			</uni-list>
		</view>
	</view>
	
	<view v-else-if="identity === 'admin'">
		<view class="function2">
			<uni-grid :column="4" :showBorder="false" :highlight="true" @change="change">
				<uni-grid-item v-for="(item, index) in list2" :index="index" :key="index">
					<view class="grid-item-box" style="background-color: #fff;">
						<image :src="item.url" class="image" mode="aspectFill" />
						<text class="text">{{item.text}}</text>
					</view>
				</uni-grid-item>
			</uni-grid>
		</view>
		<view class="uni-margin-wrap">
			<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
				:duration="duration">
				<swiper-item>
					<view class="swiper-item">
						<image :src="image1" mode="aspectFill" />
					</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">
						<image :src="image2" mode="aspectFill" />
					</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">
						<image :src="image3" mode="aspectFill" />
					</view>
				</swiper-item>
			</swiper>
		</view>
		<view class="noticeList">
			<uni-notice-bar show-icon scrollable text="下面是用户的通知,请注意查看!" />
			<uni-list v-for="(notice, index) in adminNoticeList" :key="index">
				<uni-list-item :title="notice.info">
				</uni-list-item>
				<uni-dateformat :date="notice.time" style="margin-left: 15px; margin-bottom: 10px;"></uni-dateformat>
			</uni-list>
		</view>
	</view>
	
	<view v-else-if="identity === 'market'">
		<view class="function">
			<uni-grid :column="4" :showBorder="false" :highlight="true" @change="change">
				<uni-grid-item v-for="(item, index) in list3" :index="index" :key="index">
					<view class="grid-item-box" style="background-color: #fff;">
						<image :src="item.url" class="image" mode="aspectFill" />
						<text class="text">{{item.text}}</text>
					</view>
				</uni-grid-item>
			</uni-grid>
		</view>
		<view class="uni-margin-wrap">
			<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
				:duration="duration">
				<swiper-item>
					<view class="swiper-item">
						<image :src="image4" mode="aspectFill" />
					</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">
						<image :src="image5" mode="aspectFill" />
					</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">
						<image :src="image6" mode="aspectFill" />
					</view>
				</swiper-item>
			</swiper>
		</view>
		<view class="noticeList">
			<uni-notice-bar show-icon scrollable text="下面是用户的商品需求,请注意查看!" />
			<uni-list v-for="(notice, index) in marketNoticeList" :key="index">
				<uni-list-item :title="notice.info">
				</uni-list-item>
				<uni-dateformat :date="notice.time" style="margin-left: 15px; margin-bottom: 10px;"></uni-dateformat>
			</uni-list>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				identity: '',
				list: [
					{
						text: '物业费',
						url: '/static/images/fee.png'
					},
					{
						text: '服务请求',
						url: '/static/images/request.png'
					},
					{
						text: '配送商品',
						url: '/static/images/delivery.png'
					}
				],
				list2: [
					{
						text: '业主信息',
						url: '/static/images/userManage.png'
					},
					{
						text: '房屋信息',
						url: '/static/images/houseManage.png'
					},
					{
						text: '服务请求',
						url: '/static/images/requestHandle.png'
					},
					{
						text: '物业收费',
						url: '/static/images/fee2.png'
					},
					{
						text: '通知发布',
						url: '/static/images/notice.png'
					}
				],
				list3: [
					{
						text: '商品信息',
						url: '/static/images/commodity.png'
					}
				],
				indicatorDots: true,
				autoplay: true,
				interval: 2000,
				duration: 500,
				image1: '/static/images/swiper1.jpg',
				image2: '/static/images/swiper2.jpeg',
				image3: '/static/images/swiper3.jpeg',
				image4: '/static/images/market/market1.jpeg',
				image5: '/static/images/market/market2.jpg',
				image6: '/static/images/market/market3.jpeg',
				importantNoticeList: [],
				noticeList: [],
				adminNoticeList: [],
				marketNoticeList: []
			}
		},
		onLoad() {
			this.identity = uni.getStorageSync('identity')
			let username = uni.getStorageSync('username')
			let adminname = uni.getStorageSync('adminname')
			const db = uniCloud.database()
			db.collection('notice').get().then((res)=>{
				if (res.result.data === undefined) {
					this.noticeList = ['无消息通知']
				} else {
					let len = res.result.data.length
					let importantNoticeList2 = []
					let noticeList2 = []
					let adminNoticeList2 = []
					let marketNoticeList2 = []
					for (let i = 0; i < len; i++) {
						if (res.result.data[i].name === username) {
							importantNoticeList2.push({
								info: res.result.data[i].info,
								time: res.result.data[i].time
							})
						} else if (res.result.data[i].name === 'ALL') {
							noticeList2.push({
								info: res.result.data[i].info,
								time: res.result.data[i].time
							})
						} else if (res.result.data[i].name === 'admin') {
							adminNoticeList2.push({
								info: res.result.data[i].info,
								time: res.result.data[i].time
							})
						} else if (res.result.data[i].name === 'market') {
							marketNoticeList2.push({
								info: res.result.data[i].info,
								time: res.result.data[i].time
							})
						}
					}
					this.importantNoticeList = importantNoticeList2
					this.noticeList = noticeList2
					this.adminNoticeList = adminNoticeList2
					this.marketNoticeList = marketNoticeList2
				}
			}).catch((err)=>{
				console.log(err.code)
				console.log(err.message)
			})
		},
		methods: {
			change(e) {
				let {index} = e.detail
				if (this.identity === 'user') {
					if (index === 0) {
						uni.navigateTo({
							url: '/pages/user/fee/fee'
						})
					} else if (index === 1) {
						uni.navigateTo({
							url: '/pages/user/request/request'
						})
					} else if (index === 2) {
						uni.navigateTo({
							url: '/pages/user/delivery/delivery'
						})
					}
				} else if (this.identity === 'admin') {
					if (index === 0) {
						uni.navigateTo({
							url: '/pages/admin/userManage/userManage'
						})
					} else if (index === 1) {
						uni.navigateTo({
							url: '/pages/admin/houseManage/houseManage'
						})
					} else if (index === 2) {
						uni.navigateTo({
							url: '/pages/admin/requestHandle/requestHandle'
						})
					} else if (index === 3) {
						uni.navigateTo({
							url: '/pages/admin/fee/fee'
						})
					} else if (index === 4) {
						uni.navigateTo({
							url: '/pages/admin/notice/notice'
						})
					}
				} else if (this.identity === 'market') {
					if (index === 0) {
						uni.navigateTo({
							url: '/pages/market/commodity/commodity'
						})
					}
				}
			}
		}
	}
</script>

<style lang="scss">
	.function {
		margin-top: 10px;
		background-color: white;
		height: 100px;
	}
	
	.function2 {
		margin-top: 10px;
		background-color: white;
		height: 195px;
	}
	
	.image {
		width: 45px;
		height: 45px;
	}

	.text {
		font-size: 15px;
		margin-top: 5px;
	}
	
	.grid-item-box {
		flex: 1;
		// position: relative;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 15px 0;
	}
	
	.uni-margin-wrap {
		width: 100%;
		margin-top: 10px;
	}
	
	.swiper {
		height: 350rpx;
	}
	
	.swiper-item {
		display: block;
		height: 350rpx;
		line-height: 350rpx;
		text-align: center;
	}
	
	.noticeList {
		margin-top: 10px;
		width: 100%;
	}
	
	.importantInfo {
		font-weight: bold;
		color: darkred;
	}
</style>


