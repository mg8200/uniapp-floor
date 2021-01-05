<template>
	<view class="maincont">
		<view class="login">
			<view class="text">
				你好！
				<text v-if="!isLogin">请登录</text>
				<text v-if="isLogin">欢迎来到本店！</text>
			</view>
			<view class="maincont-login-but" v-if="!isLogin" @click="goLogin">
				<text>登录/注册</text>
			</view>
		</view>
		<view class="myorder">
			<view class="order-nav">
				<view class="myorder-title">
					<view>
						我的订单
					</view>
					<view>
						全部订单>
					</view>
				</view>
				<view class="myorder-menu">
					<view class="myorder-menu-item" v-for="(item,index) in orderText" :key="index">
						<text class="iconfont" :class="item.icon"></text>
						<text>{{item.text}}</text>
					</view>
				</view>
			</view>
		</view>
		<view class="personalmenu">
			<view class="personalmenu-item iss">
				<view class="personalmenu-item">
					<text class="iconfont myicon icon-youhuiquan"></text>
					<text>优惠券</text>
				</view>
				<view class="">
					<text class="iconfont icon-xialayou is-you"></text>
				</view>
			</view>
			<view class="personalmenu-item iss">
				<view class="personalmenu-item">
					<text class="iconfont myicon2 icon-4"></text>
					<text>收货地址</text>
				</view>
				<view class="">
					<text class="iconfont myicon icon-xialayou is-you"></text>
				</view>
			</view>
			<view class="personalmenu-item iss">
				<view class="personalmenu-item">
					<text class="iconfont myicon icon-shezhi"></text>
					<text>设置</text>
				</view>
				<view class="">
					<text class="iconfont icon-xialayou is-you"></text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				orderText: [{
						icon: "icon-daifukuan",
						text: "已付款"
					},
					{
						icon: "icon-daifahuo",
						text: "派送中"
					},
					{
						icon: "icon-pingjia",
						text: "待评价"
					},
					{
						icon: "icon-dingdan",
						text: "已完成"
					}
				],
				isLogin: false
			}
		},
		methods: {
			goLogin() {
				uni.navigateTo({
					url: "../login/login"
				})
			}
		},
		onLoad() {
			uni.getStorage({
				key:"token",
				success:()=>{
					this.isLogin=true;
				}
			})
		}
	}
</script>

<style lang="scss">
	.maincont {
		background-image: url(http://www.minggang.top:8000/img/mine/bg.png);
		width: 100%;
		height: 100%;
		background-repeat: no-repeat;
		background-position: top;
		background-size: 100vw auto;
		box-sizing: border-box;

		.login {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			box-sizing: border-box;
			padding: 1.725rem 0;
			color: #fff;
		}

		.maincont-login-but {
			margin-top: 20rpx;
			width: 32%;
			padding: .5125rem 0;
			background-color: #fff;
			border-radius: 30%/100% 100% 100% 100%;
			color: #000;
			text-align: center;
		}

		.myorder {
			.order-nav {
				background-color: #fff;
				margin-left: .345rem;
				margin-bottom: .345rem;
				padding: 1.035rem;
				width: 89%;
				border-radius: 5px;

				.myorder-title {
					display: flex;
					justify-content: space-between;
					padding-bottom: .6875rem;
					border-bottom: 1px solid #e9ecf0;
				}

				.myorder-menu {
					display: flex;
					justify-content: space-around;
					margin-top: .975rem;

					.myorder-menu-item {
						display: flex;
						flex-direction: column;
						align-items: center;

						.iconfont {
							font-size: 1.4rem;
						}
					}
				}
			}
		}

		.personalmenu {
			background-color: #fff;
			margin-left: .345rem;
			margin-bottom: .345rem;
			padding: .975rem;
			width: 89%;
			border-radius: 5px;

			.personalmenu-item {
				display: flex;
				align-items: center;
				justify-content: flex-start;
				font-size: 16px;
				color: #333;
				width: 100%;

				.myicon {
					font-size: 40rpx;
				}

				.myicon2 {
					font-size: 50rpx;
				}
			}

			.iss {
				padding: 8px 0;
				border-bottom: 1px solid #e9ecf0;
				justify-content: space-between;
			}
		}
	}
</style>
