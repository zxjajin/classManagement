<template>
	<view class="content">
		<view  >
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
			<radio name="power"  value="1" :checked="student.power">是</radio>
			<radio name="power" value="0" :checked="!student.power">否</radio>
			</radio-group>
		</view>
		<text class="bt">
			<button @click="update()" type="primary">修改</button>
			<button @click="del()" type="warn">删除</button>
		</text>
	</view>
</template>

<script>
	import {baseUrl} from "../../config.js"
	export default {
		data() {
			return {
				student:{
					id:'',
					name:'',
					photo:'',
					phone:'',
					parentName:'',
					parentPhone:'',
					address:'',
					power:''
				}
			}
		},
		onLoad(e) {
			console.log(e)
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
			update(){
				uni.request({
					url: baseUrl + '/student',
					method:'PUT',
					// data:that.student,
					data:this.student,
					success: (res) => {
						// console.log(res)
						const data = res.data
						if(data.code === 200){
							uni.showToast({
								title:"修改成功！",
								icon:"success",
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
								title:"电话输入有误!",
								icon:"error"
							})
						}
					}
				})
			},
			del(){
				const result =  confirm("确认删除？")
				if(result){
					uni.request({
						// url:'http://8.138.58.49:8080/student/'+this.student.id,
						url: baseUrl +'/student/'+this.student.id,
						method:'DELETE',
						success: (res) => {
							// console.log(res)
							const data = res.data
							if(data.code === 200){
								uni.showToast({
									title:"删除成功！",
									icon:"success",
									success: () => {
										setTimeout(() => {
											uni.switchTab({
												url: '/pages/index/index'
											})
										}, 1500)
									}
								})
							}else{
								uni.showToast({
									title:"删除失败",
									icon:"error"
								})
							}
						}
					})
				}
			},
			chan(e){
				// console.log(e)
				if(e.detail.value == 1){
					this.student.power = true
				}else{
					this.student.power = false
				}
			}
			
		}
	}
</script>

<style>
.content view{
		display: flex;
		flex-direction: row;
		align-items: center;
		margin-left: 100rpx;
		margin-top: 20rpx;
	}
	input{
		border-bottom: 1px solid #ccc;
	}
	.bt{
		display: flex;
		flex-direction: column;
		justify-self: center;
		margin: 20rpx;
	}
	.bt button{
		margin-bottom: 10rpx;
	}
</style>
