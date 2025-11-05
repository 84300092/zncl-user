<template>
	<view class="content">
		<image class="bg" src='/static/index/indexbg.png'></image>
		<view class="top">
			<view>
				<image src="/static/index/xiaoxi.png" mode="" class="consult"></image>
			</view>
			<view class="title">
				首页
			</view>
		</view>
		<view class="search-area">
			<view >
				<image src="/static/index/dingwei.png" mode="" class="search-icon"></image>
			</view>
			<view class="search-input">
				<input type="text" placeholder="请输入地点" v-model="searchValue"/>
			</view>
			<view class="refresh">
				<view>
					<image src="/static/index/shuaxin.png" mode="" class="search-icon"></image>
				</view>
				<view class="refresh-text">
					刷新
				</view>
			</view>
		</view>
		<view class="option-area">
			<view class="option-image">
				<view class="option" style="background-color: blue;">
					<image src="/static/index/weixiu.png" mode=""></image>
				</view>
				<view class="option2" style="background-color: orange;">
					<image src="/static/index/baoyang.png" mode=""></image>
				</view>
				<view class="option2" style="background-color: greenyellow;">
					<image src="/static/index/xiche.png" mode=""></image>
				</view>
				<view class="option2" style="background-color: #87cefa;">
					<image src="/static/index/jiayou.png" mode=""></image>
				</view>
			</view>
			<view class="option-text-area">
				<view class="option-text">
					车辆维修
				</view>
				<view class="option-text">
					汽车保养
				</view>
				<view class="option-text">
					专业洗车
				</view>
				<view class="option-text">
					就近加油
				</view>
			</view>
		</view>
		<view class="map">
			<view class="map-top">
				<view class="map-title-area">
					<view class="map-title">
						附近加油站
					</view>
					<view class="map-title-explain">
						根据您的位置推荐最近的加油站
					</view>
				</view>
				<view class="more-area">
					<view class="more-text">
						更多
					</view>
					<view class="arrow">
					</view>
				</view>
			</view>
			<map 
			:latitude="latitude" 
			:longitude="longitude"
			:scale = 14
			enable-pan
			enable-scroll
			enable-zoom
			:markers="markers"
			>
			</map>
			<button @tap="choose">选择位置</button>
		</view>
		<view class="groupBuy">
			<view class="map-top">
				<view class="map-title-area">
					<view class="map-title">
						优惠团购
					</view>
					<view class="map-title-explain">
						钜惠来袭 为您爱车续航
					</view>
				</view>
				<view class="more-area">
					<view class="more-text">
						更多
					</view>
					<view class="arrow">
					</view>
				</view>
			</view>
			<view class="groupBuy-option-area">
				<view class="groupBuy-option">
					<view>
						<image src="/static/index/薇尔莉特01 .png" mode=""></image>
					</view>
					<view class="groupBuy-option-text">
						汽车惠民走进........
					</view>
				</view>
				<view class="groupBuy-option">
					<view>
						<image src="/static/index/薇尔莉特02.jpeg" mode=""></image>
					</view>
					<view class="groupBuy-option-text">
						汽车惠民走进........
					</view>
				</view>
				<view class="groupBuy-option">
					<view>
						<image src="/static/index/薇尔莉特03.png" mode=""></image>
					</view>
					<view class="groupBuy-option-text">
						汽车惠民走进........
					</view>
				</view>
				<view class="groupBuy-option">
					<view>
						<image src="/static/index/fengjing.jpg" mode=""></image>
					</view>
					<view class="groupBuy-option-text">
						汽车惠民走进........
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script setup>
	import { onMounted, ref } from 'vue';
	const searchValue = ref('');
	const latitude = ref('')
	const longitude = ref('')
	function myLocal(){
		uni.getLocation({
			type: 'wgs84',
			success: function (res) {
				latitude.value = res.latitude
				longitude.value = res.longitude
			}
		});
	}
	// 选择位置
	const choose = ()=>{
	// 在web端使用其他方式获取位置，例如HTML5的Geolocation API 
	// #ifdef H5
	navigator.geolocation.getCurrentPosition(
	position => {
	console.log(position.coords.latitude, position.coords.longitude);
	// 处理获取到的位置信息
	},
	error => {
	console.error(error);
	// 处理错误
	}
	);
	// #endif 
	// #ifndef H5
	uni.chooseLocation({
	success(resp) { console.log(resp); }
	})
	// #endif
	}
	const markers = [
	  {
	    id: 1,  // 唯一标识（必须）
	    latitude: 34.825817,  // 与地图中心纬度一致
	    longitude: 113.55852, // 与地图中心经度一致
	    title: '我的位置',  // 点击时显示的标题
		iconPath: '/static/位置 拷贝 4.png', 
		width: 10, // 图标宽度（单位：px）
		height: 12,  // 图标高度（单位：px）
		callout: {  // 自定义一直显示的数据，自己的定位信息 
			content: "附近门店>", 
			color: "#fff", 
			bgColor: '#000',
			borderRadius: 10,
			padding: 10, 
			display: "ALWAYS", 
		}
	  },
	  {
	    id: 2,
	    latitude: 34.826817,  // 稍偏北的位置
	    longitude: 113.55952,
	    title: '附近的商店',
		iconPath: '/static/位置 拷贝 4.png', 
		width: 10, // 图标宽度（单位：px）
		height: 10,  // 图标高度（单位：px）
	  }
	];
	onMounted(()=>{
		myLocal()
	})
