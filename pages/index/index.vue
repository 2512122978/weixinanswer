<template>
	<view class="content3">
		<view class="content1">
			<view v-if="this.thisIndex === 0">
				以下是对皮肤N中性，D干性，O油性，P色素，S敏感，W皱纹，SR光敏反应的测试，请如实回答，我们会给出相应的专业的皮肤护理及产品搭配方案。
			</view>
		</view>
		<view class="text5">
			<view class="rogress bar">
				<view class="text">
					{{pageIndex}}
				</view>
				<view class="jump">
					<view class="" @click="jump">
						跳转到练习
					</view>
				</view>
				<!-- 进度条 -->
				<view class="line">
					<view class="bar" :style="'width:'+(thisIndex+1) / items.length * 100+'%;'"></view>
				</view>
				<view class="text2">
					{{items.length}}
				</view>
			</view>
			<view class="content">
				<view class="question">{{pageIndex}}、{{thisQuestion.question}}</view>
				<view class="items">
					<view v-for="(list,ind) in thisQuestion.list" :key="ind" :class="'btn '+(thisQuestion.answer === ind?'active':'') "
					 @click="goNext(ind)">
						{{list.text}}
					</view>
				</view>
			</view>
		</view>
		<view class="btn-switch">
			<view class="btn1" @click="goPrev()">
				<image src="../../static/btn-prev.png" mode="widthFix" class="img"></image>
			</view>
			<view class="btn1" @click="goNext()">
				<image src="../../static/btn-next.png" mode="widthFix" class="img"></image>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//题目集合
				items: [],
				//得分
				total: 0,
				//当前题目
				thisQuestion: '',
				//当前题目的下标
				thisIndex: -1,
				//当前的题目答案
				thisAnswer: -1,
			}
		},
		onLoad() {
			this.init()
		},
		watch: {
			thisIndex: function(a, b) {
				if(this.items.length != 0){
					this.thisAnswer = this.items[this.thisIndex].answer
					console.log("当前题目答案"+this.thisAnswer)
				}
			}
		},
		computed: {
			pageIndex: function() {
				return this.thisIndex + 1
			},
		},
		methods: {
			init() {
				let that = this
				//初始化下标
				that.thisIndex = 0
				//初始化题目集合
				uni.request({
					url: "https://easy-mock.com/mock/5c796c30525b9135121eb173/weChat/getData",
					method: "GET",
					success: (res) => {
						that.items = res.data.data
						if (that.items) {
							//初始化题目
							that.thisQuestion = that.items[that.thisIndex]
						}
					},
					fail: (res) => {

					}
				})
			},
			goPrev() {
				let that = this
				let thisIndex = that.thisIndex
				if (thisIndex <= 0) {
					uni.showToast({
						title: "已经是第一题了!",
						icon: "none"
					})
					return
				}
				that.thisIndex--
				that.thisQuestion = that.items[thisIndex - 1]

			},
			goNext(ind) {
				let that = this
				let thisIndex = that.thisIndex

				if (thisIndex + 1 === that.items.length) {
					that.thisAnswer = ind
					that.items[thisIndex].answer = ind
					uni.setStorageSync('result', that.items)
					uni.navigateTo({
						url: '/pages/result/result'
					})
					return
				}

				if (isNaN(ind) && that.thisAnswer === -1) {
					uni.showToast({
						title: "你还没有选择题目呢",
						icon: "none"
					})
					return
				}

				//保存答案
				that.items[thisIndex].answer = ind

				//切换到下一题
				that.thisIndex++
				that.thisQuestion = that.items[thisIndex + 1]
			},
			jump(){
				uni.navigateTo({
					url:"../exercise/exercise",
					// return
				})
			}
		}
	}
</script>
<style>
	page {
		background: #EB5268;

	}

	.content {
		color: white;
		margin: 50upx 0;
	}

	.content .question {
		font-size: 36upx;
		color: #fff4f5;
		margin: 80upx 34upx;
	}

	.btn {
		display: flex;
		padding: 30upx 20upx;
		min-width: 440upx;
		line-height: 44upx;
		border-radius: 30upx;
		max-width: 100%;
		font-size: 32upx;
		color: #FF0000;
		justify-content: flex-start;
		background: white;
	}

	.content1 {
		color: #fff4f5;
		line-height: 35upx;
		min-height: 105upx;
		font-size: 20upx;
		text-align: center;
		margin: 50upx 0;
	}

	.content3 {
		padding: 0 38upx;
	}

	.content2 {
		margin: 50upx 0;
	}

	.text,
	.text2 {
		color: white;
	}

	.line {

		width: 100%;
		height: 30upx;
		border-radius: 20upx;
		background: #A33746;
	}

	.rogress {
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
	}

	.rogress .bar {
		height: 100%;
		width: 10%;
		transition: .5s;
		border-radius: 20upx;
		background: #FF9DAB;
	}

	.items .btn {
		display: flex;
		margin: 20upx 0;
		justify-content: center;
		align-items: center;
	}

	.items {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.items .btn.active {
		background: #BB3B4D;
		color: white;
	}

	.btn-switch {
		display: flex;
		justify-content: space-between;
		position: fixed;
		left: 0;
		right: 0;
		bottom: 0upx;
		display: flex;
		flex-direction: row;
	}

	.btn-switch .btn1 {}

	.btn-switch .btn1 .img {
		width: 110upx;
	}
</style>
