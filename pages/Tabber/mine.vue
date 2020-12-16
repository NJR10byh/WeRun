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
				<view class="topright">
					<image src="../../static/under-armour.png" mode="aspectFit" @click="Settings"></image>
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
			<view class="historytext">
				近期活动
			</view>
			<view class="historyinfo" v-for="item in RunHistory">
				<view class="historyinfodetail">
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
								配速（每公里）
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import currentDate from "./gettime.js";
	export default {
		// 进页面获取系统时间
		onShow: function() {
			currentDate.getDate();
			// console.log(currentDate.getDate());
			this.date = currentDate.getDate();
		},
		data() {
			return {
				date: "",
				runtimes: 2,
				distance: 8.5,
				time: "4:21:43",
				speed: "4分20秒",

				// 跑步历史
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
			}
		},
		methods: {
			Settings() {
				uni.navigateTo({
					url: "../Functions/settings"
				})
			}
		}
	}
</script>

<style lang="scss">
	.container {
		// border: 1px solid red;
		width: 750rpx;
		background-color: #f5f5f5;

		.today {
			// border: 1px solid green;
			width: 750rpx;
			height: 450rpx;
			background-color: #fff;

			.top {
				// border: 1px solid red;
				width: 96%;
				height: 100rpx;
				margin-top: 20rpx;
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
						color: back;
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
							border: 1px solid red;
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
							width: 200rpx;
							height: 100rpx;
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
						border-top: 1px solid #dcdde1;

						.historydistance .totaltime .historyspeed {
							display: flex;
							flex-direction: column;
							justify-content: center;
							align-items: center;
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
	}
</style>
