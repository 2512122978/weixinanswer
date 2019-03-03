<template>
	<view>
		您的分数是：{{score}}
	</view>
</template>

<script>
	export default {
		data() {
			return {
				score: 0,

			}
		},
		onLoad() {
			this.init();
		},
		methods: {
			init() {
				let that = this
				const data = uni.getStorageSync('result')
				data.forEach((item, index) => {
					let answer = item.answer
					console.log(answer);
					switch (answer) {
						case 0:
							that.score += 20
							break;
						case 1:
							that.score += 15
							break;
						case 2:
							that.score += 10
							break;
						case 3:
							that.score += 5
							break;
						case 4:
							that.score += 0
							break;
						case 5:
							that.score += 0
							break;
						case 6:
							that.score += 0
							break;
						default:
							break;
					}
				})
			}
		}
	}
</script>

<style>
</style>
