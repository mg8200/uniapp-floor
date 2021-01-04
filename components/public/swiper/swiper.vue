<template>
	<view>
		<swiper class="my-swiper" indicator-dots indicator-color="#ccc" indicator-active-color="#0000ff" autoplay interval="3000" circular>
			<swiper-item class="my-swiper-list" v-for="item in banner" :key="item.id">
				<image :src="'http://www.minggang.top:8000'+item.src"></image>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default {
		props: {
			url: {
				type: String,
				require: true
			}
		},
		data() {
			return {
				banner: []
			}
		},
		methods: {
			getBanner() {
				let url = this.url;
				uni.request({
					method: "GET",
					url,
					success: (res) => {
						if (res.data.code == 200) {
							this.banner = res.data.data;
							console.log(this.banner)
						}
					}
				})
			}
		},
		mounted() {
			console.log(this.url);
			this.getBanner()
		}
	}
</script>

<style lang="scss">
	.my-swiper{
		width: 100%;
		height: 11.875rem;
		.my-swiper-list {
			width: 100%;
			height: 11.875rem;
			image{
				width: 100%;
				height: 100%;
			}
		}
	}
	
</style>
