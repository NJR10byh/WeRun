<template>
	<view class="container">
		<view class="mapview">
			<map class="map" :latitude="latitude" :longitude="longitude" :markers="covers" :scale="scale" :polyline="polyline"></map>
		</view>
	</view>
</template>

<script>
	export default {
		onShow: function() {
			this.polyline[0].points = getApp().globalData.historyPath;
			this.latitude = getApp().globalData.historyPath[parseInt(getApp().globalData.historyPath.length / 2)].latitude;
			this.longitude = getApp().globalData.historyPath[parseInt(getApp().globalData.historyPath.length / 2)].longitude;
		},
		data() {
			return {
				title: 'map',
				scale: 15,
				latitude: 32.112895,
				longitude: 118.931422,
				covers: [{
					latitude: "",
					longitude: "",
					iconPath: "../../static/img/MyLocation.png",
					width: 10,
					height: 10
				}],
				polyline: [{
					// 经纬度数组
					points: "",
					arrowLine: true, // 带箭头的线
					dottedLine: false, // 是否虚线
					color: "#feca57", // 线的颜色
					width: 2, // 线的宽度
					borderColor: "#2ecc71", // 线的边框颜色
					borderWidth: 1 // 线的厚度
				}],
			}
		},
		methods: {

		}
	}
</script>

<style lang="scss">
	page {
		height: 100%;
	}

	.container {
		width: 750rpx;
		height: 100%;

		.mapview {
			width: 750rpx;
			height: 100%;

			.map {
				width: 750rpx;
				height: 100%;
			}
		}
	}
</style>
