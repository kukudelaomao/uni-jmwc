<template>
	<view class="main">
	</view>
</template>

<script>
	export default {
		data() {
			return {
				guidePages: true
			}
		},
		onLoad() {
			//#ifdef APP-PLUS
			this.checkGuide();
			// #endif
			//#ifndef APP-PLUS
			uni.redirectTo({
				url: '/pages/main-webview/main-webview'
			});
			// #endif
		},
		methods: {
			checkGuide: function() {
				/**
				 * 获取本地存储中launchFlag的值
				 * 若存在，说明不是首次启动，直接进入首页；
				 * 若不存在，说明是首次启动，进入引导页；
				 */
				console.log("进入checkGuide判断页");
				try {
					// 获取本地存储中launchFlag标识
					var launchFlag = uni.getStorageSync('launchFlag');
					console.log("launchFlag:" + launchFlag);
					if (launchFlag) { 
						//判断本地缓存跳转首页
						uni.redirectTo({
							url: '/pages/main-webview/main-webview'
						});
					} else { 
						//没有缓存进入启动页引导页
						console.log('去引导页');
						uni.redirectTo({
							url: '/pages/guide-pages/guide-pages'
						});
					}

				} catch (e) {
					// error 
					uni.setStorage({
						key: 'launchFlag',
						data: true,
						success: function() {
							console.log('error时存储launchFlag');
						}
					});
				}
			}
		}
	}
</script>

<style>
	page,
	.main {
		width: 100%;
		height: 100%;
	}
</style>
