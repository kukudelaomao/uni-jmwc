<template>
	<!-- 这是聊天室 -->
	<view class="chat">
		<scroll-view id="scrollview" scroll-y="true" class="scroll-Y" :scroll-with-animation="true" :scroll-top="scrollTop">
			<view id="msglistview" class="uni-chatMsgCnt">
				<view class="chat-list">
					<text class="chat-username red">平台提醒&nbsp;&nbsp;</text>
					<text class="chat-usertext">平台提醒&nbsp;&nbsp;请您在拍下时确认购买商品和主播实际描述一致，禁止线下交易，谨防上当受骗！</text>
					<text class="chat-username red">平台提醒&nbsp;&nbsp;</text>
				</view>
				<view class="chat-list bg-green">
					<text class="chat-username white">主播&nbsp;&nbsp;</text>
					<text class="chat-usertext">主播&nbsp;&nbsp;欢迎来到我的直播间，喜欢我可以为我点赞哦~</text>
					<text class="chat-username white">主播&nbsp;&nbsp;</text>
				</view>
				<view class="chat-list" v-for="(it,i) in chatList" :key='i'>
					<!-- <text class="chat-usertext">{{it.name}}&nbsp;&nbsp;{{it.text}}</text> -->
					<text :class="{'chat-username':true, 'color1': calName(it.name)===1, 'color2': calName(it.name)===2, 'color3': calName(it.name)===3, color4: calName(it.name)===4,color5: calName(it.name)===0}">{{it.name}}&nbsp;&nbsp;</text>
					<text class="chat-usertext">{{it.name}}&nbsp;&nbsp;{{it.text}}</text>
					<text :class="{'chat-username':true, 'color1': calName(it.name)===1, 'color2': calName(it.name)===2, 'color3': calName(it.name)===3, color4: calName(it.name)===4,color5: calName(it.name)===0}">{{it.name}}&nbsp;&nbsp;</text>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		props: {
			chatList: {
				type: Array, //实际请求获取的列表数据
			}
		},
		data() {
			return {
				scrollTop: 0,
			}
		},
		mounted() {
			setTimeout(() => {
				this.scrollToBottom()
			}, 1000)
		},
		updated() {
			this.scrollToBottom()
		},
		methods: {
			// 滚动至聊天底部  
			scrollToBottom(t) {
				let that = this;
				let query = uni.createSelectorQuery();
				query.select('#scrollview').boundingClientRect();
				query.select('#msglistview').boundingClientRect();
				query.exec((res) => {
					// console.log(res)
					if (res[1].height > res[0].height) {
						that.scrollTop = res[1].height - res[0].height
					}
				})
			},
			calName(name){
				return name.length%5
			}
		}
	}
</script>

<style>
	.scroll-Y {
		height: 300rpx;
		width: 500rpx;
	}

	.chat {
		position: fixed;
		z-index: 10;
		bottom: 120rpx;
		left: 30rpx;
	}
	.chat-list {
		margin-top: 5rpx;
		/* align-items: center; */
		/* justify-content: flex-start; */
		/* flex-direction: row; */
		background-color: rgba(0, 0, 0, .4);
		padding: 10rpx 20rpx 10rpx 20rpx;
		border-radius: 8rpx;
	}

	.chat-username {
		font-size: 24rpx;
		line-height: 40rpx;
		/* font-weight: 900; */
		color: #3dde3a;
		position: absolute;
		text-align: left;
		/* z-index: 9999 !important; */
	}

	.chat-usertext {
		font-size: 24rpx;
		line-height: 40rpx;
		color: white;
		width: 470rpx;
		text-align: left;
		/* z-index: -1 !important; */
	}
	.red {
		color: red;
	}
	.bg-green {
		background-color: #34b930;
	}
	.white {
		color: white;
	}
	.color1 {
		color: #FD7400;
	}
	.color2 {
		color: #FFE11A;
	}
	.color3 {
		color: #BEDB39;
	}
	.color4 {
		color: #1F8A70;
	}
	.color5 {
		color: #004358;
	}
</style>
