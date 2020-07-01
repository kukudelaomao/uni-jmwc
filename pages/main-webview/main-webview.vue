<template>
	<view class="container">
		<web-view :src="url" @message="handleMessage"></web-view>
	</view>
</template>

<script>
	// MobTech引入
	// const modal = uni.requireNativePlugin('modal');
	// const pluginMobShare = uni.requireNativePlugin('mob-sharesdk');
	export default {
		data() {
			return {
				url: '/hybrid/html/index.html',
				// url: 'http://www.baidu.com'
			}
		},
		// onLoad() {
		// 	// 创建加载内容窗口  
		// 	var topoffset='45px';  
		// 	if(plus.navigator.isImmersedStatusbar()){// 兼容immersed状态栏模式  
		// 		// 获取状态栏高度并根据业务需求处理，这里重新计算了子窗口的偏移位置  
		// 		topoffset=(Math.round(plus.navigator.getStatusbarHeight())+45)+'px';  
		// 	}  
		// 	// 使用偏移位置创建子窗口  
		// 	// wc=plus.webview.create(null,'doccontent',{top:topoffset,bottom:'0px',bounce:'vertical',bounceBackground:'#FFFFFF'});
		// 	plus.webview.currentWebview().setStyle( {top:topoffset,bottom: '0px'} );
		// }
		onReady() {
			// #ifdef APP-PLUS
			var pages = getCurrentPages();
			var page = pages[pages.length - 1];
			var currentWebview = page.$getAppWebview(); //页面栈最顶层就是当前webview  
			// 获取webview的children，有了子webview对象后，可以用5+丰富的api来进行前进、后退、拦截资源、禁用schema跳转、注入js等各种操作。具体参考https://www.html5plus.org/doc/zh_cn/webview.html
			var currentWebviewChild = currentWebview.children()
			console.log(currentWebviewChild[0].getURL())
			
			// 不太好的广告界面
			// let view = new plus.nativeObj.View('test',
			// 	{top:'0px',left:'0px',height:'100%',width:'100%'},
			// 	[
			// 		{tag:'img',id:'imgBg',src:'static/colorBg.png',position:{top:'0',left:'0',width:'100%',height:'100%'}},
			// 		{tag:'img',id:'img',src:'static/jmwc.png',position:{top:'30%',left:'25%',width:'50%',height:'auto'}},
			// 		{tag:'img',id:'imgSec',src:'static/second3.gif',position:{top:'10px',right:'10px',height:'44px',width:'44px'}}
			// 		// {tag:'rect',id:'rect',color:'#FF0000',position:{top:'0px',left:'0px',width:'100%',height:'1px'}},
			// 		// {tag:'font',id:'font',text:'原生控件',textStyles:{size:'18px'},}
			// 	]);
			// view.show();
			// setTimeout(()=>{
			// 	view.close();
			// }, 3000)
			
			// 使用偏移位置调整状态栏样式
			// const currentWebview = this.$scope.$getAppWebview() //此对象相当于html5plus里的plus.webview.currentWebview()。在uni-app里vue页面直接使用plus.webview.currentWebview()无效，非v3编译模式使用this.$mp.page.$getAppWebview()
			// var topoffset='45px';
			// if(plus.navigator.isImmersedStatusbar()){
			// 	// 兼容immersed状态栏模式，获取状态栏高度并根据业务需求处理，这里重新计算了子窗口的偏移位置  
			// 	topoffset=(Math.round(plus.navigator.getStatusbarHeight()));  
			// }
			// setTimeout( () => {
			// 	const wv = currentWebview.children()[0]
			// 	wv.setStyle({top:topoffset,bottom:0})
			// }, 1000); //如果是页面初始化调用时，需要延时一下
			
			// loading图标
			// var w = plus.nativeUI.showWaiting()
			// currentWebviewChild[0].addEventListener('loaded', () => {
			// 	console.log('New Window loaded!');
			// 	currentWebviewChild[0].show(); // 显示窗口
			// 	w.close();
			// 	w = null;
			// }, false);
			
			// 拦截所有页面跳转，可使用参数拦截weibo.com域名之外的跳转（{mode:'allow',match:'.*weibo\.com/.*'}）
			// currentWebviewChild[0].overrideUrlLoading({
			// 	mode: 'reject',
			// 	match: '.*baidu\.com/.*'
			// }, function(e) {
			// 	console.log('reject url: ' + e.url);
			// });
			// #endif
		},
		onLoad:function(option){
			console.log(option)
			if(option && option.goodsId!==undefined){
				this.url = '/hybrid/html/index.html#/malldetail?goodsId=' + encodeURIComponent(option.goodsId)
				console.log(this.url)
			}
		},
		methods: {
			handleMessage(evt) {
				console.log('接收到的消息：' + JSON.stringify(evt.detail.data));
				const evtData = evt.detail.data[0]
				// 获取webview方法一
				// #ifdef APP-PLUS  
				// const currentWebview = this.$scope.$getAppWebview() //此对象相当于html5plus里的plus.webview.currentWebview()。在uni-app里vue页面直接使用plus.webview.currentWebview()无效，非v3编译模式使用this.$mp.page.$getAppWebview()
				// #endif
				// 获取webview方法二
				// var pages = getCurrentPages();  
				// var page = pages[pages.length - 1];  
				// #ifdef APP-PLUS  
				// var currentWebview = page.$getAppWebview(); //页面栈最顶层就是当前webview  
				// #endif
				// 获取webview的children，有了子webview对象后，可以用5+丰富的api来进行前进、后退、拦截资源、禁用schema跳转、注入js等各种操作。具体参考https://www.html5plus.org/doc/zh_cn/webview.html
				// var currentWebviewChild = currentWebview.children()
				// console.log(currentWebviewChild[0].getURL())

				// MobTech Share
				// this.submitPrivacyGrantResult();

				// share
				// this.formSubmit()

				switch (evtData.type) {
					// 生成二维码海报并分享
					case 'make-share-qrcode':
						uni.navigateTo({
							url: '/pages/make-share-qrcode/make-share-qrcode?qrcode=' + evtData.qrcode
						});
						break;
					// 扫描二维码并返回给webview
					case 'scan-qrcode':
						uni.scanCode({
							success: res => {
								console.log(JSON.stringify(res))
								console.log('条码内容：' + res.result)
								const urlHashArray = evtData.hash.split('?')
								if (urlHashArray.length === 1) {
									this.url = '/hybrid/html/index.html' + urlHashArray[0] + '?unidata=' + encodeURIComponent(res.result)
								} else {
									if (urlHashArray[1].indexOf('unidata') !== -1) {
										const tmp = urlHashArray[1].split('unidata=')
										this.url = '/hybrid/html/index.html' + urlHashArray[0] + '?' + tmp[0] + 'unidata=' + encodeURIComponent(
											res.result)
									} else {
										this.url = '/hybrid/html/index.html' + urlHashArray[0] + '?' + urlHashArray[1] + '&unidata=' +
											encodeURIComponent(res.result)
									}
								}
							},
							fail: res => {
								console.log('扫码失败')
							}
						})
						break;
					// 专门用于访问外部链接的webview，方便返回主程序界面
					case 'open-new-page':
						uni.navigateTo({
							url: '/pages/new-webview/new-webview?newUrl=' + evtData.newUrl
						});
						break
					case 'look-online-video':
						uni.navigateTo({
							url:'../tencent-live-video/la/index'
						});
						break
					case 'push-online-video':
						uni.navigateTo({
							// url: '/pages/new-webview/new-webview?newUrl=' + evtData.newUrl,
							url:'../tencent-live-video/openZhibo/index'
						});
						break
					case 'TXMLVB-video':
						console.log(evtData)
						// getApp().globalData.personalData.firmId = evtData.firmId
						// getApp().globalData.personalData.sessionStr = evtData.sessionStr
						// getApp().globalData.personalData.nickname = evtData.nickname
						// getApp().globalData.personalData.portrait = evtData.portrait
						// getApp().globalData.personalData.userId = evtData.userId
						// getApp().globalData.personalData.cellPhone = evtData.cellPhone
						uni.setStorage({key: 'firmId', data: evtData.firmId});
						uni.setStorage({key: 'sessionStr', data: evtData.sessionStr});
						uni.setStorage({key: 'nickname', data: evtData.nickname});
						uni.setStorage({key: 'portrait', data: evtData.portrait});
						uni.navigateTo({
							url:'../txmlvb/login'
						});
						break
					default:
						break;
				}
			},
			// MobTech回传用户授权结果
			// isurl参数如果传入字符串‘true’则表示接受隐私协议
			// submitPrivacyGrantResult() {
			// 	pluginMobShare.submitPrivacyGrantResult({
			// 		isUrl: 'true',
			// 		mobAppkey: '2f18c7d86d960',
			// 		mobSecret: '9d203e1b7a86441d6969976712a2a1e5',
			// 	}, result => {
			// 		const msg = JSON.stringify(result);
			// 		modal.toast({
			// 			message: msg,
			// 			duration: 8,
			// 		});
			// 		this.mobShareWebpage();
			// 	});
			// },
			// MobTech分享
			// mobShareWebpage() {
			// 	pluginMobShare.generalShare({
			// 		platName: '22',
			// 		mobAppkey: '2f18c7d86d960',
			// 		mobSecret: '9d203e1b7a86441d6969976712a2a1e5',
			// 		text: 'text',
			// 		title: "title",
			// 		url: 'http://wiki.mob.com/',
			// 		imageUrl: "http://img1.2345.com/duoteimg/qqTxImg/2012/04/09/13339485237265.jpg",
			// 		shareType: 3,
			// 	}, result => {
			// 		const msg = JSON.stringify(result);
			// 		modal.toast({
			// 			message: msg,
			// 			duration: 8,
			// 		});
			// 		switch (result.type) {
			// 			case 'onComplete':
			// 				console.log("callback---onComplete--" + result.onCompleteResult);
			// 				break;
			// 			case 'onError':
			// 				// console.log("callback---onError--" + result.onErrorResult);
			// 				console.log("callback---onError--" + JSON.stringify(result));
			// 				break;
			// 			case 'onCancel':
			// 				console.log("callback---onCancel--");
			// 				break;
			// 		}
			// 	});

			// },
			// uni内置分享
			// formSubmit: function(e) {
			// 	uni.share({
			// 		provider: "weixin",
			// 		scene: "WXSceneSession",
			// 		type: 2,
			// 		imageUrl: "https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png",
			// 		success: function(res) {
			// 			console.log("success:" + JSON.stringify(res));
			// 		},
			// 		fail: function(err) {
			// 			console.log("fail:" + JSON.stringify(err));
			// 		}
			// 	});
			// },
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
