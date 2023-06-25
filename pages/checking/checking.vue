<template>
	<view class="content">
		<view class="main" v-for="item in list" :key="item.id">
			<view>学号：{{item.id}}</view>
			<text>姓名：{{item.name}}</text>
			<view class="ml">
				<text v-if="item.absent==0">无缺勤记录</text>
				<text v-else-if="item.absent==1">缺勤1节课</text>
				<text v-else-if="item.absent==2">缺勤2节课</text>
				<text v-else-if="item.absent==3">缺勤3节课</text>
				<text v-else-if="item.absent==4">缺勤4节课</text>
				<text v-else-if="item.absent==5">已到达缺课警戒线</text>
				<text v-else>缺勤已达处分</text>
			</view>
			<navigator :url="'/pages/absent/absent?id='+item.id"><button type="primary"   class="cent">登记</button></navigator>
		</view>
		
		<view>
			<text v-for="num in pages">
				<text class="tex" @click="page(num)"> {{ num }} </text>
			<!-- 	<text @click="page(currentPage+1)">{{currentPage+1}}</text> -->
			</text>
		</view>

		<navigator class="b_t" url="/pages/leave/leave"><button class="b_t" @click="add" type="primary">请假</button></navigator>
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
					absent:'',
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
		margin-top: 20rpx;
	}
	.b_t{
		width: 100%;
		margin:10rpx 0rpx;
	}
	.main{
		margin-left: 15rpx;
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
    .ml{
		text-align: center;
	}
	.ml{
		margin-left: -25rpx;
	}
	.tex{
		padding: 20rpx;
		color: darkgreen;
	}

</style>
