<template>
	<view>
		<view class="love">
			<image src="../../static/love.png" mode="aspectFit"></image>
		</view>
		<view class="love love2">
			<image src="../../static/love.png" mode="aspectFit"></image>
		</view>
		<view class="content" :style="{height: mainHeight+'px',background: 'url(../../static/background.jpg) no-repeat',backgroundSize: 'cover'}">
			<view class="dataTitle">
				<view class="other">
					{{getTime().yer}} {{getTime().Mon}}
				</view>
				<view class="day">
					{{getTime().day}}
				</view>

			</view>
			<view class="imgb">

			</view>
			<view :class="{imga:true,shadow:shadwow,transL:transleft,transR:transright,tranfinal:returnF}" :style="[Style3]"
			 @touchstart="getshaow" @touchend="getshaow" @touchmove="getmove">
				<view class="kezi">
					<image class="logo1" src="../../static/border-pink.png" :style="[Style]"></image>
				</view>
				<view class="kezi2" style="background-color: #FFFFFF;">
					<image src="../../static/mini/gala/background/1.png" class="logo2"></image>
					<image :src="getimg()" class="logo2 logoss"></image>
					<image src="../../static/mini/gala/luck/2.png" class="logossr"></image>
					<text class="logotext">{{numluck.content[0]}}</text>
					<text class="logotext1">{{numluck.content[1]}}</text>
				</view>
			</view>0
			<view :class="{imga:true,shadow2:true}" :style="[Style2]">
				<image class="logo logo3" src="/static/border-pink.png"></image>
			</view>
			<view class="change" @click="getshaow({type:'new'})">
				<text class="who">{{allpart[point]}}</text>
				<image src="../../static/change.png" mode="aspectFit" class="tChange"></image>
			</view>
			<view class="button-demo">
				<button :hover-stop-propagation="true" hover-class="btnHoverClass-plain" @click="getTip">抽取运势</button>
			</view>
		</view>
		<pop ref="pop" direction="center" :is_close="true" :is_mask="true" :width="70">
			<view class="luckChose">
				你选取了<text>{{allpart[point]}}</text>,{{point<=2?'你将随机抽取一种运签':'你将抽取只包含'+allpart[point]+'小姐的运签'}}
			</view>
			<view class="button1">
				<button type="warn" class="butt cancel" @click="()=>{$refs.pop.close()}">取消</button>
				<button type="primary" class="butt confirm" @click="getluck">确认</button>
			</view>

		</pop>
	</view>
</template>

