<template>
	<!-- <waterfall column-count="2" column-width="auto" column-gap="12" left-gap="12" right-gap="12">
		    <view class="waterfall_cell" v-for="item in roomLists" :key="item.roomID" @click="onClickRoom(item)" >
				<image class="waterfall_cel_image" :src="item.pushers[0].userAvatar" mode="scaleToFill"></image>
				<view class="live_list_audienceCount">
					<text class="live_icon">◆ 直播中</text>
					<text class="live_people">{{item.audienceCount}}观看</text>
				</view>
				<view class="live_list_roomName">
					<text class="live_name">{{item.pushers[0].userName}}</text>
				</view>
		    </view>
			<button type="primary" size="mini" class="openLive" @click="onClickLiveLogin()">开直播</button>
		</waterfall> -->
	<view>
		<wfalls-flow :list="list" ref="wfalls" @finishLoad="getLoadNum"></wfalls-flow>
		<view v-if="list.length===0" class="no-live">{{blockTip}}</view>
		<button v-if="isStore" type="primary" size="mini" class="openLive" @click="onClickLiveOpen()">开直播</button>
	</view>
	
</template>

<script>
	//插件对象
	var sdk = uni.requireNativePlugin('TXMLVB-SdkWX');
	//LVB插件对象
	var sdkwx = uni.requireNativePlugin('TXMLVB-LiveRoom');
	import wfallsFlow from '@/components/wfalls-flow/wfalls-flow'
	import $httpLive from '@/js_sdk/zhouWei-request/requestConfigLiveRoom.js';
	// const list = require('@/static/wfalls-flow/data.json').list
	export default {
		data() {
			return {
				isStore: false,
				storeName: null,
				storeLogo: null,
				storeId: null,
				roomLists: [
					// {
					// 	"mixedPlayURL": "http://live.tjdami.com/live/1400384719_111031.flv",
					// 	"roomCreator": "111031",
					// 	"custom": {"hotDegree":99999},
					// 	"audienceCount": 0,
					// 	"pushers": [{
					// 		"userName": "Wei xiao",
					// 		"userID": "111031",
					// 		"userAvatar": "http://tcloud.tjdami.com:32223/uploadfile/1592300212788.jpeg"
					// 	}],
					// 	"roomID": "111031",
					// 	"roomInfo": {"roomAvatar":"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592817969337&di=271b9b9d32d4e72a62831087d117ef38&imgtype=0&src=http%3A%2F%2Fa3.att.hudong.com%2F16%2F18%2F300000932954129238184537620_950.jpg","liveTitle":"这是一个直播的标题 应该是够长的 不够长也不能太长","storeAvatar":"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592818014508&di=6e89966fbf68b2cd4af8a4742880bc0a&imgtype=0&src=http%3A%2F%2Fa3.att.hudong.com%2F53%2F88%2F01200000023787136831885695277.jpg","storeName":"这是一个有点长的店铺名称","storeId":"57"}
					// },
					// {
					// 	"mixedPlayURL": "http://live.tjdami.com/live/1400384719_111031.flv",
					// 	"roomCreator": "111031",
					// 	"custom": {"hotDegree":8888},
					// 	"audienceCount": 99999,
					// 	"pushers": [{
					// 		"userName": "Wei xiao",
					// 		"userID": "111031",
					// 		"userAvatar": "http://tcloud.tjdami.com:32223/uploadfile/1592300212788.jpeg"
					// 	}],
					// 	"roomID": "111031",
					// 	"roomInfo": {"roomAvatar":"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592817969337&di=271b9b9d32d4e72a62831087d117ef38&imgtype=0&src=http%3A%2F%2Fa3.att.hudong.com%2F16%2F18%2F300000932954129238184537620_950.jpg","liveTitle":"这是一个直播的标题 应该是够长的 不够长也不能太长","storeAvatar":"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592818014508&di=6e89966fbf68b2cd4af8a4742880bc0a&imgtype=0&src=http%3A%2F%2Fa3.att.hudong.com%2F53%2F88%2F01200000023787136831885695277.jpg","storeName":"这是一个有点长的店铺名称"}
					// },
					// {
					// 	"mixedPlayURL": "http://live.tjdami.com/live/1400384719_111031.flv",
					// 	"roomCreator": "111031",
					// 	"custom": {"hotDegree":777},
					// 	"audienceCount": 99999,
					// 	"pushers": [{
					// 		"userName": "Wei xiao",
					// 		"userID": "111031",
					// 		"userAvatar": "http://tcloud.tjdami.com:32223/uploadfile/1592300212788.jpeg"
					// 	}],
					// 	"roomID": "111031",
					// 	"roomInfo": {"roomAvatar":"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592817969337&di=271b9b9d32d4e72a62831087d117ef38&imgtype=0&src=http%3A%2F%2Fa3.att.hudong.com%2F16%2F18%2F300000932954129238184537620_950.jpg","liveTitle":"这是一个直播的标题 应该是够长的 不够长也不能太长","storeAvatar":"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592818014508&di=6e89966fbf68b2cd4af8a4742880bc0a&imgtype=0&src=http%3A%2F%2Fa3.att.hudong.com%2F53%2F88%2F01200000023787136831885695277.jpg","storeName":"这是一个有点长的店铺名称"}
					// },
					// {
					// 	"mixedPlayURL": "http://live.tjdami.com/live/1400384719_111031.flv",
					// 	"roomCreator": "111031",
					// 	"custom": {"hotDegree":66},
					// 	"audienceCount": 99999,
					// 	"pushers": [{
					// 		"userName": "Wei xiao",
					// 		"userID": "111031",
					// 		"userAvatar": "http://tcloud.tjdami.com:32223/uploadfile/1592300212788.jpeg"
					// 	}],
					// 	"roomID": "111031",
					// 	"roomInfo": {"roomAvatar":"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592817969337&di=271b9b9d32d4e72a62831087d117ef38&imgtype=0&src=http%3A%2F%2Fa3.att.hudong.com%2F16%2F18%2F300000932954129238184537620_950.jpg","liveTitle":"这是一个直播的标题 应该是够长的 不够长也不能太长","storeAvatar":"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592818014508&di=6e89966fbf68b2cd4af8a4742880bc0a&imgtype=0&src=http%3A%2F%2Fa3.att.hudong.com%2F53%2F88%2F01200000023787136831885695277.jpg","storeName":"这是一个有点长的店铺名称"}
					// },
					// {
					// 	"mixedPlayURL": "http://live.tjdami.com/live/1400384719_111031.flv",
					// 	"roomCreator": "111031",
					// 	"custom": {"hotDegree":5},
					// 	"audienceCount": 99999,
					// 	"pushers": [{
					// 		"userName": "Wei xiao",
					// 		"userID": "111031",
					// 		"userAvatar": "http://tcloud.tjdami.com:32223/uploadfile/1592300212788.jpeg"
					// 	}],
					// 	"roomID": "111031",
					// 	"roomInfo": {"roomAvatar":"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592817969337&di=271b9b9d32d4e72a62831087d117ef38&imgtype=0&src=http%3A%2F%2Fa3.att.hudong.com%2F16%2F18%2F300000932954129238184537620_950.jpg","liveTitle":"这是一个直播的标题 应该是够长的 不够长也不能太长","storeAvatar":"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592818014508&di=6e89966fbf68b2cd4af8a4742880bc0a&imgtype=0&src=http%3A%2F%2Fa3.att.hudong.com%2F53%2F88%2F01200000023787136831885695277.jpg","storeName":"这是一个有点长的店铺名称"}
					// }
				],
				list: [],
				isNewRenderDone: false ,//锁的作用
				listIndex: 0,
				listCount: 50,
				blockTip: "暂无直播"
			}
		},
		onLoad() {
			console.log("onLoad")
			//初始化
			var licenceURL = "http://license.vod2.myqcloud.com/license/v1/1b356763d5a20f7aeaa8bbc64506e3a8/TXLiveSDK.licence";
			var licenceKey = "eda5dff730338f1a1f0f1479d309070d";
			sdk.setLicence(licenceURL, licenceKey);
			this.onClickPlayLogin()
			this.getMyStore()
		},
		onShow() {
			
		},
		onReachBottom() {
			console.log('onReachBottom');
			// 加锁，避免在加载更多时用户频繁下拉导致的重复触发而渲染异常
			if (!this.isNewRenderDone) return;
			this.isNewRenderDone = false
			uni.showLoading({
				title: '正在加载更多'
			})
			console.log(this.listIndex, this.listCount)
			sdkwx.getRoomList(this.listIndex, this.listCount, res => {
				console.log(res);
				if (res.code == 0) {
					//显示房间列表
					for (let s of res.data) {
						s.roomInfo = JSON.parse(s.roomInfo)
						s.custom = JSON.parse(s.custom)
						s.audienceCount = s.audienceCount===undefined ? s.audiences.length : s.audienceCount
					}
					this.list = this.roomLists.concat(res.data);
					console.log(this.list)
					// APP上触发不了还是setTimeout万能
					setTimeout(() => {
						this.$refs.wfalls.handleViewRender();
					}, 0)
					this.listIndex = this.list.length
					console.log(this.listIndex, this.listCount)
				}
			});
		},
		onPullDownRefresh() {
			this.listIndex = 0
			console.log(this.listIndex, this.listCount)
			sdkwx.getRoomList(this.listIndex, this.listCount, res => {
				console.log(res);
				if (res.code == 0) {
					//显示房间列表
					for (let s of res.data) {
						s.roomInfo = JSON.parse(s.roomInfo)
						s.custom = JSON.parse(s.custom)
						s.audienceCount = s.audienceCount===undefined ? s.audiences.length : s.audienceCount
					}
					this.list = this.roomLists.concat(res.data);
					console.log(this.list)
					// this.$refs.wfalls.handleViewRender();
					this.$refs.wfalls.init()
					uni.stopPullDownRefresh()
					uni.showToast({
						title: '刷新成功',
						icon: 'none'
					})
					this.listIndex = this.list.length
					console.log(this.listIndex, this.listCount)
				}
			});
		},
		methods: {
			getMyStore(){
				let httpData = {
					firmId: uni.getStorageSync('firmId')
				};
				$httpLive.post("ishop-store-query/store/getMyStore", httpData,{
					isPrompt: false,
				}).then(res => {
					console.log(res)
					if (res) {
						if(res.value.content.length > 0 && res.value.content[0].applyStatusId=== 3){
							this.isStore = true
							this.storeLogo = res.value.content[0].logoUrl
							this.storeName = res.value.content[0].storeName
							this.storeId = res.value.content[0].storeId
							console.log(res.value.content[0])
							uni.setStorage({key: 'storeLogo', data: res.value.content[0].logoUrl});
							uni.setStorage({key: 'storeName', data: res.value.content[0].storeName});
							uni.setStorage({key: 'storeId', data: res.value.content[0].storeId});
						}else{
							this.isStore = false
						}
					} else {
						uni.showToast({
							title: res.message,
							icon: "none"
						});
					}
				});
			},
			onClickLiveOpen() {
				// 开直播
				uni.navigateTo({
					url: '/pages/txmlvb/live_menu'
				});
			},
			onClickPlayLogin() {
				//观众登录
				let httpData = {};
				this.blockTip = '正在登录直播系统...'
				$httpLive.post("live/IMCommunication/getImSig", httpData,{
					isPrompt: false,
				}).then(res => {
					console.log(res)
					if (res) {
						const sig = res.value
						sdkwx.login({
							sdkAppID: "1400399301", //直播的appID
							userID: uni.getStorageSync('firmId'), //用户ID
							userName: uni.getStorageSync('nickname'), //用户名称
							userAvatar: uni.getStorageSync('portrait'), //用户头像
							userSig: sig //签名，参考腾讯官方文档
						}, resL => {
							if (resL.code == 0) {
								console.log("login success",uni.getStorageSync('firmId'),uni.getStorageSync('nickname'));
								// console.log(resL);
								this.blockTip = '正在获取房间列表...'
								sdkwx.getRoomList(this.listIndex, this.listCount, resRL => {
									console.log(resRL);
									if (resRL.code == 0) {
										this.blockTip = '暂无直播'
										//显示房间列表
										for (let s of resRL.data) {
											s.roomInfo = JSON.parse(s.roomInfo)
											s.custom = JSON.parse(s.custom)
											s.audienceCount = s.audienceCount===undefined ? s.audiences.length : s.audienceCount
										}
										this.list = this.roomLists.concat(resRL.data);
										console.log(this.list)
										this.$refs.wfalls.init();
										this.listIndex = this.list.length
										console.log(this.listIndex, this.listCount)
									} else {
										this.blockTip = '暂无直播'
										console.log("login failed! ", "errCode：" + resRL.code + ", errMsg:" + resRL.data);
										uni.showToast({
											title: '获取房间列表失败：' + resRL.code,
											icon: "none"
										});
									}
								});
								
							} else {
								this.blockTip = '暂无直播'
								console.log("login failed! ", "errCode：" + resL.code + ", errMsg:" + resL.data);
								uni.showToast({
									title: '登录直播系统失败：' + resL.code,
									icon: "none"
								});
							}
						});
					} else {
						this.blockTip = '暂无直播'
						uni.showToast({
							title: '获取登录签名失败',
							icon: "none"
						});
					}
				});
			},
			getLoadNum(num) {
				console.log('共加载了:' + num);
				!this.isNewRenderDone && uni.hideLoading()
				this.isNewRenderDone = true
			}
		}
	}
</script>

<style>
	.openLive {
		position: fixed;
		bottom: 100upx;
		right: 10upx;
		width: 180upx;
		border-radius: 50upx;
		border-color: #31b630 !important;
		background-color: #31b630 !important;
	}

	.no-live{
		width: 100%;
		padding-top: 50%;
		color: #c0c0c0;
		text-align: center;
	}
	.waterfall_cell {
		padding: 30upx 0upx;
	}

	.waterfall_cel_image {
		border-radius: 20upx;
	}

	.live_list_audienceCount {
		position: absolute;
		top: 100upx;
		left: 50upx;
		color: #FFFFFF;
	}

	.live_icon {
		background-color: #31b630;
		color: #FFFFFF;
		text-align: center;
		position: relative;
		width: 180upx;
		font-size: 32upx;
	}

	.live_people {
		background-color: rgba(100, 100, 100, 0.3);
		color: #FFFFFF;
		text-align: right;
		position: relative;
		width: 180upx;
		font-size: 32upx;
	}

	.live_list_roomName {
		padding-top: 20upx;
	}

	.live_name {
		text-align: center;
		font-size: 32upx;
		lines: 1;
		overflow: hidden;
		text-overflow: ellipsis;
	}
</style>
