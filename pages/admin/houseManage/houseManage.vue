<template>
	<view>
		<uni-list v-for="(house, index) in houseList">
			<uni-card :cover="house.url">
				<text class="uni-body">{{house.text}}</text>
			</uni-card>
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				houseList: [
					{
						text: '1栋',
						url: '/static/images/house/house1.jpeg'
					},
					{
						text: '2栋',
						url: '/static/images/house/house2.jpg'
					},
					{
						text: '3栋',
						url: '/static/images/house/house3.jpeg'
					},
					{
						text: '4栋',
						url: '/static/images/house/house4.jpeg'
					},
					{
						text: '5栋',
						url: '/static/images/house/house5.jpeg'
					}
				]
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	.uni-body {
		font-weight: bold;
		font-size: 20px;
	}
</style>
