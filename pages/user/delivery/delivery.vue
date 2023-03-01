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
				</template>
		    </uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				commodityList: [],
			}
		},
		methods: {
			
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
</style>
