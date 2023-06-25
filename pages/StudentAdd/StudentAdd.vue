<template>
	<view class="content">
		<view>
			姓名：<input type="text" v-model="student.name">
		</view>
		<view>
			照片链接：<input type="text" v-model="student.photo">
		</view>
		<view>
			电话：<input type="text" v-model="student.phone">
		</view>
		<view>
			家长：<input type="text" v-model="student.parentName">
		</view>
		<view>
			家长电话：<input type="text" v-model="student.parentPhone">
		</view>
		<view>
			地址：<input type="text" v-model="student.address">
		</view>
		<view>
			纪委：<radio-group @change="chan">
				<radio name="power" value="1" :checked="student.power">是</radio>
				<radio name="power" value="0" :checked="!student.power">否</radio>
			</radio-group>
		</view>
		<text class="bt">
			<button @click="add()" type="primary">添加</button>
		</text>
	</view>
</template>

<script>
	import {
		baseUrl
	} from "../../config.js"
	export default {
		data() {
			return {
				student: {
					id: '',
					name: '',
					photo: '',
					phone: '',
					parentName: '',
					parentPhone: '',
					address: '',
					power: ''
				}
			}
		},
		methods: {
			add() {
				uni.request({
					url: baseUrl + '/student',
					method: 'POST',
					data: this.student,
					success: (res) => {
						// console.log(res)
						const data = res.data
						// console.log(data)
						if (data.code === 200) {
							uni.showToast({
									title: "添加成功！",
									icon: "success",
									success: () => {
										setTimeout(() => {
											uni.switchTab({
												url: '/pages/index/index'
											})
										}, 1500)
									}
								})
							}else if(data.status === 400){
								uni.showToast({
									title:"请填写正确的数据!",
									icon:"error"
								})
							}
					},fail: (err)=> {
						uni.showToast({
							title:"请求发生错误!",
							icon:"error"
						})
					}


				})
			},
			chan(e) {
				// console.log(e)
				if (e.detail.value == 1) {
					this.student.power = true
				} else {
					this.student.power = false
				}
			}
		}
	}
</script>

<style>
	.content view {
		display: flex;
		flex-direction: row;
		align-items: center;
		margin-left: 100rpx;
		margin-top: 20rpx;
	}

	input {
		border-bottom: 1px solid #ccc;
	}

	.bt {
		display: flex;
		flex-direction: column;
		justify-self: center;
		margin: 20rpx;
	}

	.bt button {
		margin-bottom: 10rpx;
	}
</style>
