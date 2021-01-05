<template>
	<view class="content" v-if="goodData">
		<swiper class="my-swiper" indicator-dots indicator-color="#ccc" indicator-active-color="#0000ff" autoplay interval="3000"
		 circular>
			<swiper-item class="my-swiper-item" v-for="(item,index) in goodData.goodsSwiperArr" :key="index">
				<image :src="'http://www.minggang.top:8000'+item" mode=""></image>
			</swiper-item>
		</swiper>

		<!-- 商品简介 -->
		<view class="good-introduction">
			<view class="proinfo">
				<view class="proinfo-head">
					{{goodData.name}}
				</view>
				<view class="proinfo-body">
					<view class="proinfo-bodyleft">
						<view class="fontone redcolor">
							{{goodData.price}}
						</view>
						<view class="fonttwe">
							￥<text class="price">
								{{goodData.original_price}}
							</text>
						</view>
					</view>
					<view class="proinfo-bodyright">
						<view class="fontthree">
							已售{{goodData.sales}}笔
						</view>
					</view>
				</view>
			</view>
			<view class="product-des">
				<view class="product-desitem">
					<view class="product-destitle">
						材料
					</view>
					<view class="product-desbody">
						{{goodData.material}}
					</view>
				</view>
				<view class="product-desitem">
					<view class="product-destitle">
						包装
					</view>
					<view class="product-desbody">
						{{goodData.packaging}}
					</view>
				</view>
				<view class="product-desitem">
					<view class="product-destitle">
						配送
					</view>
					<view class="product-desbody">
						{{goodData.distribution}}
					</view>
				</view>
				<view class="product-desitem">
					<view class="product-destitle">
						附送
					</view>
					<view class="product-desbody">
						{{goodData.attached}}
					</view>
				</view>
			</view>
		</view>

		<!-- 选择数量 -->
		<view class="product-dess">
			<view class="product-destitle">
				数量
			</view>
			<view class="product-count">
				<view class="nmb_jj">
					<view class="jia">
						-
					</view>
					<view class="nr">
						<input type="text" value="" class="srkk" v-model="count" />
					</view>
					<view class="jia">
						+
					</view>
				</view>
			</view>
		</view>
		<!-- 商品评论 -->
		<view class="goodEvaluate">
			<view class="comments-head">
				<view class="comments-head-title">
					商品评论
				</view>
				<view class="comments-head-body">
					<view class="comments-count">
						<text>
							最近已有{{commentsData.length}}条评论
						</text>
						<text class="iconfont icon-xialayou"></text>
					</view>
				</view>
			</view>
			<view class="comments-listbody">
				<view class="comments-item" v-for="item in commentsData" :key="item.id">
					<view class="comments-item-head">
						<view class="comments-itemhead-userinfo">
							<view class="comments-itemhead-touxiang">
								<image style="width: 44rpx;height: 44rpx;" src="https://www.dinghuale.com/public/images/morenTou.png" mode=""></image>
							</view>
							<view class="comments-itemhead-name">
								{{item.username}}
							</view>
						</view>
						<view class="comments-itemhead-star">
							<text class="iconfont icon-shoucang-copy"></text>
						</view>
					</view>
					<view class="comments-item-body">
						<view class="comments-itembody-text">
							{{item.comments_content}}
						</view>
					</view>
				</view>
				<view class="morereviews">
					<text class="more">
						查看更多评论
					</text>
				</view>
			</view>
			<!-- 商品图片详情 -->
			<view class="productDetail">
				<image v-for="(item,index) in goodData.goodsDetailsImgArr" :key="index" :src="'http://www.minggang.top:8000'+item" mode="widthFix" lazy-load="true"></image>
			</view>
		</view>
		<!-- 底部导航 -->
		<view class="goods-carts">
			<uni-goods-nav :options="options" :fill="true" :button-group="buttonGroup" @click="onLeftClick" @buttonClick="buttonClick" />
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 商品id
				goodId: Number,
				// 商品信息 
				goodData: [],
				// 底部导航
				options: [{
					icon: 'home',
					text: '首页'
				}, {
					icon: 'cart',
					text: '购物车',
					info: 0
				}],
				buttonGroup: [{
						text: '加入购物车',
						backgroundColor: '#ff0000',
						color: '#fff'
					},
					{
						text: '立即购买',
						backgroundColor: '#ffa200',
						color: '#fff'
					}
				],
				// 商品数量
				count: 1,
				commentsData: []
			}
		},
		methods: {
			// 获取商品信息
			getGoodData(id) {
				uni.request({
					url: "https://www.minggang.top:444/goods/getGoodsDetails/" + id,
					success: (res) => {
						this.goodData = res.data.data;
						this.goodData.goodsSwiperArr = this.goodData.goodsSwiperArr.split(",");
						this.goodData.goodsDetailsImgArr = this.goodData.goodsDetailsImgArr.split(",")
						let title = this.goodData.name
						uni.setNavigationBarTitle({
							title
						})
					}
				})
			},
			// 获取商品评论信息
			getCommentsData(id) {
				uni.request({
					url: "http://www.minggang.top:8000/comments/getComments/" + id,
					success: (res) => {
						this.commentsData = res.data.data;
					}
				})
			},

			// 左边
			onLeftClick(e) {
				if (e.content.text == "首页") {
					uni.switchTab({
						url: "../index/index"
					})
				}
				// if(e.content.text=="购物车"){
				// 	uni.navigateTo({

				// 	})
				// }
			}
		},
		onLoad(options) {
			this.goodId = options.id;
		},
		mounted() {
			this.getGoodData(this.goodId);
			this.getCommentsData(this.goodId)
		}
	}