<script>
	import pop from "@/components/ming-pop/ming-pop.vue"
	import luckTip from '../../static/luckjson.js'
	export default {
		components: {
			pop
		},
		data() {
			return {
				luckTip,
				returnF: false,
				transfinal: false,
				transleft: false,
				transright: false,
				transleft2: false,
				transright2: false,
				mainHeight: 0,
				title: 'Hello',
				point: 0,
				point2: 1,
				allpart: [
					'一个魂', '一个魂', '一个魂', '嘉然', '乃琳', '珈乐', '向晚', '贝拉'
				],
				shadwow: false,
				movestart: null,
				startplace: null,
				endplace: null,
				index: 7,
				index2: 3,
				status: 0,
				status2: 'running',
				numluck: {luck:'',content:''},
			}
		},
		onLoad() {
			this.mainHeight = uni.getSystemInfoSync().windowHeight
			this.getDetail()
		},
		methods: {
			getDetail() {
				this.numluck = luckTip[7]
				this.numluck.content = this.numluck.content.split(/[\n，]/g)
			},
			getimg() {
				if (this.numluck.luck === '大吉') {
					return '../../static/luckimg/0.png'
				} else if (this.numluck.luck === '吉') {
					return '../../static/luckimg/1.png'
				} else if (this.numluck.luck === '小吉') {
					return '../../static/luckimg/2.png'
				} else if (this.numluck.luck === '半吉') {
					return '../../static/luckimg/3.png'
				} else if (this.numluck.luck === '末小吉') {
					return '../../static/luckimg/4.png'
				} else if (this.numluck.luck === '末吉') {
					return '../../static/luckimg/5.png'
				} else if (this.numluck.luck === '凶') {
					return '../../static/luckimg/6.png'
				}
			},
			getluck() {
				if (this.returnF) return
				this.$refs.pop.close();
				this.transfinal = true
				this.returnF = true
				setTimeout(() => {
					this.status = 1
					this.status2 = 'paused'
				}, 3000)
			},
			getTip() {
				if (this.transfinal) return
				this.$refs.pop.show();
			},
			getTime() {
				let date = new Date();
				let day = date.getDate()
				let Mon = date.toDateString().split(" ")[1]
				let yer = date.getFullYear()
				return {
					day,
					Mon,
					yer
				}
			},
			changeBack(e) {
				if (this.point === 7) {
					this.point = 0
				} else {
					this.point++
				}
			},
			getshaow(e, num) {
				if (e.type === 'touchstart') {
					if (this.transfinal) return this.shadwow = true
					this.shadwow = true
					this.startplace = e.touches[0].pageX
				} else if (e.type === 'touchend') {
					if (this.transfinal && this.status === 0) {
						this.shadwow = false
						return
					} else if (this.transfinal && this.status === 1) {
						this.shadwow = false
						// this.returnF = false
						this.status2 = 'running'
						return
					}
					this.shadwow = false
					let time = e.timeStamp - this.movestart
					if (time < 2000) {
						if ((this.startplace - this.endplace) > 0) {
							// this.changeBack(1)
							this.transleft = true
							setTimeout(() => {
								this.changeBack()
								this.transleft = false
								if (this.point2 === 7) {
									this.point2 = 0
								} else {
									this.point2++
								}
							}, 600)
						} else if ((this.startplace - this.endplace) < 0) {
							// this.changeBack(-1)
							this.transright = true
							setTimeout(() => {
								this.changeBack()
								this.transright = false
								if (this.point2 === 7) {
									this.point2 = 0
								} else {
									this.point2++
								}
							}, 600)
						} else {
							return
						}

					}
					this.movestart = null
					this.startplace = null
					this.endplace = null
				} else if (e.type === 'new') {
					this.transright = true
					setTimeout(() => {
						this.changeBack()
						this.transright = false
						if (this.point2 === 7) {
							this.point2 = 0
						} else {
							this.point2++
						}
					}, 600)
				}

			},
			getmove(e) {
				if (this.transfinal) return
				this.endplace = e.touches[0].pageX
				if (this.movestart) {
					return
				}
				this.movestart = e.timeStamp
			},
		},
		computed: {
			Style() {
				let obj = {
					"background-image": `url(../../static/background/${this.point}.jpg)`,
					"background-size": "cover",
					"z-index": this.index,
				}
				return obj
			},
			Style2() {
				let obj = {
					"background-image": `url(../../static/background/${this.point2}.jpg)`,
					"background-size": "cover",
					"z-index": this.index2
				}
				return obj
			},
			Style3() {
				let obj = {
					'animation-play-state': this.status2,
					'z-index': '5'
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
	@keyframes fade {
		0% {
			transform: scale(1);
		}

		30% {
			transform: rotateY(0deg) scale(1.7);
		}

		75% {
			transform: rotateY(180deg) scale(1.7);
		}

		100% {
			transform: rotateY(180deg) scale(1);
		}
	}

	.fade(@time: 1s, @ease: linear, @fillmode: 1) {
		animation-name: fade;
		animation-duration: @time;
		animation-timing-function: @ease;
		animation-iteration-count: @fillmode;
		animation-fill-mode: forwards;
	}

	.tranfinal {
		.fade(4s)
	}

	.butt {
		display: inline-block;
		width: 40%;
		font-size: 30rpx;
		border-radius: 40rpx;
	}

	.button1 {
		display: flex;
		justify-content: space-between;
	}

	.luckChose {
		width: 80%;
		color: #DFA7CC;
		font-size: 30rpx;
		margin: auto;
		margin-top: 30rpx;
		margin-bottom: 40rpx;

		text {
			color: red;
		}
	}

	.shadow2 {
		box-shadow: 20rpx 20rpx 1px #DFA7CC;
	}

	.transL {
		transform: translateX(-75vw);
		transition: transform 0.6s
	}

	.transR {
		transform: translateX(75vw);
		transition: transform 0.6s
	}

	.shadow {
		box-shadow: #666 0px 0px 20rpx
	}

	.love {
		position: fixed;
		z-index: 3;
		top: 50%;
		left: 10rpx;
		transform: translate(0, -50%);

		image {
			width: 12vh;
		}

	}

	.love2 {
		left: 80vw;
		transform: scale(0.8);
	}

	.change {
		margin-top: 40rpx;
		z-index: 3;

		.who {
			font-size: 40rpx;
			color: #fff;
			font-weight: bold;
			vertical-align: middle;
		}

		.tChange {
			margin-left: 20rpx;
			width: 60rpx;
			height: 60rpx;
			vertical-align: middle;
		}
	}

	.dataTitle {
		color: #FFFFFF;
		z-index: 3;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		margin-bottom: 30rpx;

		.other {
			font-size: 30rpx;
		}

		.day {
			display: block;
			font-size: 90rpx;
			font-weight: bold;
		}
	}

	.content {
		width: 100%;
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
		opacity: 0.5;
		filter: blur(5px);
	}

	.logo {
		height: 44vh;
		width: 25vh;
		border-radius: 30rpx;
		margin-left: auto;
		margin-right: auto;
		position: absolute;
		z-index: 7;
	}

	.logo2 {
		top: 0;
		left: 0;
		height: 44vh;
		width: 25vh;
		border-radius: 30rpx;
		margin-left: auto;
		margin-right: auto;
		position: absolute;
		transform: rotateY(180deg);
		z-index: 6;
	}
	.logossr {
		bottom: 5vh;
		left: 50%;
		height: 12vh;
		width: 12vh;
		margin-left: auto;
		margin-right: auto;
		position: absolute;
		transform: rotateY(180deg) translate(50%, 0);
		z-index: 6;
	}
	.logotext{
		font-family:"momo_xinjian";
		color: red;	
		display: block;
		bottom: 8vh;
		left: 50%;
		height: 13vh;
		max-width: 15vh;
		margin-left: auto;
		margin-right: auto;
		position: absolute;
		transform: rotateY(180deg) translate(25%,0);
		z-index: 6;
		font-size: 30rpx;
	}
	.logotext1{
		color:  red;
		font-family:"momo_xinjian";
		display: block;
		bottom: 6vh;
		left: 50%;
		height: 13vh;
		max-width: 15vh;
		margin-left: auto;
		margin-right: auto;
		position: absolute;
		transform: rotateY(180deg) translate(75%,0);
		z-index: 6;
		font-size: 30rpx;
	}
		

	.logo1 {
		top: 0;
		left: 0;
		height: 44vh;
		width: 25vh;
		border-radius: 30rpx;
		margin-left: auto;
		margin-right: auto;
		position: absolute;
		z-index: 4;
	}

	.imga {
		position: absolute;
		height: 44vh;
		width: 25vh;
		border-radius: 30rpx;
		margin-left: auto;
		margin-right: auto;
		transform-style: preserve-3d
	}

	.imgb {
		position: relative;
		height: 44vh;
		width: 25vh;
		border-radius: 30rpx;
		margin-left: auto;
		margin-right: auto;
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
			border-top-color: #F8EBF2;
			border-left-color: #F8EBF2;
			border-bottom-color: #EDD0E6;
			border-right-color: #EDD0E6;
			background-color: #DFA7CC;
			opacity: 0.9;
		}

		.btnHoverClass-plain::after {
			background-color: rgba(0, 0, 0, 0.08) !important;
		}

		.logoss {
			z-index: 5;
		}
	}
</style>
