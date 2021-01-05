<template>
	<view>
		<!-- 头部筛选 -->
		<view class="header">
			<view class="item" :class="active==0?'active':''" @click="sort(0)">
				综合
			</view>
			<view class="item" :class="active==1?'active':''" @click="sort(1)">
				售量
			</view>
			<view class="item" :class="active==2?'active':''" @click="sort(2)">
				价格
			</view>
		</view>
		<!-- 商品列表 -->
		<uni-renderGoodList v-if="goodList.length>0" :goodsData="goodList"></uni-renderGoodList>
	</view>
</template>

<script>
	import uniRenderGoodList from "./renderGoodList.vue";
	export default {
		components: {
			uniRenderGoodList
		},
		data() {
			return {
				goodId:Number,
				goodList: [],
				active: 0,
				isAscending: true
			}
		},
		methods: {
			getGoodList(id) {
				uni.request({
					method: "GET",
					url: "https://www.minggang.top:444/goods/getGoodsTypeName/" + id,
					success: (res) => {
						this.goodList = res.data.data;
					}
				})
			},
			sort(val) {
				this.active = val;
				if (val == 0) {
					this.getGoodList(this.goodId)
				}
				if (val == 1) {
					this.salesOrder()
				}
				if(val==2){
					this.isAscending = !this.isAscending;
					this.priceOrder()
				}
			},
			salesOrder() {
				this.goodList.sort((a, b) => {
					return a.sales - b.sales;
				});
			},
			priceOrder() {
				if (this.isAscending) {
					this.goodList.sort((a, b) => {
						return b.price - a.price;
					});
				} else {
					this.goodList.sort((a, b) => {
						return a.price - b.price;
					});
				}
			},
		},
		onLoad(options) {
			uni.setNavigationBarTitle({
				title: options.name
			})
			this.goodId=options.tid;
			this.getGoodList(options.tid)
		}
	}
</script>

<style lang="scss">
	.header {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: space-around;
		background-color: #FFFFFF;
		height: 2.8225rem;
	}

	.active {
		color: red;
	}
</style>
