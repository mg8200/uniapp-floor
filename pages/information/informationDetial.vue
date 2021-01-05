<template>
	<view class="content" v-html="data.details">
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				data:{}
			}
		},
		methods: {
			getData(id){
				uni.request({
					url:"https://www.minggang.top:444/information/getArticleitem/"+id,
					success: (res) => {
						this.data=res.data.data;
						let title=this.data.title
						uni.setNavigationBarTitle({
							title
						})
					}
				})
			}
		},
		onLoad(options) {
			this.getData(options.did)
		}
	}
</script>

<style>
.content{
	width: 100%;
	height: 100%;
	background-color: #fff;
}
</style>
