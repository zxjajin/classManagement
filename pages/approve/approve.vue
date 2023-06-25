<template>
	<view class="content">
		<view class="main" v-for="item in list" :key="item.id">
			<text>学号：{{item.studentId}}</text>
			<text>姓名：{{item.name}}</text>
			<text v-if="item.teacherId == 1">班主任：沈大旺</text>
			<text v-else-if="item.teacherId == 2">班主任：陈凡键</text>
			<text v-else-if="item.teacherId == 3">班主任：付玉珍</text>
			<text v-else-if="item.teacherId == 4">班主任：陈胜娣</text>
			<text>请假原因：{{item.leaveReason}}</text>
			<text>请假时间：{{item.createTime.slice(0,10)}}</text>
			<text v-if="item.leaveSchool">
				离开学校
			</text>
			
			<text v-if="item.leaveSchool">
				前往地点：{{item.arriveAdress}}
			</text>
			<text v-if="item.leaveSchool" class="xxx">
				回校时间：{{item.backSchool.slice(0,10)}}
			</text>
			<view v-if="item.teacherAudit" style="text-align: center;color: darkgreen;">已同意请假</view>
			<view v-else-if="item.refuse" style="text-align: center;color: red;">拒绝请假</view>
			<view v-else><button @click="access(item.id)" type="primary" style="margin-bottom: 10rpx;">同意</button><button type="warn" @click="not(item.id)">拒绝</button></view>
		</view>
		
		<view>
			<text v-for="num in pages">
				<text class="tex" @click="getList(num)"> {{ num }} </text>
			<!-- 	<text @click="page(currentPage+1)">{{currentPage+1}}</text> -->
			</text>
		</view>
	
		
	</view>
</template>

<script>
	import {baseUrl} from '../../config.js'
	export default {
		data() {
			return {
				list:[],
				currentPage: 1,
				total: 0,
				pageSize: 5,
				pages:''
			}
		},
		onShow() {
			// console.log("开始请求")
			this.getList(1)
		},
		methods: {
			getList(currentPage){
				uni.request({
					url:baseUrl + '/attendance?currentPage='+currentPage,
					success: (res) => {
						this.list = res.data.data.records
						console.log(res.data)
						this.currentPage = res.data.data.current
						this.total = res.data.data.total
						this.pageSize = res.data.data.size
						this.pages =  res.data.data.pages
					}
				})
			},
			access(id){
				uni.request({
					url:baseUrl + '/attendance',
					method:'PUT',
					data:{
						id:id,
						teacherAudit:true
					},
					success: (res) => {
						if(res.data.code == 200){
							uni.showToast({
								title:'同意请假',
								icon:'success',
								success: (re) => {
									setTimeout(() => {
										this.getList(1)
									}, 1500)
								}
							})
						}
					}
				})
				
			},
			not(id){
				uni.request({
					url:baseUrl + '/attendance',
					method:'PUT',
					data:{
						id:id,
						refuse:true
					},
					success: (res) => {
						if(res.data.code == 200){
							uni.showToast({
								title:'拒绝请假',
								icon:'success'
							})
						}
					}
				})
			}
		}
	}
</script>

<style>
.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.b_t{
		width: 100%;
		margin:10rpx 0rpx;
	}
	.main{
		/* margin-left: 120rpx; */
		margin-bottom: 10rpx;
	}
	.main image{
		width: 200rpx;
		height: 200rpx;
		margin-top: 20rpx;
		margin-left: 20rpx;
	}
	.main text{
		display: block;
		margin: 10rpx;
	}
	.main .cent{
		width: 100%;
	}
	uni-button{
		margin-left: -20rpx;
		
	}

	.tex{
		padding: 20rpx;
		color: darkgreen;
	}
</style>
