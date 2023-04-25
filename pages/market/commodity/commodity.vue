<template>
	<view>
		<uni-list v-for="(commodity, index) in commodityList">
		    <uni-list-item>
				<template v-slot:header>
					<view>
						<image class="uni-thumb" :src="commodity.image" mode="aspectFit"/>
					</view>
				</template>
				<template v-slot:footer>
					<view class="commodity">
						<text>{{commodity.name}}</text>
						<text class="commodityPrice">¥{{commodity.price}}</text>
					</view>
					<view class="delete">
						<uni-icons type="trash-filled" size="25" color="red" @click="deleteCommodity(commodity.name)"></uni-icons>
						<text class="text-red">删除</text>
					</view>
				</template>
		    </uni-list-item>
		</uni-list>
		<uni-fab ref="fab" :pattern="pattern" :horizontal="horizontal"
			:vertical="vertical" @fabClick="addCommodity()"></uni-fab>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				commodityList: [],
				pattern: {
					color: '#7A7E83',
					backgroundColor: '#fff',
					selectedColor: '#007AFF',
					buttonColor: '#007AFF',
					iconColor: '#fff'
				},
				horizontal: 'right',
				vertical: 'bottom',
				direction: 'horizontal'
			}
		},
		methods: {
			deleteCommodity(name) {
				const db = uniCloud.database()
				db.collection('commodity').where({
				    name: name
				}).remove().then((res)=>{
				    uni.showToast({
				        title: '商品删除成功',
				        icon: 'success'
				    })
					db.collection('commodity').get().then((res)=>{
						let len = res.result.data.length
						let commodityList2 = []
						for (let i = 0; i < len; i++) {
							commodityList2.push({
								image: res.result.data[i].image,
								name: res.result.data[i].name,
								price: res.result.data[i].price.toString()
							})
						}
						this.commodityList = commodityList2
					})
				}).catch((err)=>{
				    console.log(err.code)
					console.log(err.message)
				})
			},
			addCommodity() {
				uni.navigateTo({
					url: '/pages/market/addCommodity/addCommodity'
				})
			}
		},
		onShow() {
			const db = uniCloud.database()
			db.collection('commodity').get().then((res)=>{
				let len = res.result.data.length
				let commodityList2 = []
				for (let i = 0; i < len; i++) {
					commodityList2.push({
						image: res.result.data[i].image,
						name: res.result.data[i].name,
						price: res.result.data[i].price.toString()
					})
				}
				this.commodityList = commodityList2
			})
		}
	}
</script>

<style lang="scss">
	.uni-thumb {
		height: 160px;
		width: 160px;
	}
	
	.commodity {
		margin-top: 50px;
		margin-right: 80px;
	}
	
	text {
		display: block;
	}
	
	.commodityPrice {
		margin-top: 10px;
		color: red;
		font-size: 20px;
	}
	
	.delete {
		margin-top: 60px;
	}
</style>
