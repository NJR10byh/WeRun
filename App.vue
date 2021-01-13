<script>
	export default {
		onLaunch: function() {
			let that = this;
			// 锁定屏幕竖直方向
			plus.screen.lockOrientation("portrait-primary");
			// console.log('App Launch');
			uni.getSystemInfo({
				success: (res) => {
					console.log(res);
					that.globalData.Systeminfo.systemwidth = res.safeArea.width;
					that.globalData.Systeminfo.systemtop = res.safeArea.top;
					// console.log(that.globalData.Systeminfo);
				}
			})
		},
		onShow: function() {
			// console.log('App Show')
		},
		onHide: function() {
			// console.log('App Hide')
		},
		// 全局变量
		globalData: {
			// 当前位置
			latitude: "",
			longitude: "",
			cityname: "",
			// 行政区划代码
			adcode: "",
			// 历史轨迹
			historyPath: "",
			// 历史轨迹距离
			historyPathDistance: 0,
			// 运动历史
			History: [{
					openId: "49D3A04415C4F944149ED16F8422B5D8",
					RunHistory: [{
							"time": "2020-10-20",
							"week": "星期二",
							"runtimedetail": "早晨跑",
							"historydistance": 4.3,
							"totaltime": "2:13:25",
							"historyspeed": "3分36秒",
							"imgsrc": "../../static/nike.png"
						},
						{
							"time": "2020-10-22",
							"week": "星期四",
							"runtimedetail": "晚间跑",
							"historydistance": 7.5,
							"totaltime": "4:12:56",
							"historyspeed": "5分17秒",
							"imgsrc": "../../static/under-armour.png"
						},
						{
							"time": "2020-12-06",
							"week": "星期日",
							"runtimedetail": "中午跑",
							"historydistance": 2.8,
							"totaltime": "1:32:28",
							"historyspeed": "2分21秒",
							"imgsrc": "../../static/puma.png"
						}
					]
				},
				{
					openId: "49D3A04415C4F944149ED16F8422B5D9",
					RunHistory: [{
							"time": "2020-10-21",
							"week": "星期二",
							"runtimedetail": "早晨跑",
							"historydistance": 4.3,
							"totaltime": "2:13:25",
							"historyspeed": "3分36秒",
							"imgsrc": "../../static/nike.png"
						},
						{
							"time": "2020-10-22",
							"week": "星期四",
							"runtimedetail": "晚间跑",
							"historydistance": 7.5,
							"totaltime": "4:12:56",
							"historyspeed": "5分17秒",
							"imgsrc": "../../static/under-armour.png"
						},
						{
							"time": "2020-12-06",
							"week": "星期日",
							"runtimedetail": "中午跑",
							"historydistance": 2.8,
							"totaltime": "1:32:28",
							"historyspeed": "2分21秒",
							"imgsrc": "../../static/puma.png"
						}
					]
				}
			],
			// 获取用户设备信息
			Systeminfo: {
				systemwidth: "",
				systemtop: "",
				systemheight: "",
			},
			// qqinfo
			qq: {
				logined: "",
				nickName: "",
				openId: "",
				avatarUrl: ""
			},
			// weiboinfo
			weibo: {
				logined: "",
				nickName: "",
				openId: "",
				avatarUrl: ""
			},
			// wechatinfo
			wechat: {
				logined: "",
				nickName: "",
				openId: "",
				avatarUrl: ""
			},

		}
	}
</script>

<style lang="scss">
	/*每个页面公共css */
</style>
