<template>
	<view class="container">
		<view class="mapview">
			<map class="map" :latitude="latitude" :longitude="longitude" :markers="covers" :scale="scale" :polyline="polyline"></map>
		</view>
		<view class="bottompart1" v-if="startrun===true">
			<button type="default" @click="StartRun">
				<text>Start</text>
			</button>
		</view>
		<view class="bottompart2" v-if="pauserun===true">
			<image src="../../static/img/pause.png" mode="aspectFit" @click="Pause"></image>
		</view>
		<view class="bottompart3" v-if="continuerun===true">
			<view class="bottompart3-right">
				<image class="continueimg" src="../../static/img/continue.png" mode="aspectFit" @click="Continue"></image>
				<image class="stopimg" src="../../static/img/stop.png" mode="aspectFit" @click="Stop"></image>
			</view>
		</view>
		<!-- <uni-fab :pattern="pattern" direction="vertical"></uni-fab> -->
	</view>
</template>

<script>
	import uniFab from '@/components/uni-fab/uni-fab.vue';
	var timer = null; // 进入页面获取位置的定时器
	var startrun = null // 开始跑步的定时器
	export default {
		onShow: function() {
			let that = this;
			timer = setInterval(function() {
				uni.getLocation({
					type: 'gcj02',
					success: function(res) {
						that.latitude = res.latitude;
						that.longitude = res.longitude;
						that.covers[0].latitude = res.latitude;
						that.covers[0].longitude = res.longitude;
						console.log(that.latitude, that.longitude);
					}
				});
			}, 2000);
		},
		onHide: function() {
			clearInterval(timer);
		},
		data() {
			return {
				startrun: true,
				pauserun: false,
				continuerun: false,

				// Map
				id: 0, // 使用 marker点击事件 需要填写id
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
					// points: [{
					// 		"latitude": 32.112895,
					// 		"longitude": 118.931422,
					// 	}, {
					// 		"latitude": 32.112195,
					// 		"longitude": 118.931922,
					// 	},
					// 	{
					// 		"latitude": 32.112295,
					// 		"longitude": 118.931522,
					// 	},
					// 	{
					// 		"latitude": 32.112595,
					// 		"longitude": 118.931922,
					// 	}
					// ],
					points: [],
					arrowLine: true, // 带箭头的线
					dottedLine: false, // 是否虚线
					color: "#feca57", // 线的颜色
					width: 2, // 线的宽度
					borderColor: "#2ecc71", // 线的边框颜色
					borderWidth: 1 // 线的厚度
				}],

				// HistoryPath
				historypath: [],

				// Fab
				pattern: {
					"color": "#3c3e49",
					"selectedColor": "#940f8a",
					"buttonColor": "#940f8a"
				},
			}
		},
		methods: {
			StartRun() {
				let that = this;
				this.startrun = false;
				this.pauserun = true;
				clearInterval(timer);
				startrun = setInterval(function() {
					uni.getLocation({
						type: 'gcj02',
						success: function(res) {
							that.covers[0].latitude = res.latitude;
							that.covers[0].longitude = res.longitude;
							that.latitude = res.latitude;
							that.longitude = res.longitude;
							let newpoint = {
								latitude: res.latitude,
								longitude: res.longitude
							};
							that.polyline[0].points.push(newpoint);
							console.log(that.polyline[0].points);
						}
					});
				}, 2000);
			},
			Pause() {
				this.pauserun = false;
				this.continuerun = true;
				clearInterval(startrun);
			},
			Continue() {
				let that = this;
				this.continuerun = false;
				this.pauserun = true;
				startrun = setInterval(function() {
					uni.getLocation({
						type: 'gcj02',
						success: function(res) {
							that.covers[0].latitude = res.latitude;
							that.covers[0].longitude = res.longitude;
							that.latitude = res.latitude;
							that.longitude = res.longitude;
							let newpoint = {
								latitude: res.latitude,
								longitude: res.longitude
							};
							that.polyline[0].points.push(newpoint);
							console.log(that.polyline[0].points);
						}
					});
				}, 2000);
			},
			Stop() {
				let that = this;
				this.pauserun = false;
				this.startrun = true;
				clearInterval(startrun);
				//console.log(that.polyline[0].points);
				//that.historypath.push(that.polyline[0].points);
				//console.log(that.historypath[0]);
				getApp().globalData.historyPath.push(that.polyline[0].points);
				console.log(getApp().globalData);
			}
		},
		components: {
			uniFab
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
			height: 80%;

			.map {
				width: 750rpx;
				height: 100%;
			}
		}

		.bottompart1 {
			width: 750rpx;
			height: 20%;
			display: flex;
			align-items: center;
			justify-content: center;

			button {
				width: 200rpx;
				height: 200rpx;
				background-color: #f1c40f;
				border-radius: 50%;
				display: flex;
				align-items: center;
				justify-content: center;

				text {
					font-weight: bold;
					font-size: 50rpx;
				}
			}
		}

		.bottompart2 {
			width: 750rpx;
			height: 20%;
			display: flex;
			align-items: center;
			justify-content: center;

			image {
				width: 130rpx;
				height: 130rpx;
			}
		}

		.bottompart3 {
			// border: 1px solid red;
			width: 750rpx;
			height: 20%;
			display: flex;
			align-items: center;
			justify-content: flex-end;

			.bottompart3-right {
				// border: 1px solid red;
				width: 57%;
				height: 100%;
				display: flex;
				flex-direction: row;
				align-items: center;
				justify-content: flex-start;

				.continueimg {
					width: 140rpx;
					height: 140rpx;
				}

				.stopimg {
					width: 80rpx;
					height: 80rpx;
					margin-left: 80rpx;
				}
			}
		}
	}
</style>
