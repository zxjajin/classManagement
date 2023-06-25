<template>
	<view class="content">
		<view class="main" v-for="item in list" :key="item.id">
			<image v-if="item.photo!=null" :src="item.photo" mode="widthFix" /> 
			<text>学号：{{item.id}}</text>
			<text>姓名：{{item.name}}</text>
			<text>电话：{{item.phone}}</text>
			<text>家长：{{item.parentName}}</text>
			<text>家长电话：{{item.parentPhone}}</text>
			<text>地址：{{item.address}}</text>
			<navigator :url="'/pages/student/student?id='+item.id"><button type="primary"   class="cent">编辑</button></navigator>
		</view>
		
		<view>
			<text v-for="num in pages">
				<text class="tex" @click="page(num)"> {{ num }} </text>
			<!-- 	<text @click="page(currentPage+1)">{{currentPage+1}}</text> -->
			</text>
		</view>

		<navigator class="b_t" url="../StudentAdd/StudentAdd"><button class="b_t" @click="add" type="primary">添加</button></navigator>
	</view>
</template>

<script>
	import {baseUrl} from "../../config.js"
	export default {
		data() {
			return {
				list:[],
				currentPage: 1,
				total: 0,
				pageSize: 5,
				pages:'',
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
		onShow() {
			this.page(1);
		},
		methods: {
			page(currentPage){
				uni.request({
					// url:'http://8.138.58.49:8080/student?currenPage=' + currentPage,
					url:baseUrl + "/student?currentPage=" + currentPage,
					
					success: (res) => {
						this.list = res.data.data.records
						// console.log(res.data)
						this.currentPage = res.data.data.current
						this.total = res.data.data.total
						this.pageSize = res.data.data.size
						this.pages =  res.data.data.pages
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
		margin-left: 60rpx;
		margin-bottom: 20rpx;
	}
	.main image{
		width: 200rpx;
		height: 200rpx;
		margin-top: 25rpx;
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
