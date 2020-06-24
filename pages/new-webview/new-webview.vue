<template>
	<view class="container">
		<web-view :webview-styles="webviewStyles" :src="url" @message="handleMessage"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				webviewStyles: {
					progress: {
						color: '#00aaff'
					}
				},
				url: '',
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			console.log(option.newUrl); //打印出上个页面传递的参数。
			this.url = option.newUrl
		},
		methods: {
			handleMessage(evt) {
				console.log('接收到的消息：' + JSON.stringify(evt.detail.data));
				// const evtData = evt.detail.data[0]
				// switch (evtData.type) {
				// 	case 'make-share-qrcode':
				// 		uni.navigateTo({
				// 			url: '/pages/make-share-qrcode/make-share-qrcode?qrcode=' + evtData.qrcode
				// 		});
				// 		break;
				// 	default:
				// 		break;
				// }
			},
		}
	}
	// 动态设置应用全屏显示！  
	// plus.navigator.setFullscreen(true);
</script>

<style>
	.container {
		padding: 20px;
		font-size: 14px;
		line-height: 24px;
	}
</style>
