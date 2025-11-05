<template>
	<view class="all">
		<!-- 搜索栏 -->
		<view class="search">
			<view class="search-input">
				<input type="text" placeholder="输入需要搜索的内容" v-model="input"/>
				<img src="/static/05refuel/search@3x.png" alt="" mode="aspectFit"/>
			</view>
			<view class="model">
				<view @click="switchModel">{{model}}</view>
			</view>
		</view>
		<view v-if="model==='地图模式'">
			<!-- 汽油型号 -->
			<view class="petrol">
				<view v-for="item in petrol" :key="item" class="petrol-item">{{item}}</view>
			</view>
			<!-- 加油站 -->
			<view class="station">
					<view class="station-item" v-for="item in gasStation" :key="item" v-model="gasStation"> 
						<view class="station-item-top">
							<view>{{item.name}}</view>
							<view>{{item.distance}}</view>
						</view>
						<view class="station-item-discount">
							<view class="discount-item" v-for="it in item.discount" :key="it" v-model="item.discount">
								{{it}}
							</view>
						</view>
						<view class="station-item-des">
							<img :src="item.image" alt=""/>
							<view class="desRight">
								<view>营业时间：{{item.businessTime}}</view>
								<view>￥{{item.price}}/升</view>
								<view class="address">
									<view>{{item.address}}</view>
									<img src="/static/05refuel/plane@3x.png" alt="" />
								</view>
							</view>
						</view>
					</view>
			</view>
		</view>
		<view v-if="model==='列表模式'" class="geo">
			aa
			<map name=""></map>
		</view>
		
	</view>
	
</template>

<script setup>
import { ref } from 'vue';

const model = ref("地图模式")
const input = ref("")
const petrol = ref(["90#","92#","93#","95#","97#","98#"])
const gasStation = ref([
	{
		name: "中国石油加油站",
		distance: "556m",
		discount: ["满200减20","满500减60"],
		image: '/static/05refuel/background@3x.png',
		businessTime: "周一至周五",
		price: "5.66",
		address: "山阳区人民路与解放路交叉口"
	},
	{
		name: "中国石油加油站",
		distance: "556m",
		discount: ["满200减20","满500减60"],
		image: '/static/05refuel/background@3x.png',
		businessTime: "周一至周五",
		price: "5.66",
		address: "山阳区人民路与解放路交叉口"
	}
])
const switchModel = ()=>{
	if(model.value === "地图模式"){
		model.value = "列表模式"
	}else{
		model.value = "地图模式"
	}
	
	
}
</script>

<style>
/* 全局背景 */
.all{
	background-image: url(/static/05refuel/background@3x.png);
	width: 100%;
	height: 300rpx;
	padding-top: 10rpx; /* 将margin改为padding */
	box-sizing: border-box; /* 确保内边距不会增加总宽度 */
}
/* 搜索栏 */
.search{
	margin: 20rpx;
	display: flex;
	
}
.search-input{
	background-color: #fff;
	margin-top: 10rpx;
	padding: 20rpx;
	box-sizing: border-box;
	height: 80rpx;
	width: 600rpx;
	display: flex;
	justify-content: space-between;
	border-radius: 50rpx;
	opacity: .5;
}
.search-input img{
	width: 40rpx;
	height: 40rpx;
	margin: 0rpx;
}
.model{
	width: 100rpx;
	height: 100rpx;
	background-image: url(/static/05refuel/group5.png);
	background-size: 100% 100%;
	padding: 20rpx 25rpx;
	box-sizing: border-box;
	font-size: 25rpx;
	color: #fff;
	margin-left: 10rpx;
}
/* 汽油选择 */
.petrol{
	display: flex;
	justify-content: space-around;
	color: #fff;
	padding: 0 30rpx;
}
.petrol-item{
	padding: 5rpx 20rpx;	
	/* height: 30rpx; */
	border-radius: 15rpx;
	border: 1rpx solid #fff;
	font-size: 26rpx;
}

/* 加油站 */
.station{
	padding: 20rpx;
}
.station-item{
	padding: 30rpx;
	background-color: #fff;
	border-radius: 20rpx;
	margin-bottom: 20rpx;
}
.station-item-top{
	display: flex;
	justify-content: space-between;
}
.station-item-top :nth-child(2){
	color: #0472fe;
	font-size: 24rpx;
	padding-top: 10rpx;
}
.station-item-discount{
	display: flex;
	margin: 20rpx 0;
}
.discount-item{
	margin-right: 20rpx;
	padding: 1rpx 10rpx;
	border-radius: 10rpx;
	background-color: #ffece5;
	color: #ff4205;
	font-size: 18rpx;
}
.station-item-des{
	display: flex;
	margin: 20rpx 0;
}
.station-item-des>img{
	width: 250rpx;
	height: 150rpx;
	margin-right: 20rpx;
	border-radius: 15rpx;
}
.desRight view{
	font-size: 24rpx;
	margin: 10rpx;
	display: flex;
	
}
.desRight :nth-child(2){
	color: #ff4205;
}
.desRight :nth-child(3){
	margin: -8rpx 3rpx;
}
.desRight view img{
	width: 20rpx;
	height: 20rpx;
	margin-top: 18rpx;
}





/* 地图模式 */
.geo{
	border-radius: 20rpx;
	background-color: #fff;
	margin: 10rpx;
	padding: 20rpx;
}
</style>