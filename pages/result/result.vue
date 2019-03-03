<template>
	<view>
		您的得分：{{score}}
	</view>
</template>

<script>
	export default {
		data() {
			return {
				score: 0
			}
		},
		methods: {
			initData() {
				let that = this
				const data = uni.getStorageSync('result')

				//定义得分数组
				let a = 20
				let b = 10
				let c = 15
				let d = 5
				let e = 0

				data.forEach((item, index) => {
					let answer = item.answer
					// console.log(answer)
					switch (answer) {
						case 0:
							that.score += a
							break;
						case 1:
							that.score += b
							break;
						case 2:
							that.score += c
							break;
						case 3:
							that.score += d
							break;
						case 4:
							that.score += e
							break;
						default:
							that.score += 0
							break;
					}
				})

			}
		},
		onLoad() {
			this.initData()
		}
	}
</script>

<style lang="">
</style>
