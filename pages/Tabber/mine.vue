<template>
	<view class="container">
		<view class="today">
			<view class="top">
				<view class="topleft">
					<view class="date">
						{{date}}
					</view>
					<view class="toptext">
						Today
					</view>
				</view>
				<view class="topright" v-if="logined==false" @click="login">
					Sign in
				</view>
				<view class="topright" v-if="logined==true">
					<image :src="avatarUrl" mode="aspectFit" @click="Settings"></image>
				</view>
			</view>
			<view class="todayview">
				<view class="todayinfo">
					<view class="distanceinfo">
						<view class="distance">
							{{distance}}
						</view>
						<view class="km">
							公里
						</view>
					</view>
					<view class="detail">
						<view class="runtimes">
							<view class="up">
								{{runtimes}}
							</view>
							<view class="down">
								跑步次数
							</view>
						</view>
						<view class="times">
							<view class="up">
								{{time}}
							</view>
							<view class="down">
								时间
							</view>
						</view>
						<view class="speed">
							<view class="up">
								{{speed}}
							</view>
							<view class="down">
								配速（每公里）
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="history">
			<view class="historytext" @tap="isAvailable">
				近期活动
			</view>
			<view class="historyinfo" v-for="(item,index) in RunHistory">
				<view class="historyinfodetail" @click="history(index)">
					<view class="historytop">
						<view class="logo">
							<image :src="item.imgsrc" mode="aspectFit"></image>
						</view>
						<view class="historytop-right">
							<view class="historytop-right-top">
								{{item.time}}
							</view>
							<view class="historytop-right-bottom">
								<view class="historytop-right-bottom-week">
									{{item.week}}
								</view>
								<view class="historytop-right-bottom-runtimedetail">
									{{item.runtimedetail}}
								</view>
							</view>
						</view>
					</view>
					<view class="historybottom">
						<view class="historydistance">
							<view class="up">
								{{item.historydistance}}
							</view>
							<view class="down">
								公里
							</view>
						</view>
						<view class="totaltime">
							<view class="up">
								{{item.totaltime}}
							</view>
							<view class="down">
								时间
							</view>
						</view>
						<view class="historyspeed">
							<view class="up">
								{{item.historyspeed}}
							</view>
							<view class="down">
								配速
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="bottom"></view>
	</view>
</template>

<script>
	import currentDate from "./gettime.js";
	const pedometer = uni.requireNativePlugin('DC-Pedometer');
	export default {
		// 进页面获取系统时间及用户跑步信息
		onShow: function() {
			let that = this;
			// console.log(pedometer.isAvailable());
			that.distance = (getApp().globalData.historyPathDistance / 1000).toFixed(3);
			that.date = currentDate.getDate();
			if (getApp().globalData.qq.logined) {
				that.logined = getApp().globalData.qq.logined;
				that.avatarUrl = getApp().globalData.qq.avatarUrl;
				for (var i = 0; i < getApp().globalData.History.length; i++) {
					if (getApp().globalData.qq.openId == getApp().globalData.History[i].openId) {
						that.RunHistory = getApp().globalData.History[i].RunHistory;
					}
				}
			} else if (getApp().globalData.wechat.logined) {
				that.logined = getApp().globalData.wechat.logined;
				that.avatarUrl = getApp().globalData.wechat.avatarUrl;
			} else if (getApp().globalData.weibo.logined) {
				that.logined = getApp().globalData.weibo.logined;
				that.avatarUrl = getApp().globalData.weibo.avatarUrl;
			}
		},
		onPullDownRefresh: function() {
			let that = this;
			setTimeout(function() {
				console.log(currentDate.getDate());
				that.date = currentDate.getDate();
			}, 1000);
			uni.stopPullDownRefresh();
		},
		data() {
			return {
				//是否登录
				logined: false,
				// 用户头像
				avatarUrl: "",

				date: "",
				runtimes: 2,
				distance: "",
				time: "4:21:43",
				speed: "4分20秒",

				// 跑步历史
				RunHistory: ""
			}
		},
		methods: {
			// 跳转登录界面
			login() {
				uni.navigateTo({
					url: "../Functions/login"
				});
			},
			// 跳转设置界面
			Settings() {
				uni.navigateTo({
					url: "../Functions/settings"
				});
			},
			// 跳转历史轨迹界面
			history(index) {
				console.log(index);
				// 检查支持的认证方式
				uni.checkIsSupportSoterAuthentication({
					success(res) {
						console.log(res.supportMode[0]);
						// 支持Face ID
						if (res.supportMode[0] == "facial") {
							// 检查是否录入Face ID
							uni.checkIsSoterEnrolledInDevice({
								checkAuthMode: 'facial',
								success(res) {
									if (res.isEnrolled == true) {
										// 进行Face ID认证
										uni.startSoterAuthentication({
											requestAuthModes: ['facial'],
											challenge: '123456',
											authContent: '请用FaceID解锁',
											success(res) {
												console.log(res);
												// uni.navigateTo({
												// 	url: "../Functions/historypath"
												// })
											},
											fail(err) {
												console.log(err);
											},
											complete(res) {
												console.log(res);
											}
										})
									}
								},
							})
						} else if (res.supportMode[0] == "fingerPrint") {
							// 检查是否录入指纹
							uni.checkIsSoterEnrolledInDevice({
								checkAuthMode: 'fingerPrint',
								success(res) {
									// 进行Touch ID认证
									uni.startSoterAuthentication({
										requestAuthModes: ['fingerPrint'],
										challenge: '123456',
										authContent: '请用指纹解锁',
										success(res) {
											console.log(res);
										},
										fail(err) {
											console.log(err);
										},
										complete(res) {
											console.log(res);
										}
									})
								},
							})
						}
					}
				})
			},
			// 是否支持计步器
			isAvailable() {
				var avaliable = pedometer.isAvailable();
				console.log(avaliable);
			},
			// 开始计步
			startPedometer() {
				pedometer.startPedometer(ret => {
					this.tranMsg = JSON.stringify(ret);
				});
			},
			// 暂停计步
			pausePedometer() {
				pedometer.pausePedometer();
			},
			// 停止计步
			stopPedometer() {
				pedometer.stopPedometer();
			},
			// 查询 2019-01-20 日数据
			queryPedometerData() {
				pedometer.queryPedometerData({
					start: "2019-01-20 00:00:00",
					end: "2019-01-21 00:00:00"
				}, ret => {
					this.tranMsg = JSON.stringify(ret);
				});
			}
		}
	}