</script>

<style>
	.content {
		padding: 20rpx;
		max-width: 750rpx;
		background-color: #eeeeee;
		position: relative;
		z-index: 1;
	}

	.bg {
		width: 100%;
		height: 200px;
		position: absolute;
		z-index: -1;
		top: 0;
		right: 0;
	}
	.top{
		display: flex;
		align-items: center;
		padding: 20rpx;
		
	}
	.title{
		font-size: 36rpx;
		font-weight: bold;
		color: #FFFFFF;
		margin-left: 270rpx;
		letter-spacing: 10rpx;
	}
	.consult{
		width: 30rpx;
		height: 30rpx;
	}
	/* 搜索框 */
	.search-area{
		display: flex;
		align-items: center;
		justify-content: space-between;
		background-color: #FFFFFF;
		border-radius: 40rpx;
		min-height: 80rpx;
		margin-top: 20rpx;
		padding: 0 20rpx;
	}
	.search-icon{
		width: 30rpx;
		height: 30rpx;
		
	}
	.search-input{
		    flex: 1; /* 改为弹性布局，占据剩余空间 */
		    margin: 0 20rpx; /* 添加左右边距 */
	}
	.search-input input{
		font-size: 24rpx;
		
	}
	.refresh{
		display: flex;
		align-items: center;
	}
	.refresh-text{
		font-size: 24rpx;
		color: #87cefa;
		margin-left: 10rpx;
	}
	/* 选项 */
	.option-area{
		
		padding: 20rpx 0;
		background-color: #FFFFFF;
		margin-top: 30rpx;
		border-radius: 20rpx;
	}
	.option-image{
		display: flex;
		align-items: center;
		justify-content: space-around;
		padding-top: 20rpx;
	}
	.option{
		width: 90rpx;
		height: 90rpx;
		padding: 10rpx;
		border-radius: 30rpx;
	}
	.option2{
		width: 90rpx;
		height: 90rpx;
		padding: 10rpx;
		border-radius: 30rpx;
	}
	.option image{
		margin-top: 20rpx;
		width: 90rpx;
		height: 50rpx;
	}
	.option2 image{
		margin-top: 14rpx;
		width: 60rpx;
		height: 60rpx;
		margin-left: 14rpx;
	}
	.option-text-area{
		display: flex;
		align-items: center;
		justify-content: space-around;
		padding: 20rpx 0;
	}
	.option-text{
		font-size: 24rpx;
	}
	.map{
		margin-top: 20rpx;
		background-color: #FFFFFF;
		border-radius: 20rpx;
		padding: 20rpx;
	}
	.map-top{
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	.map-title-area{
		padding: 10rpx;
	}
	.map-title{
		font-size: 32rpx;
		font-weight: bold;
	}
	.map-title-explain{
		padding: 10rpx 0;
		font-size: 16rpx;
		color: #a9a9a9;
	}
	.more-area{
		display: flex;
		align-items: center;
		font-size: 24rpx;
	}
	.more-text{
		line-height: 10rpx;
	}
	.arrow{
	  width: 12rpx;
	  height: 12rpx;
	  border-right-style: solid;
	  border-right-width: 2rpx;
	  border-top-style: solid;
	  border-top-width: 2rpx;
	  transform: rotate(45deg);
	}
	.map map{
		width: 100%;
		height: 300rpx;
		padding: 10rpx 0;
	}
	.groupBuy{
		margin-top: 20rpx;
		background-color: #FFFFFF;
		border-radius: 20rpx;
		padding: 20rpx;
	}
	.groupBuy-option-area{
		display: flex;
		flex-wrap: wrap;
		margin-left: 20rpx;
		gap: 30rpx;
		font-size: 16rpx;
	}
	.groupBuy-option image{
		width: 300rpx;
		height: 180rpx;
		border-radius: 20rpx;
	}
</style>