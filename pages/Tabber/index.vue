<template>
	<view class="container">
		<view class="mapview">
			<map class="map" :latitude="latitude" :longitude="longitude" :markers="covers" :scale="scale" :polyline="polyline"
			 :controls="controls" @controltap="StartRun"></map>
		</view>
	</view>
</template>

<script>
	var timer = null; // 进入页面获取位置的定时器
	var startrun = null // 开始跑步的定时器
	var run = null // 开始跑步的定时器
	var i = 0;
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
						getApp().globalData.latitude = res.latitude;
						getApp().globalData.longitude = res.longitude;
						console.log(that.latitude, that.longitude);
					}
				});
			}, 2000);
			uni.getSystemInfo({
				success: (res) => {
					// console.log(res);
					getApp().globalData.Systeminfo.systemheight = res.safeArea.height - 50;
					this.controls[0].position.left = res.safeArea.width / 2 - 40;
					this.controls[0].position.top = res.safeArea.height - 125;
					// console.log(this.controls[0].position);
				}
			});
		},
		onHide: function() {
			clearInterval(timer);
		},
		data() {
			return {
				// Map
				id: 0, // 使用 marker点击事件 需要填写id
				title: 'map',
				scale: 15,
				latitude: 32.112895,
				longitude: 118.931422,
				controls: [{
					id: 1,
					position: {
						left: "",
						top: "",
						width: 80,
						height: 80
					},
					iconPath: "../../static/img/startrun.png",
					clickable: true,
				}],
				covers: [{
					latitude: "",
					longitude: "",
					iconPath: "../../static/img/MyLocation.png",
					width: 10,
					height: 10
				}],
				polyline: [{
					// 经纬度数组
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
				let left = getApp().globalData.Systeminfo.systemwidth / 2 - 90;
				let top = getApp().globalData.Systeminfo.systemtop + 100;
				let stopleft = getApp().globalData.Systeminfo.systemwidth / 2 - 40;
				let stopheight = getApp().globalData.Systeminfo.systemheight - 100;
				// console.log(that.controls[0].iconPath);
				clearInterval(timer);
				if (that.controls[0].iconPath == "../../static/img/startrun.png") {
					let newcon1 = {
						id: 2,
						position: {
							left: left,
							top: top,
							width: 180,
							height: 180
						},
						iconPath: "../../static/img/3.png",
						backgroundColor: "transparent",
						clickable: true,
					};
					let newcon2 = {
						id: 3,
						position: {
							left: left,
							top: top,
							width: 180,
							height: 180
						},
						iconPath: "../../static/img/2.png",
						backgroundColor: "transparent",
						clickable: true,
					};
					let newcon3 = {
						id: 4,
						position: {
							left: left,
							top: top,
							width: 180,
							height: 180
						},
						iconPath: "../../static/img/1.png",
						backgroundColor: "transparent",
						clickable: true,
					};
					let stop = {
						id: 4,
						position: {
							left: stopleft,
							top: stopheight,
							width: 80,
							height: 80
						},
						iconPath: "../../static/img/stop.png",
						backgroundColor: "transparent",
						clickable: true,
					};
					setTimeout(function() {
						that.controls.push(newcon1);
						setTimeout(function() {
							that.controls.pop();
							that.controls.push(newcon2);
							setTimeout(function() {
								that.controls.pop();
								that.controls.push(newcon3);
								setTimeout(function() {
									that.controls.pop();
									that.controls.pop();
									that.controls.push(stop);
								}, 1000);
							}, 1000);
						}, 1000);
					}, 1000);
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
							}
						});
					}, 2000);
				} else if (that.controls[0].iconPath == "../../static/img/stop.png") {
					let that = this;
					let stop = {
						id: 4,
						position: {
							left: stopleft,
							top: stopheight,
							width: 80,
							height: 80
						},
						iconPath: "../../static/img/startrun.png",
						backgroundColor: "transparent",
						clickable: true,
					};
					that.controls.pop();
					that.controls.push(stop);
					clearInterval(startrun);
					uni.showModal({
						title: "提示",
						content: "是否保存此次运动记录",
						showCancel: true,
						cancelText: "下次一定！",
						confirmText: "存它！",
						success: (res) => {
							if (res.confirm) {
								var i = 0;
								// console.log('用户点击确定');
								getApp().globalData.historyPath = that.polyline[0].points;
								console.log(getApp().globalData.historyPath);
								that.Distance();
							} else if (res.cancel) {
								console.log('用户点击取消');
								return false;
							}
						}
					});
				}
			},
			Distance() {
				let that = this;
				uni.request({
					url: "http://api.map.baidu.com/routematrix/v2/driving?output=json&origins=" + getApp().globalData.historyPath[i]
						.latitude + "," + getApp().globalData.historyPath[i].longitude +
						"&destinations=" + getApp().globalData.historyPath[i + 1].latitude + "," + getApp().globalData.historyPath[
							i + 1].longitude +
						"&ak=4DXL5GtYfjanZxjheF4EkuhBejrydkG6",
					method: "GET",
					success: (res) => {
						console.log(i, res.data.result[0].distance.value);
						getApp().globalData.historyPathDistance += res.data.result[0].distance.value;
						if (++i < getApp().globalData.historyPath.length - 1) {
							that.Distance();
						}
						console.log(getApp().globalData.historyPathDistance);
						return;
					}
				})
			}
		},
		components: {
			// uniFab
		}
	}
</script>

<style lang="scss">
	page {
		height: 100%;
	}

	.container {
		position: relative;
		display: flex;
		justify-content: center;
		align-items: center;
		width: 750rpx;
		height: 100%;
		// iOS安全区域设置
		padding-bottom: constant(safe-area-inset-bottom);
		padding-bottom: env(safe-area-inset-bottom);

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
