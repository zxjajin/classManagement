<template>
	<view class="content">
		<view>学号：{{student.id}}</view>
		<view>
			姓名：<input type="text" v-model="student.name">
		</view>
		<view>
			缺勤：<input type="text" v-model="student.absent">
		</view>
		
		<text class="bt">
			<button @click="add()" type="primary">登记</button>
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
					absent:''
				}
			}
		},
		onLoad(e) {
			// console.log(e)
			this.getstudent(e.id);
		},
		methods: {
			getstudent(id){
			uni.request({
				url:baseUrl + '/student/'+id,
				success: res => {
					this.student = res.data.data
					// console.log(this.student)
				}
			})
			},
			add() {
				uni.request({
					url: baseUrl + '/absent',
					method: 'POST',
					data: this.student,
					success: (res) => {
						// console.log(res)
						const data = res.data
						// console.log(data)
						if (data.code === 200) {
							uni.showToast({
									title: "登记成功！",
									icon: "success",
									success: (res) => {
										setTimeout(() => {
											uni.switchTab({
												url: '/pages/checking/checking'
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
