<template>
	<view>
		<view class="good-list">
			<view class="good-item" v-for="item in goodsData" :key="item.id">
				<image :src="'http://www.minggang.top:8000'+item.src" mode="widthFix"></image>
		
				<view class="goods-item-content">
					<view class="minifont">
						{{item.name}}
					</view>
					<view class="salsList">
						<view class="redfont">
							￥{{item.price}}
						</view>
						<view class="sales">
							售量{{item.sales}}笔
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:{
			goodsData:{
				type:Array,
				require:true
			}
		},
		data() {
			return {
				
			}
		},
		methods: {
			
		},
	}
</script>

<style lang="scss">
.good-list {
		width: 100%;
		padding: .6875rem;
		box-sizing: border-box;
		background-color: #e9ecf0;
		width: 100%;
		display: flex;
		justify-content: space-between;
		flex-wrap: wrap;

		.good-item {
			width: 49%;
			background-color: #fff;
			border-radius: .3125rem;
			margin-bottom: 1vh;

			image {
				width: 100%;
			}

			.goods-item-content {
				width: 100%;
				padding: .6875rem;
				box-sizing: border-box;
	.minifont {
			box-sizing: border-box;
			font-size: .875rem;
			overflow: hidden;
			margin: 0;
			text-overflow: ellipsis;
			white-space: nowrap;
			color: #777;
		}
				.salsList {
					display: flex;
					margin: 0;
					justify-content: space-between;

					.redfont {
						color: #ff734c;
						margin: 0;
					}

					.sales {
						color: #ccc;
						margin: 0;
						font-size: .75rem;
					}
				}
			}
		}
}
</style>
