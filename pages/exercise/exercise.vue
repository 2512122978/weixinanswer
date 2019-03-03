<template>
	<view class="content">
		<view class="title">
			以下是皮肤N中性，D干性，o油性，P色素，S敏感，W皱纹，SR光敏反应的测试，请如实回答，我们会给出相应专业的皮肤护理及产品搭配方案。
		</view>
		<view class="scroll ">
			<view class="nowQuestion">
				1
			</view>
			<view class="bar">
				<view class="barTo" v-bind:style="'width:' + (nowQuestion+1)/items.length*100+'%;'">

				</view>
			</view>
			<view class="allQuestion">
				{{items.length}}
			</view>
		</view>
		<view class="text">

			<view class="question">
				{{nowQuestion+1}}. {{items[nowQuestion].question}}
			</view>
			<view v-for="(list,index) in items[nowQuestion].list" v-bind:key="index" 
			@click="goNext(index)" :class="'answer '+(items[nowQuestion].answer == index?'active':'')">
				{{items[nowQuestion].list[index].text}}
			</view>
		</view>
		<view class="choseQusetion">
			<view @click="lastQuestion()"  class="lastQusetion">
				<image class="lastQusetionImg" src="../../static/btn-prev.png" mode=""></image>
			</view>
			<view @click="goNext()" class="nextQusetion">
				<image class="nextQusetionImg" src="../../static/btn-next.png" mode=""></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				items:[],
				//当前题目下标
				nowQuestion: -1 ,
				//当前题目选择答案
				nowAnswer: -1 ,
				//当前答案下标
				Answer: -1,
				
			}
		},
		onLoad(){
			this.init();
		},
		watch:{
// 			nowQuestion:function () {
// 				if(this.items.length != 0){
// 					console.log(111)
// 				}else{
// 					console.log(222)
// 				}
// 			}
			
		},
		computed:{
			
		},
		methods:{
			init(){
				let that = this
				that.nowQuestion = 0
				that.Answer = 0
				uni.request({
					url:"https://easy-mock.com/mock/5c796c30525b9135121eb173/weChat/getData",
					method:"GET",
					success(res) {
						that.items = res.data.data
						// console.log(that.items[that.nowQuestion])
					},
					fail(res){
						// console.log("失败")
					}
				})
			},
			goNext(index){
				let that = this
				// console.log(that.nowQuestion+1)
				if(that.nowQuestion+1 == that.items.length){
					uni.setStorageSync('result', that.items)
					uni.navigateTo({
						url:'../score/score'
					})
					console.log("已经是最后一题了")
					return
				}
				console.log("nowQuestion")
				console.log(that.nowQuestion)
				if(isNaN(index) && that.items[that.nowQuestion].answer == -1 ){
					uni.showToast({
						title: "你还没有选择题目呢",
						icon: "none"
					})
					return
					
				}
				//将答案下标保存在data中
				that.items[that.nowQuestion].answer = index
				//自加，进行下一题
				that.nowQuestion ++
			},
			lastQuestion(){
				let that = this
				if(that.nowQuestion == 0){
					uni.showToast({
						title:"已经是第一题了",
						icon:"none"
					})
					return
				}
					that.nowQuestion--
					return
			}
		}
	}
	
</script>

<style>
	page {
		background: #EB5268;
	}
	.content{
		padding: 30upx 30upx;
	}
	.content .title {
		color: #FAF4FF;
		font-size: 26upx;
		line-height: 38upx;
		text-align: center;
	}

	.content .scroll {
		font-size: 36upx;
		display: flex;
		justify-content: center;
		align-items: center;
		color: #FAF4FF;
		margin-top: 22upx ;
		padding: 0 16upx;
	}

	.content .scroll .bar {
		color: blue;
		width: 100%;
		height: 32upx;
		background: #BB3B4D;
		border-radius: 80upx;
		margin: 0 10upx;
	}

	.content .scroll .bar .barTo {
		background: #FF9DAB;
		transition: .5s;
		width: 10%;
		height: 32upx;
		border-radius: 80upx;
	}

	.content .text {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		
	}

	.content .text .question {
		font-size: 34upx;
		color: #FAF4FF;
		margin: 24upx 28upx;
	}

	.content .text .answer {
		display: flex;
		flex-direction: column;
		/* align-items: center;
		justify-content: center; */
		text-align: center;
		color: #EB5268;
		width: 60%;
		height: 180%;
		padding: 30upx 56upx;
		margin: 10upx;
		background: #FFFFFF;
		font-size: 26upx;
		border-radius: 100upx;
	}
	.content .text .answer.active{
		color: #FFFFFF;
		background-color: #BB3B4D;
	}
	.content .choseQusetion{
		position: absolute;
		bottom: 10upx;
		left: 10upx;
		right: 10upx;
		display: flex;
		justify-content: space-between;
		/* justify-content: flex-start; */
	}
	.content .choseQusetion .lastQusetionImg{
		
		max-width: 120upx;
		max-height: 120upx;
	}
	.content .choseQusetion .nextQusetionImg{
		/*display: flex; 
		justify-content: flex-end; */
		max-width: 120upx;
		max-height: 120upx;
	}
	.content .choseQusetion .lastQusetion{
		max-width: 140upx;
		max-height: 140upx;
		display: flex;
		justify-content: flex-start;
	}
	.content .choseQusetion .nextQusetion{
		max-width: 140upx;
		max-height: 140upx;
		display: flex;
		justify-content: flex-end;
	}
	
</style>
