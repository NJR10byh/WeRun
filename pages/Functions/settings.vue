<template>
	<view class="container">
		<view class="top">
			<view class="text">
				<text>账户</text>
			</view>
		</view>
		<view class="part1">
			<view class="part1left" @click="Changephoto">
				<image src="../../static/under-armour.png" mode="aspectFit"></image>
			</view>
			<view class="part1right">
				<view class="username">
					NJR10byh Tutu
				</view>
				<view class="userinfo">
					<view class="userlevel" @click="Level">
						Lv98
					</view>
					<!-- <view class="uservip">
						vip
					</view> -->
					<view class="usersvip" @click="Svip">
						SVIP
					</view>
				</view>
			</view>
		</view>
		<view class="part2">
			<view class="index" v-for="item in functions">
				<view class="indexleft">
					{{item.left}}
				</view>
				<view class="indexgo">
					<image src="../../static/img/go-right.png" mode="aspectFit"></image>
				</view>
			</view>
		</view>
		<view class="part3">
			<view class="index">
				<view class="indexleft">
					设置
				</view>
				<view class="indexgo">
					<image src="../../static/img/go-right.png" mode="aspectFit"></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				functions: [{
						left: "修改昵称"
					},
					{
						left: "订单"
					},
					{
						left: "服务"
					}
				]
			}
		},
		methods: {
			Changephoto() {
				console.log("photo");
			},
			Level() {
				console.log("aaa")
			},
			Svip() {
				console.log("bbb");
			}
		}
	}
</script>

<style lang="scss">
	page {
		height: 100%;
	}

	.container {
		// border: 1px solid red;
		width: 750rpx;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: flex-start;
		align-items: center;
		background-color: #f3f3f3;

		.top {
			// border: 1px solid red;
			width: 660rpx;
			margin-top: 30rpx;

			text {
				font-size: 30px;
				font-weight: bold;
			}
		}

		.part1 {
			// border: 1px solid red;
			width: 750rpx;
			height: 150rpx;
			margin-top: 40rpx;
			background-color: #fff;
			display: flex;
			flex-direction: row;
			justify-content: flex-start;
			align-items: center;

			.part1left {
				// border: 1px solid red;
				border-radius: 50%;
				width: 100rpx;
				height: 100rpx;
				margin-left: 50rpx;
				display: flex;
				align-items: center;
				justify-content: center;

				image {
					width: 100rpx;
					height: 100rpx;
				}
			}

			.part1right {
				// border: 1px solid red;
				width: 400rpx;
				height: 100rpx;
				margin-left: 30rpx;
				display: flex;
				flex-direction: column;
				align-items: flex-start;
				justify-content: space-around;

				.username {
					// border: 1px solid red;
					font-size: 18px;
					font-weight: bold;
				}

				.userinfo {
					// border: 1px solid red;
					display: flex;
					justify-content: center;
					align-items: center;

					.userlevel {
						// border: 1px solid red;
						font-size: 13px;
						font-weight: bold;
						color: $theme-color;
						background-color: #f0f0f0;
						padding: 2rpx 10rpx;
						border-radius: 10rpx;
					}

					.usersvip {
						// border: 1px solid red;
						font-size: 12px;
						font-weight: bold;
						color: #d63031;
						background-color: #ffd32a;
						padding: 2rpx 15rpx;
						border-radius: 10rpx;
						margin-left: 10rpx;
					}
				}
			}
		}

		.part2,.part3 {
			// border: 1px solid red;
			width: 750rpx;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: flex-end;
			margin-top: 20rpx;
			background-color: #fff;

			.index {
				// border: 1px solid red;
				width: 700rpx;
				height: 100rpx;
				display: flex;
				flex-direction: row;
				justify-content: space-between;
				align-items: center;
				border-top: 0.5px solid #e6e6e6;
				border-bottom: 0.5px solid #e6e6e6;

				.indexleft {
					font-weight: bold;
				}

				.indexgo {
					image {
						width: 30rpx;
						height: 30rpx;
						margin-right: 20rpx;
					}
				}
			}
		}
	}
</style>
