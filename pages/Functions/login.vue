<template>
	<view class="container">
		<view class="Box">
			<view class="logo">
				<image src="../../static/Logo-WeRun.png" mode="aspectFit"></image>
			</view>
			<view class="login">
				<view class="username">
					<!-- <text class="text">用户名: </text> -->
					<input type="text" value="" placeholder="请输入用户名" placeholder-style="color:#bdc3c7;text-align:center;" />
				</view>
				<view class="password">
					<!-- <text class="text">密码: </text> -->
					<input type="password" placeholder="请输入密码" placeholder-style="color:#bdc3c7;text-align:center;" />
				</view>
			</view>
			<view class="loginbutton">
				<image src="../../static/img/login.png" mode="aspectFit"></image>
			</view>
			<view class="register">
				<text>还没有账号？点击注册</text>
			</view>
			<view class="OthorLogin">
				<view class="top">
					------ 其他登录方式 ------
				</view>
				<view class="method">
					<image src="../../static/img/icon-qq.png" mode="aspectFit" @click="qqlogin"></image>
					<image src="../../static/img/icon-wechat.png" mode="aspectFit" @click="wechatlogin"></image>
					<image src="../../static/img/icon-weibo.png" mode="aspectFit" @click="weibologin"></image>
					<image src="../../static/img/icon-apple.png" mode="aspectFit"></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {
			qqlogin() {
				uni.login({
					provider: "qq",
					success: (res) => {
						console.log(res);
						uni.getUserInfo({
							provider: "qq",
							success: (res) => {
								console.log(res);
								getApp().globalData.qq.logined = true;
								getApp().globalData.wechat.logined = false;
								getApp().globalData.weibo.logined = false;
								getApp().globalData.qq.nickName = res.userInfo.nickName;
								getApp().globalData.qq.openId = res.userInfo.openid;
								getApp().globalData.qq.avatarUrl = res.userInfo.avatarUrl;
								uni.switchTab({
									url: "../Tabber/mine"
								});
							}
						})
					}
				})
			},
			wechatlogin() {
				uni.login({
					provider: "weixin",
					success: (res) => {
						console.log(res);
						uni.getUserInfo({
							provider: "weixin",
							success: (res) => {
								console.log(res);
								getApp().globalData.qq.logined = false;
								getApp().globalData.wechat.logined = true;
								getApp().globalData.weibo.logined = false;
								getApp().globalData.wechat.nickName = res.userInfo.nickName;
								getApp().globalData.wechat.openId = res.userInfo.openid;
								getApp().globalData.wechat.avatarUrl = res.userInfo.avatarUrl;
								uni.switchTab({
									url: "../Tabber/mine"
								});
							}
						})
					}
				})
			},
			weibologin() {
				uni.login({
					provider: "sinaweibo",
					success: (res) => {
						console.log(res);
						uni.getUserInfo({
							provider: "sinaweibo",
							success: (res) => {
								console.log(res);
								getApp().globalData.qq.logined = false;
								getApp().globalData.wechat.logined = false;
								getApp().globalData.weibo.logined = true;
								getApp().globalData.weibo.nickName = res.userInfo.nickName;
								getApp().globalData.weibo.openId = res.userInfo.openid;
								getApp().globalData.weibo.avatarUrl = res.userInfo.avatarUrl;
								uni.switchTab({
									url: "../Tabber/mine"
								});
							}
						})
					}
				})
			},
			// Apple登录暂不开放
			// applelogin() {
			// 	uni.login({
			// 		provider: "apple",
			// 		success: (res) => {
			// 			console.log(res);
			// 			uni.getUserInfo({
			// 				provider: "apple",
			// 				success: (res) => {
			// 					console.log(res)
			// 				}
			// 			})
			// 		}
			// 	})
			// },
		},
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
		align-items: center;
		justify-content: center;

		.Box {
			// border: 1px solid red;
			width: 600rpx;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			padding: 20rpx;

			.logo image {
				display: flex;
				justify-content: center;
				width: 300rpx;
				height: 300rpx;
			}

			.login {
				margin-top: 30rpx;

				.username {
					padding: 20rpx;
					display: flex;
					flex-direction: row;
					justify-content: space-between;
					align-items: center;

					.text {
						font-size: 36rpx;
					}
				}

				.password {
					padding: 20rpx;
					display: flex;
					flex-direction: row;
					justify-content: space-between;
					align-items: center;
				}

				input {
					width: 400rpx;
					height: 80rpx;
					background-color: #f5f6fa;
					border-radius: 50rpx;
					padding: 5rpx 30rpx;
				}

				input::-webkit-input-placeholder {
					text-align: center;
				}
			}

			.loginbutton {
				margin-top: 30rpx;

				image {
					width: 150rpx;
					height: 150rpx;
				}
			}

			.register {
				text-decoration: underline;
				font-size: 30rpx;
				color: #a4b0be;
				margin-top: 20rpx;
			}

			.OthorLogin {
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;
				margin-top: 60rpx;
				padding: 20rpx;

				.top {
					display: flex;
					justify-content: center;
					color: #b2bec3;
				}

				.method {
					width: 400rpx;
					display: flex;
					flex-direction: row;
					justify-content: space-around;
					margin-top: 20rpx;

					image {
						width: 60rpx;
						height: 60rpx;
					}
				}
			}
		}
	}
</style>
