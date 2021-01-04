<template>
	<view class="module">
		<view class="module-title">
			<view class="text">
				{{name}}
			</view>
			<text></text>
		</view>
		<view class="pro-list">
			<view class="pro-item" v-for="item in dataList" @click="goGoodDetail(item)" :key="item.id">
				<image :src="'http://www.minggang.top:8000'+item.src" lazy-load="true"></image>
				<view class="one-txt-cut">
					{{item.name}}
				</view>
				<view class="pro-bottom">
					<text class="price">￥{{item.price}}</text>
					<text class="sales">
						售量{{item.sales}}笔
					</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			name: {
				type: String,
				require: true
			},
			pid: {
				type: Number,
				require: true
			}
		},
		data() {
			return {
				// 商品数据
				dataList: []
			};
		},
		methods: {
			getDataList() {
				let id = this.pid;
				uni.request({
					method: "GET",
					url: "https://www.minggang.top:444/home/module/" + id,
					success: (res) => {
						this.dataList = res.data.data;
					}
				})
			},
			// 前往商品详情页
			goGoodDetail(val){
				uni.navigateTo({
					url:"../good/goodDetail?id="+val.id
				})
			}
		},
		mounted() {
			this.getDataList()
		}
	}
</script>

<style lang="scss">
	.module {
		margin-top: .625rem;
		padding-top: .9375rem;
		padding-bottom: .9375rem;
		background-color: #fff;

		.module-title {
			text-align: center;
			font-weight: 600;
			padding-bottom: .9375rem;
		}

		.pro-list {
			display: flex;
			justify-content: space-between;
			align-items: center;
			flex-wrap: wrap;
			padding: 0 1.035rem;

			.pro-item {
				width: 49%;
				background-color: #fff;
				border-radius: .1rem;
				margin-top: .3rem;
				border-radius: 10px;

				image {
					width: 10.65rem;
					height: 10.65rem;
				}

				.one-txt-cut {
					color: #666;
					margin: .345rem 0;
					overflow: hidden;
					white-space: nowrap;
					text-overflow: ellipsis;
				}

				.pro-bottom {
					display: flex;
					justify-content: space-between;
					align-items: center;

					.price {
						font-size: .9784rem;
						color: #f43;
						font-weight: 600;
					}

					.sales {
						font-size: 0.828rem;
						color: #999;
					}
				}
			}
		}
	}
</style>