</script>

<style lang="scss">
	.content {
		padding-bottom: 100rpx;
	}

	.my-swiper {
		width: 100%;
		height: 750rpx;

		.my-swiper-item {
			width: 100%;
			height: 100%;

			image {
				width: 100%;
				height: 100%;
			}
		}
	}

	.good-introduction {
		.proinfo {
			background-color: #fff;
			padding: 1.035rem;
			border-bottom: 1px solid #e9ecf0;

			.proinfo-head {
				display: flex;
				justify-content: space-between;
			}

			.proinfo-body {
				display: flex;
				justify-content: space-between;
				padding-top: 1.035rem;

				.proinfo-bodyleft {
					display: flex;
					align-items: center;

					.fontone {
						font-size: 1.25rem;
						font-weight: 550;
					}

					.redcolor {
						color: #ff734c;
					}

					.fonttwe {
						font-size: .875rem;
						text-decoration: line-through;
						color: #b4babf;
						margin-left: .1725rem;

						.price {
							font-size: .875rem;
							text-decoration: line-through;
							color: #b4babf;
							margin-left: .1725rem;
						}
					}
				}

				.proinfo-bodyright {
					display: flex;
					align-items: center;

					.fontthree {
						font-size: 20rpx;
					}
				}
			}
		}

		.product-des {
			background-color: #fff;
			padding: .345rem 1.035rem;
			margin-bottom: .5175rem;
			font-size: .9375rem;

			.product-desitem {
				display: flex;
				justify-content: space-between;

				.product-destitle {
					width: 14%;
					padding: .69rem 0;
				}

				.product-desbody {
					width: 84%;
					border-bottom: .0625rem solid #e9ecf0;
					padding: .69rem 0;
				}
			}
		}
	}

	.product-dess {
		background-color: #fff;
		padding: .345rem .975rem;
		margin-bottom: .5175rem;
		font-size: .9688rem;
		display: flex;

		.product-destitle {
			width: 14%;
			padding: .69rem 0;
			line-height: 2rem;
		}

		.product-count {
			box-sizing: border-box;
			width: 86%;
			padding: .69rem 0;

			.nmb_jj {
				overflow: hidden;
				margin-left: .625rem;
				display: flex;

				.jia {
					border-right: none;
					cursor: pointer;
					color: #999;
					width: 2rem;
					padding: 0;
					background-color: #f5f5f5;
					text-align: center;
					line-height: 2rem;
					border-right: 1px solid #ddd;
					width: 1.875rem;
					font-size: 1rem;
					border: none;
					line-height: 1.5rem;
					text-align: center;
				}

				.nr {
					float: left;
					text-align: center;
					line-height: 1.875rem;
					height: 1.875rem;
					border: 1px solid #ddd;
					font-size: 20px;

					.srkk {
						height: 100%;
						width: 1.875rem;
						font-size: 1rem;
						border: none;
						line-height: 1.5rem;
						text-align: center;
					}
				}
			}
		}

	}

	.goodEvaluate {
		background-color: #fff;
		margin-bottom: .5175rem;

		.comments-head {
			display: flex;
			justify-content: space-between;
			align-items: center;
			border-bottom: .0625rem solid #e9ecf0;
			padding: 1.035rem;
			box-sizing: border-box;
		}

		.comments-head-title {
			font-size: 1.09rem;
		}

		.comments-head-body {
			font-size: .7894rem;

			.comments-count {
				text-decoration: none;
				color: #333;
				font-family: 'Microsoft YaHei', Tahoma, Arial, sans-serif;
				display: block;

				text {
					display: inline-block;
					height: 100%;
				}
			}
		}

	}

	.comments-listbody {
		.comments-item {
			padding: .345rem 1.035rem;
			border-bottom: .0625rem solid #e9ecf0;

			.comments-item-head {
				display: flex;
				justify-content: space-between;
				padding: .69rem 0;
				box-sizing: border-box;

				.comments-itemhead-userinfo {
					display: flex;

					.comments-itemhead-touxiang {
						width: 1.3799rem;
					}

					.comments-itemhead-name {
						padding-left: 1.25rem;
					}
				}
			}
		}
	}

	.morereviews {
		display: flex;
		padding-top: 1.035rem;
		padding-bottom: 2.07rem;
		justify-content: center;

		.more {
			padding: .345rem;
			border: 1px solid #232628;
			font-size: .75rem;
			color: #232628;
		}
	}
	.productDetail{
		width: 100%;
		image{
			width: 100%;
		}
	}

	.goods-carts {
		width: 100%;
		position: fixed;
		bottom: 0;
		left: 0;
	}
</style>
