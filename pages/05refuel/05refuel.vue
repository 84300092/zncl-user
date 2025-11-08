<template>
	<view class="all">
		<!-- <image class="bg" src="/static/05refuel/background@3x.png" mode=""></image> -->
		<view class="bg">
			
		</view>
		<!-- 搜索栏 -->
		<view class="search">
			<view class="search-input">
				<input type="text" placeholder="输入需要搜索的内容" v-model="input"/>
				<image src="/static/05refuel/search@3x.png" mode=""></image>
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
			<view class = "station" >
					<view class="station-item" v-for="item in gasStation" :key="item" @click="showStationOnMap(item)" > 
						<view class="station-item-top">
							<view>{{item.name}}</view>
							<view>{{item.distance}}</view>
						</view>
						<view class="station-item-discount">
							<view class="discount-item" v-for="it in item.discount" :key="it" >
								{{it}}
							</view>
						</view>
						<view class="station-item-des">
							<image :src="item.image" alt=""/>
							<view class="desRight">
								<view>营业时间：{{item.businessTime}}</view>
								<view>￥{{item.price}}/升</view>
								<view class="address">
									<view>{{item.address}}</view>
									<image src="/static/05refuel/plane@3x.png" alt="" />
								</view>
							</view>
						</view>
					</view>
			</view>
		</view>
		<view v-if="model==='列表模式'" class="geo">
			
			<map
				:latitude="latitude"
				:longitude="longitude"
				min-scale="14"
				enable-pan
				enable-scroll
				enable-zoom
				:markers="markers"
				:show-location="true"
				@markertap="handleMarkerTap"
				style="width: 100%; height: 800rpx;"
			></map>
			<!-- 加油站信息 -->
			<view class="stationDel">
				<view class="stationDelLeft">
					<view>中国石油加油站</view>
					<view>山阳区人民路与解放路交叉口</view>
					<view class="storeDel">
						<image src="/static/05refuel_geo/detail@2x.png" alt="" />
						<!-- <view>门店详情</view> -->
						<rich-text nodes="门店详情 >>" @click="stationDel"></rich-text>
					</view>
				</view>
				<view class="stationDelRight">
					<image src="/static/05refuel/background@3x.png" alt="" />
				</view>
			</view>
			<button type="primary" @click="toStation">立即导航</button>
		</view>
		
	</view>
	
</template>

<script setup>
import { ref, onMounted } from 'vue';

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
		address: "山阳区人民路与解放路交叉口",
		latitude: 34.825817,  // 纬度
		longitude: 113.55952  // 经度
	},
	{
		name: "中国石油加油站",
		distance: "556m",
		discount: ["满200减20","满500减60"],
		image: '/static/05refuel/background@3x.png',
		businessTime: "周一至周五",
		price: "5.66",
		address: "中国石油加油站(雪松路站)",
		latitude: 34.82845,  // 纬度
		longitude: 113.55785500000002,  // 经度
	}
])

// 地图相关数据
const latitude = ref(0);       // 中心纬度（用户位置）
const longitude = ref(0);      // 中心经度（用户位置）
const markers = ref([]);       // 地图标记点
const selectedStation = ref(null);  // 当前选中的加油站

// 页面加载时获取用户位置并初始化地图标记
onMounted(() => {
	getUserLocation();
	initMarkers();
});

// 获取用户当前位置
const getUserLocation = () => {
	uni.getLocation({
		type: 'gcj02',  // 高德地图坐标系（中国标准）
		success: (res) => {
			latitude.value = res.latitude;
			longitude.value = res.longitude;
			// 更新地图中心到用户位置
			updateMarkers();
		},
		fail: (err) => {
			console.error('获取位置失败：', err);
			uni.showToast({ title: '请开启位置权限', icon: 'none' });
			//  fallback：使用默认位置
			latitude.value = 34.825817;
			longitude.value = 113.55852;
		}
	});
};

