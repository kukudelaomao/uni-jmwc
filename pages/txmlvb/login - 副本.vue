<template>
	<view>
		<button type="primary" @click="onClickLiveLogin()">主播直播登录</button>
		<button type="primary" @click="onClickPlayLogin()">观众观看登录</button>
	</view>
</template>

<script>
	//插件对象
	var sdk = uni.requireNativePlugin('TXMLVB-SdkWX');
	//LVB插件对象
	var sdkwx = uni.requireNativePlugin('TXMLVB-LiveRoom');
	export default {
		data() {
			return {
				uid: null
			}
		},
		onLoad() {
			//初始化
			var licenceURL = "http://license.vod2.myqcloud.com/license/v1/463ed367160e4904c4a319860c45f1e5/TXLiveSDK.licence";
			var licenceKey = "b8852a6c9f5ac5b6230663a5917f1ada";
			sdk.setLicence(licenceURL, licenceKey);
		},
		onShow() {
			this.uid = uni.getStorageSync('firmId')
		},
		methods: {
			onClickLiveLogin() {
				//主播登录
				var uid = "test15";
				var sig = "eJwtzE0LgkAUheH-MttC7h0ddIQWBWILyz6EaKnMJJcyBmcUI-rvibo8z4H3y4rs6vW6ZTHjHrD1tEnpt6MHTey0dYjLY9WzNIYUizEA8KMgRDk-ejDU6tGFEBwAZnXUTCY5DyHycalQPYbTygktV5APsiuCPKnNjk4NfirMkv6Mt-3Bpv7l*Oru2w37-QExXDD7";
				sdkwx.login({
					sdkAppID:"1400384719",//直播的appID
					userID:uid,//用户ID
					userName:"主播",//用户名称
					userAvatar:"",//用户头像
					userSig:sig //签名，参考腾讯官方文档
				}, function(res){
					console.log(res);
					if (res.code == 0) {
						console.log("login success");
						uni.navigateTo({
							url:'/pages/txmlvb/live_menu'
						});
					} else {
						console.log("login failed! ", "errCode：" + res.code + ", errMsg:" + res.data);
					}
				});
			},
			onClickPlayLogin() {
				//观众登录
				var uid = "weixiaokaqiusha";
				var sig = "eJwtzc0KgkAUBeB3mXXodXLUEVq0MaVSrAgCNwMz1sXS8acUondP1OX9zuHcL7kczsZHNcQn1ACymm6Uquwwx4l7hQOKqhA1vtuHWCqtLITWKIlv2QBrz3YtPidq0Nio0RljFABm7fA1GacUHIe7ywrexw-62mWmDKMcuRey3fZ0c-dVZvbHOmmqZzxEASRBmcYpT0rYkN8fSJE17w__";
				sdkwx.login({
					sdkAppID:"1400384719",//直播的appID
					userID:uid,//用户ID
					userName:"观众",//用户名称
					userAvatar:"",//用户头像
					userSig:sig //签名，参考腾讯官方文档
				}, function(res){
					if (res.code == 0) {
						console.log("login success");
						uni.navigateTo({
							url:'/pages/txmlvb/room_menu'
						});
					} else {
						console.log("login failed! ", "errCode：" + res.code + ", errMsg:" + res.data);
					}
				});
			},
			
		}
	}
</script>

<style>

</style>
