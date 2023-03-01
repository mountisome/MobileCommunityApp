<template>
	<view class="form">
		<uni-forms :modelValue="baseFormData">
			<uni-forms-item label="商品名称" required>
				<uni-easyinput v-model="baseFormData.name"
				placeholder="请输入商品名称"></uni-easyinput>
			</uni-forms-item>
			<uni-forms-item label="商品价格" required>
				<uni-easyinput v-model="baseFormData.price"
				placeholder="请输入商品价格"></uni-easyinput>
			</uni-forms-item>
			<uni-forms-item label="商品图像" required>
				<uni-file-picker limit="1" v-model="result"></uni-file-picker>
			</uni-forms-item>
		</uni-forms>
		<button type="primary" @click="addCommodity()">提交</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				baseFormData: {
					name: '',
					price: ''
				},
				result: {
					
				}
			}
		},
		methods: {
			addCommodity() {
				const db = uniCloud.database()
				db.collection('commodity').add({
				    name: this.baseFormData.name,
				    price: Number(this.baseFormData.price),
					image: this.result[0].url
				}).then((res)=>{
				    uni.showToast({
						title: '商品添加成功',
						icon: 'success'
					})
					let timestamp = new Date().getTime()
					db.collection('notice').add({
						info: this.baseFormData.name + '商品已添加',
						time: timestamp,
						name: 'admin'
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
	.form {
		padding: 15px;
		background-color: #fff;
	}
	
	.button {
		display: flex;
		align-items: center;
		height: 35px;
		margin-left: 10px;
	}
</style>
