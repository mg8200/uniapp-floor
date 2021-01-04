<template>
	<view class="content">
		<view class="left">
			<view class="left-list">
				<view class="left-item" v-for="(item,index) in leftData" :key="item.id" :class="leftActive==index?'active':''"
				 @click="handleClick(item,index)">
					{{item.name}}
				</view>
			</view>
		</view>

		<view class="right">
			<view class="information-top" v-if="rightData.length>0">
				<view class="title">
					<h3>
						<text>{{leftData[leftActive].name}}</text>
					</h3>
				</view>
			</view>
			<view class="information-list">
				<view class="information-item" v-for="item in rightData" :key="item.id" @click="goInformationDetail(item.id)">
					<view class="img">
						<image :src="'http://www.minggang.top:8000'+item.details_img" mode="widthFix"></image>
					</view>
					<view class="content">
						<view class="item-title">
							{{item.title}}
						</view>
						<view class="text">
							{{item.title_text}}
						</view>
						<view class="lab_grp">
							<text class="tag" v-if="item.tag_name">
								{{item.tag_name}}
							</text>
							<text class="viewed">
								<text class="icon"></text>
								<text class="count">{{item.viewed_count}}</text>
							</text>
							<text class="time">{{item.time}}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				leftData: [],
				leftActive: 0,
				rightData: []
			}
		},
		methods: {
			getLeftData() {
				let slef
				uni.request({
					url: "https://www.minggang.top:444/information/getNav",
					success: (res) => {
						this.leftData = res.data.data;
						this.getRightData(this.leftData[0].catid)
					}
				})
			},
			getRightData(id) {
				uni.request({
					url: "http://www.minggang.top:8000/information/getArticleLists/" + id,
					success: (res) => {
						this.rightData = res.data.data
					}
				})
			},
			handleClick(val, index) {
				this.leftActive = index;
				// #ifdef H5
				this.getRightData(val.catid)
				//#endif

				// #ifdef MP-WEIXIN
				this.getRightData(this.leftData[index].catid)
				// #endif
			},
			goInformationDetail(id){
				uni.navigateTo({
					url:"./informationDetial?did="+id
				})
			}
		},
		onLoad() {
			this.getLeftData();
		}
	}
</script>

<style lang="scss">
	.content {
		background-color: #fff;
		width: 100%;
		display: flex;
	}

	.left {
		width: 160rpx;
		height: 100%;
		background-color: #fff;

		.left-item {
			position: relative;
			display: block;
			box-sizing: border-box;
			padding: 40rpx 22rpx;
			overflow: hidden;
			color: #323233;
			font-size: 28rpx;
			line-height: 40rpx;
			cursor: pointer;
			-webkit-user-select: none;
			user-select: none;
		}

		.active::before {
			position: absolute;
			top: 50%;
			left: 0;
			width: 4px;
			height: 16px;
			background-color: #ee0a24;
			-webkit-transform: translateY(-50%);
			transform: translateY(-50%);
			content: '';
		}


		.active {
			color: #ee0a24;
		}
	}

	.right {
		width: 73%;
		background-color: #fff;
		padding: .6875rem;

		.information-top {
			.title {
				border-bottom: 1px solid #eee;
				padding: .3125rem 0;

				h3 {
					height: 60rpx;
					text-align: center;

					text {
						display: block;
						float: left;
						width: 100%;
						text-align: center;
						font-size: 28px;
						color: #d7000f;
						font-weight: 700;
						line-height: 100%;
					}
				}
			}
		}
	}

	.information-list {
		padding: .9375rem 0;
		padding-top: .3125rem;
		box-sizing: border-box;

		.information-item {
			display: flex;
			justify-content: space-between;
			border-bottom: .0625rem solid #eee;
			padding: .3125rem 0;
			box-sizing: border-box;

			.img {
				width: 30%;
				display: flex;
				align-items: center;

				image {
					width: 100%;
				}
			}

			.content {
				width: 70%;
				box-sizing: border-box;
				padding: .3125rem;
				display: flex;
				flex-wrap: wrap;

				.item-title {
					color: #333;
					width: 100%;
					text-overflow: ellipsis;
					white-space: nowrap;
					font-weight: 600;
					font-size: .9375rem;
					overflow: hidden;
				}

				.text {
					padding-top: .5rem;
					font-size: .625rem;
					width: 100%;
					overflow: hidden;
					text-overflow: ellipsis;
					white-space: nowrap;
				}

				.lab_grp {
					margin-top: .5rem;
					height: 1.5rem;
					display: flex;
					line-height: 1.5rem;

					.tag {
						font-size: .625rem;
					}

					.viewed {
						padding-left: 0.9375rem;

						.icon {
							width: 16px;
							height: 12px;
							background: url(https://static.chinapp.com/home/img/bg.png) -579px -43px no-repeat;
							float: left;
							margin: 6px 5px 0 0;
						}
						.count{
							font-size: 20rpx;
						}
					}
				}
				.time{
					font-size: .625rem;
					    line-height: 1.5rem;
					    box-sizing: border-box;
					    padding-left: .9375rem;
					
				}
			}
		}
	}
</style>
