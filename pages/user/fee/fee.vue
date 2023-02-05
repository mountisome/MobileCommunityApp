<template>
	<view>
		<uni-list border-full>
			<uni-list-item title="用户名:"  :rightText="username"/>
			<uni-list-item title="物业费:" :rightText="fee"/>
		</uni-list>
	</view>
	<view>
		<uni-datetime-picker v-model="range" type="daterange" @change="change" />
	</view>
</template>

<script>
	export default {
		data() {
			return {
				range: ['', ''],
				date1: '',
				date2: '',
				username: 'user',
				fee: '0'
			}
		},
		onLoad() {
			if (uni.getStorageSync('username') !== '')
				this.username = uni.getStorageSync('username')
		},
		methods: {
			change(e) {
				if (e.length != 0) {
					this.date1 = e[0]
					this.date2 = e[1]
					let year1 = Number(this.date2[3])
					let year2 = Number(this.date1[3])
					let year = year1 - year2
					let month1 = Number(this.date2[5])
					let month2 = Number(this.date2[6])
					let month3 = Number(this.date1[5])
					let month4 = Number(this.date1[6])
					let month = month1 * 10 + month2 - month3 * 10 - month4
					let day1 = Number(this.date2[8])
					let day2 = Number(this.date2[9])
					let day3 = Number(this.date1[8])
					let day4 = Number(this.date1[9])
					let day = day1 * 10 + day2 - day3 * 10 - day4
					this.fee = String((year * 365 + month * 30 + day) * 2)
				}
			}
		}
	}
</script>

<style lang="scss">

</style>
