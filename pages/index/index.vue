<template>
	<view>
		<!-- 轮播图 -->
		<view class="banner" v-if="sipwerList.length>0">
			<my-swiper dots :list="sipwerList" :interval="5000" imgKey="imgUrl" autoplay :crown="true" :loop="true" :shadow='true'
			 :height='129' :previousMargin="120" :nextMargin='120' :imgRadius="5" />
		</view>
		<!-- 服务介绍 -->
		<view class="service-introduced">
			<view class="service-introduced-item">
				<text class="iconfont icon-dui"></text>
				<text class="text"> 品牌鲜花网 </text>
			</view>
			<view class="service-introduced-item">
				<text class="iconfont icon-dui"></text>
				<text class="text"> 实拍秀 </text>
			</view>
			<view class="service-introduced-item">
				<text class="iconfont icon-dui"></text>
				<text class="text"> 1小时送达 </text>
			</view>
			<view class="service-introduced-item">
				<text class="iconfont icon-dui"></text>
				<text class="text"> 50w+好评 </text>
			</view>
		</view>
		<!-- 从类型快速选择鲜花 -->
		<view class="fast-type-list">
			<view class="fast-type-item" v-for="item in fastTypeList" :key="item.id" @click="goGoodList(item)">
				<image :src="'http://www.minggang.top:8000'+item.imgSrc" class="image"></image>
				<view class="text">{{item.name}}</view>
			</view>
		</view>
		<!-- 一秒选花 -->
		<view class="second-pick-flowers">
			<view class="second-pick-flowers-title">
				<view class="second-pick-flowers-text">
					一秒选花
				</view>
			</view>
			<view class="second-pick-flowers-list top">
				<view class="second-pick-flowers-item" v-for="(item,i) of secondPickFlowersList" :key="item.sid"  @click="goquick(item)">
					<image :src="'http://www.minggang.top:8000'+item.src" mode=""></image>
					<text>{{item.title}}</text>
				</view>
			</view>
			<view class="second-pick-flowers-list bottom">
				<view class="second-pick-flowers-item" v-for="item in secondPickFlowersList_2" :key="item.sid" @click="goquick(item)">
					<view class="text">{{item.title}}</view>
					<image :src="'http://www.minggang.top:8000'+item.src" mode=""></image>
				</view>
			</view>
		</view>
		<!-- 商品模块 -->
		<view class="module-list">
			<uni-module v-for="item in moduleList" :key="item.good_tag_id" :name="item.name" :pid="item.good_tag_id"></uni-module>
		</view>
	</view>
</template>

<script>
	import mySwiper from "../../components/public/swiper/swiper.vue"
	import uniModule from "./module.vue"
	export default {
		components: {
			mySwiper,
			uniModule
		},
		data() {
			return {
				sipwerList: [],
				// 从类型快速选择鲜花列表
				fastTypeList: [],
				// 一秒选花列表,第一段
				secondPickFlowersList: [],
				// 一秒选花列表,第er段
				secondPickFlowersList_2:[],
				// 模块数据
				moduleList: []
			}
		},
		onLoad() {
			this.getFastTypeList();
			this.getSecondPickFlowersList();
			this.getModuleList();
			this.getSipwerList()
		},
		methods: {
			// 获取轮播图数据
			getSipwerList() {
				uni.request({
					url: "https://www.minggang.top:444/home/getBanner",
					success: (res) => {
						this.sipwerList = res.data.data;
					}
				})
			},
			// 获取类型快速选择鲜花列表
			getFastTypeList() {
				uni.request({
					method: "GET",
					url: "https://www.minggang.top:444/home/homeNav",
					success: (res) => {
						this.fastTypeList = res.data.data;
					}
				})
			},

			// 获取一秒选花列表
			getSecondPickFlowersList() {
				uni.request({
					methods: "get",
					url: "https://www.minggang.top:444/home/imgNav",
					success: (res) => {
						
						let secondPickFlowersList = res.data.data;
						secondPickFlowersList.forEach(item=>{
							item.sid=item.id
						})
						this.secondPickFlowersList=secondPickFlowersList.slice(0,3);
						this.secondPickFlowersList_2=secondPickFlowersList.slice(3);
					}
				})
			},
			// 获取模块数据
			getModuleList() {
				uni.request({
					methods: "GET",
					url: "https://www.minggang.top:444/home/getGoodsType",
					success: (res) => {
						this.moduleList = res.data.data
					}
				})
			},
			// 前往商品列表
			goGoodList(val) {
				let id = val.good_type_id
				uni.navigateTo({
					url: `../good/goodsList?tid=${id}&name=${val.name}`
				})
			},
			goquick(val) {
				let id=val.sid;
				let title=val.title;
				uni.navigateTo({
						url: `../good/quickSelect?tid=${id}&name=${title}`
				})
			}
		}
	}
</script>

<style lang="scss">
	.banner {
		padding-top: 20rpx;
		width: 100%;
		height: 8.0625rem;
		background-color: #fff;
	}

	.service-introduced {
		width: 100%;
		padding: .9375rem;
		background-color: #fff;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: space-between;

		.service-introduced-item {
			font-size: 0.24rem;

			.text {
				margin-left: 0.5rem;
			}
		}
	}

	.fast-type-list {
		background-color: #fff;
		display: flex;
		justify-content: space-around;   
		align-items: center;
		font-size: 28rpx;
		padding: 22rpx 0;

		.fast-type-item {
			.image {
				width: 2.5rem;
				height: 2.5rem;
				display: inline-block;
			}

			.text {
				text-align: center;
			}
		}
	}

	.second-pick-flowers {
		background-color: #fff;
		width: 100%;
		margin-top: .625rem;
		padding: .625rem;
		box-sizing: border-box;

		.second-pick-flowers-title {
			margin-bottom: .625rem;

			.second-pick-flowers-text {
				border-left: .1875rem solid #884e22;
				padding-left: .625rem;
				height: .9375rem;
				font-weight: 600;
				color: #884e22;
				font-size: .3rem;
				line-height: .9375rem;
			}
		}

		.second-pick-flowers-list {
			display: flex;
			justify-content: space-between;
			align-items: center;
		}

		.top {
			display: flex;

			.second-pick-flowers-item {
				width: 30%;
				height: 7.8125rem;
				overflow: hidden;
				border-radius: .3125rem;
				position: relative;

				image {
					width: 100%;
					height: 100%;
				}

				text {
					background-color: #ffafb2;
					height: 1.875rem;
					width: 100%;
					line-height: 1.875rem;
					text-align: center;
					position: absolute;
					bottom: 0;
					left: 0;
					color: #fff;
					font-weight: 600;
					letter-spacing: .04rem;

				}
			}

		}

		.bottom {
			width: 100%;

			.second-pick-flowers-item {
				margin-top: .625rem;
				width: 5.3125rem;
				height: 7.325rem;
				border-radius: .3125rem;
				text-align: center;
				background-color: #f3ebe3;

				image {
					width: 100%;
					height: 160rpx;
				}

				.text {
					text-align: center;
					font-weight: 600;
					color: #884e22;
					padding-top: .2rem;
					padding-bottom: .1rem;
				}
			}
		}
	}
</style>