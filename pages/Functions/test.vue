<template>
	<view class="content">
		<!-- 在map标签绑定经纬度和标记点数组，以及连线数组 -->
		<map id="maps" :latitude="latitude" :longitude="longitude" :markers="markers" :polyline="polylines" scale="10"></map>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				latitude: '', //纬度
				longitude: '', //经度
				markers: [], //标记点数组
				polylines: [] //连续数组
			}
		},
		onReady() {
			//调用的函数放到onReady里面
			this.getlocal();
			this.test();
		},
		methods: {
			//地图标记点;   具体的属性可以看看官网的说明
			test() {
				var that = this;
				//地图标记点连线静态数据赋值;
				// 可以理解成下面polylines里面的一个对象为一条线，每一条线都是独立的，可以设置不同的连续颜色
				//points[]里面是从第一个对象连到最后一个对象的经纬度成一条线
				that.polylines = [{
					points: [{
							latitude: 23.043594,
							longitude: 113.160704
						},
						{
							latitude: 23.036344,
							longitude: 113.160704
						},
						{
							latitude: 23.03528,
							longitude: 113.149565
						},
						{
							latitude: 23.03794,
							longitude: 113.138498
						},
						{
							latitude: 23.049247,
							longitude: 113.145684
						}
					],
					arrowLine: true,
					dottedLine: true,
					arrowIconPath: "/static/images/2btn_loc/btn_loc3.png",
					width: 2,
					color: "FEAA34"
				}];
				//地图标记点静态数据赋值;
				//标记点和连线的意思差不多，标记点的数组是单纯将markers数组中的对象的经纬度通过图片的方式标记出来（图片可以自定义）
				that.markers = [{
						title: '123',
						latitude: 23.043594,
						longitude: 113.160704,
						iconPath: '/static/btn_loc0.png'
					},
					{
						title: '123',
						latitude: 23.036344,
						longitude: 113.160704,
						iconPath: '/static/btn_loc0.png'
					},
					{
						title: '123',
						latitude: 23.03528,
						longitude: 113.149565,
						iconPath: '/static/btn_loc0.png'
					},
					{
						title: '123',
						latitude: 23.03794,
						longitude: 113.138498,
						iconPath: '/static/btn_loc0.png'
					},
					{
						title: '123',
						latitude: 23.049247,
						longitude: 113.145684,
						iconPath: '/static/btn_loc0.png'
					},
				];
			},
			//定位方法;获取当前的经纬度，也可以通过经纬度来获取当前的地理位置，比如：xx省、xx市、xx镇
			getlocal: function() {
				let _this = this;
				//显示当前位置
				var map = uni.createMapContext('maps', this).$getAppMap();
				map.showUserLocation(true);
				//获取当前定位
				uni.getLocation({
					type: 'wgs84',
					success: function(res) {
						console.log('当前位置的经度：' + res.longitude);
						console.log('当前位置的纬度：' + res.latitude);
						_this.longitude = res.longitude;
						_this.latitude = res.latitude;
					}
				})
			},
		}
	}
</script>

<style scoped lang="scss">
	.content {
		position: absolute;
		width: 100%;
		height: 100%;

		#maps {
			width: 100%;
			height: 100%;
		}
	}
</style>