</script>

<style lang="scss">
	page {
		height: 100%;
		background-color: #f5f5f5;
	}

	.container {
		// border: 1px solid red;
		width: 750rpx;
		background-color: #f5f5f5;
		// iOS安全区域设置
		padding-bottom: constant(safe-area-inset-bottom);
		padding-bottom: env(safe-area-inset-bottom);

		.today {
			// border: 1px solid green;
			width: 750rpx;
			height: 450rpx;
			background-color: #fff;

			.top {
				// border: 1px solid red;
				width: 96%;
				height: 100rpx;
				display: flex;
				flex-direction: row;
				justify-content: space-between;
				align-items: center;

				.topleft {
					// border: 1px solid red;
					width: 220rpx;
					height: 100%;
					display: flex;
					flex-direction: column;

					.date {
						color: #888;
						font-size: 25rpx;
						font-weight: bold;
						margin-left: 20rpx;
					}

					.toptext {
						color: #000;
						font-size: 50rpx;
						font-weight: 900;
						margin-left: 20rpx;
					}
				}

				.topright {
					// border: 1px solid red;
					width: 130rpx;
					height: 70%;
					display: flex;
					align-self: flex-end;
					align-items: center;
					justify-content: center;
					font-size: 30rpx;
					font-weight: bold;
					color: #808080;

					image {
						width: 65rpx;
						height: 65rpx;
						border-radius: 50%;
					}
				}
			}

			.todayview {
				// border: 1px solid blue;
				width: 750rpx;
				height: 300rpx;
				margin-top: 20rpx;

				.todayinfo {
					// border: 1px solid red;
					width: 70%;
					height: 100%;
					display: flex;
					flex-direction: column;
					justify-content: space-between;
					align-items: flex-start;
					margin-left: 40rpx;

					.distanceinfo {
						// border: 1px solid red;

						.distance {
							font-size: 100rpx;
							font-weight: bolder;
						}

						.km {
							font-size: 28rpx;
							color: #888;
						}
					}

					.detail {
						// border: 1px solid red;
						width: 500rpx;
						display: flex;
						flex-direction: row;
						align-items: center;
						justify-content: space-between;

						.speed .times .runtimes {
							// border: 1px solid red;
							display: flex;
							flex-direction: column;
							justify-content: center;
							align-items: center;
						}

						.up {
							font-size: 32rpx;
							font-weight: bold;
						}

						.down {
							font-size: 28rpx;
							color: #888;
						}
					}
				}
			}
		}

		.history {
			margin-top: 40rpx;

			.historytext {
				// border: 1px solid red;
				display: flex;
				justify-content: flex-start;
				color: #111;
				font-size: 40rpx;
				font-weight: bold;
				margin-left: 50rpx;
				margin-bottom: 20rpx;
			}

			.historyinfo {
				// border: 1px solid red;
				width: 750rpx;
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;

				.historyinfodetail {
					// border: 1px solid red;
					width: 650rpx;
					height: 300rpx;
					margin-bottom: 30rpx;
					background-color: #fff;
					border-radius: 20rpx;
					padding: 10rpx 0;
					display: flex;
					flex-direction: column;
					align-items: center;
					justify-content: center;

					.historytop {
						// border: 1px solid red;
						width: 90%;
						height: 50%;
						display: flex;
						flex-direction: row;
						justify-content: flex-start;
						align-items: center;

						.logo {
							// border: 1px solid red;

							image {
								width: 100rpx;
								height: 100rpx;
							}
						}

						.historytop-right {
							// border: 1px solid blue;
							margin-left: 20rpx;
							display: flex;
							flex-direction: column;
							justify-content: center;
							align-items: flex-start;

							.historytop-right-top {
								// border: 1px solid red;
								font-weight: bold;
							}

							.historytop-right-bottom {
								// border: 1px solid red;
								display: flex;
								flex-direction: row;
								justify-content: flex-start;
								align-items: center;

								.historytop-right-bottom-week {
									font-size: 28rpx;
									color: #888;
								}

								.historytop-right-bottom-runtimedetail {
									background-color: #f9ca24;
									border-radius: 10rpx;
									font-size: 20rpx;
									padding: 2rpx 8rpx;
									margin-left: 10rpx;
								}
							}
						}
					}

					.historybottom {
						// border: 1px solid red;
						width: 90%;
						height: 50%;
						display: flex;
						flex-direction: row;
						justify-content: space-around;
						align-items: center;
						border-top: 1px solid #eaeaea;

						.historydistance,
						.totaltime,
						.historyspeed {
							// border: 1px solid red;
							display: flex;
							flex-direction: column;
							justify-content: center;
							align-items: flex-start;
						}

						.up {
							font-size: 30rpx;
							font-weight: bold;
						}

						.down {
							font-size: 26rpx;
							color: #888;
						}
					}
				}
			}
		}

		.bottom {
			width: 750rpx;
			height: $tabbar-height;
		}
	}
</style>
