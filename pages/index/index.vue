<template>
	<view>
		<view class="love">
			<image src="../../static/love.png" mode="aspectFit"></image>
		</view>
		<view class="love love2">
			<image src="../../static/love.png" mode="aspectFit"></image>
		</view>
	<view class="content" :style="{height: mainHeight+'px'}">
		<view class="dataTitle">
			<view class="other">
				{{getTime().yer}} {{getTime().Mon}}
			</view>
			<view class="day">
				{{getTime().day}}
			</view>
			
		</view>
		<view class="imga" :style="[Style]">
			<image class="logo" src="/static/border-pink.png"></image>
		</view>
		<view class="change" @click="changeBack">
			<text class="who">{{allpart[point]}}</text>
			<image src="../../static/change.png" mode="aspectFit" class="tChange"></image>
		</view>
		<view class="button-demo">
			<button :hover-stop-propagation="true" hover-class="btnHoverClass-plain">抽取运势</button>
		</view>
	</view>
</view>
</template>

<script>
	export default {
		data() {
			return {
				mainHeight: 0,
				title: 'Hello',
				point:0,
				allpart:[
					'一个魂','一个魂','一个魂','嘉然','乃琳','珈乐','向晚','贝拉'
				]
			}
		},
		onLoad() {
			this.mainHeight = uni.getSystemInfoSync().windowHeight
		},
		methods: {
			getTime(){
				let date = new Date();
				let day = date.getDate()
				let Mon =date.toDateString().split(" ")[1]
				let yer = date.getFullYear()
				return {
					day,Mon,yer
				}
			},
			changeBack(){
				if(this.point===7){
					this.point = 0
				}else{
					this.point++
				}
				
			}
		},
		computed:{
			Style(){
				let obj = {
					"background-image":`url(../../static/background/${this.point}.jpg)`,
					"background-size": "cover"
				}
				return obj
			}
		},
		onShow() {
			this.mainHeight = uni.getSystemInfoSync().windowHeight
		}
	}
</script>

<style lang="less" scoped>
	.love{
		position: fixed;
		z-index: 3;
		top: 50%;
		left:10rpx;
		transform: translate(0,-50%);
		image{
			width: 12vh;
		}
		
	}
	.love2{
		left:80vw;
		transform: scale(0.8);
	}
	.change{
		margin-top: 10rpx;
		z-index: 5;
		.who{
			font-size: 40rpx;
			color: #fff;
			font-weight: bold;
			vertical-align: middle;
		}
		.tChange{
			margin-left: 20rpx;
			width: 60rpx;
			height: 60rpx;
			vertical-align: middle;
		}
	}
	.dataTitle{
		color: #FFFFFF;
		z-index: 5;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		margin-bottom: 30rpx;
		.other{
			font-size: 30rpx;
		}
		.day{
			display: block;
			font-size: 90rpx;
			font-weight: bold;
		}
	}
	.content {
		width: 100%;
		background: url(../../static/background.jpg) no-repeat;
		background-size: cover;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		box-sizing: border-box;
		z-index: 1;
	}

	.content:after {
		content: "";
		/*此处指定的宽度与高度特定为模糊区域的大小*/
		width: 100%;
		height: 100%;
		position: absolute;
		/*伪元素使用绝对定位*/
		top: 0;
		left: 0;
		bottom: 0;
		right: 0;
		z-index: 2;
		/*在这种定位方式下，无需指定伪元素的大小*/
		background: inherit;
		opacity: 0.7;
		filter: blur(5px);
	}
	.logo {
		height: 45vh;
		width: 25vh;
		border-radius: 30rpx;
		margin-left: auto;
		margin-right: auto;
		z-index: 4;
	}
	.imga{
		height: 45vh;
		width: 25vh;
		border-radius: 30rpx;
		margin-left: auto;
		margin-right: auto;
		z-index: 3;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.button-demo {
		margin-top: 30rpx;
		z-index: 4;

		button {
			width: 350rpx;
			font-size: 40rpx;
			height: 90rpx;
			line-height: 85rpx;
			border-radius: 60rpx;
			color: #F8EBF2;
			border: 5rpx solid;
			border-top-color:#F8EBF2;
			border-left-color:#F8EBF2;
			border-bottom-color: #EDD0E6;
			border-right-color: #EDD0E6;
			background-color: #DFA7CC;
			opacity: 0.9;
		}
		.btnHoverClass-plain::after {
			background-color: rgba(0, 0, 0, 0.08) !important;
		}
	}
</style>