// 初始化加油站标记
const initMarkers = () => {
	markers.value = gasStation.value.map(station => ({
		id: station.id,
		latitude: station.latitude,
		longitude: station.longitude,
		title: station.name,
		iconPath: '/static/05refuel_geo/geo@3x.png',  // 加油站标记图标
		width: 20,  // 图标宽度（rpx）
		height: 20, // 图标高度（rpx）
		callout: {  // 点击标记时显示的气泡
			content: `${station.name}\n￥${station.price}/升`,
			color: '#333',
			bgColor: '#fff',
			fontSize: 14,
			borderRadius: 4,
			padding: 8,
			display: 'BYCLICK'  // 点击时显示
		}
	}));
};

// 更新标记（添加用户位置标记）
const updateMarkers = () => {
	if (latitude.value && longitude.value) {
		// 添加用户位置标记（id=0确保唯一）
		markers.value.unshift({
			id: 0,
			latitude: latitude.value,
			longitude: longitude.value,
			iconPath: '/static/05refuel_nav111/group@3x.png',  // 用户位置图标
			width: 40,
			height: 40,
			zIndex: 10  // 层级高于加油站标记
		});
	}
};

// 切换模式
const switchModel = () => {
		if(model.value === "地图模式"){
			model.value = "列表模式"
		}else{
			model.value = "地图模式"
			getUserLocation();
		}
	// 切换到地图模式时重新定位
	// if (model.value === "列表模式") {
	// 	getUserLocation();
	// }
};

// 点击列表项，在地图上高亮显示对应加油站
const showStationOnMap = (station) => {
	model.value = "列表模式";  // 切换到地图
	// 移动地图中心到该加油站
	latitude.value = station.latitude;
	longitude.value = station.longitude;
	// 选中该加油站
	selectedStation.value = station;
};

// 点击地图标记
const handleMarkerTap = (e) => {
	const markerId = e.markerId;
	// 过滤掉用户位置标记（id=0）
	if (markerId !== 0) {
		selectedStation.value = gasStation.value.find(station => station.id === markerId);
	}
};

// 查看详情
const viewDetail = () => {
	uni.showToast({ title: `查看${selectedStation.value.name}详情`, icon: 'none' });
};

// 导航到加油站
const toStation = () => {
	// if (!selectedStation.value) return;
	// // 调用地图导航（支持高德/百度/腾讯地图）
	// uni.openLocation({
	// 	latitude: selectedStation.value.latitude,
	// 	longitude: selectedStation.value.longitude,
	// 	name: selectedStation.value.name,
	// 	address: selectedStation.value.address,
	// 	scale: 18
	uni.openLocation({
		latitude: gasStation.value[1].latitude,
		longitude: gasStation.value[1].longitude,
		name: gasStation.value[1].name,
		address: gasStation.value[1].address,
		scale: 18
	
	});
};
// 详情页
const stationDel=()=>{
	uni.navigateTo({
		url: '/pages/05refuel/stationDel/stationDel',
		success: res => {},
		fail: () => {},
		complete: () => {}
	});
}	
	
</script>

<style scoped>
/* 全局背景 */
.all{
	padding-top: 10rpx;
	box-sizing: border-box; 
	position: relative;
	z-index: 1;
}
.bg{
	background-color: #0472fe;
	width: 100%;
	height: 300rpx;
	position: absolute;
	z-index: -1;
	top: 0;
	right: 0;
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
	/* width: 600rpx; */
	width: calc(100% - 120rpx); /* 改用计算宽度，预留按钮空间 */
	display: flex;
	justify-content: space-between;
	border-radius: 50rpx;
	opacity: .5;
}
.search-input>image{
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
.station-item-des>image{
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
.desRight>view>image{
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
.stationDel{
	display: flex;
	justify-content: space-between;
	padding: 30rpx;
}
.stationDelLeft :nth-child(2){
	font-size: 24rpx;
	margin: 10rpx 0;
}
.storeDel{
	display: flex;
	line-height: 10rpx;
	color: #0472fe;
}
.storeDel image{
	width: 30rpx;
	height: 30rpx;
	margin-right: 5rpx;
}
.stationDelRight image{
	width: 200rpx;
	height: 100rpx;
	margin-left: 20rpx;
	border-radius: 15rpx;
}
button{
	border-radius: 50rpx;
	height: 60rpx;
	font-size: 26rpx;
}
</style>