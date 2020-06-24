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
		<button v-if="isStore" type="primary" size="mini" class="openLive" @click="onClickLiveLogin()">开直播</button>
	</view>
	
	<!-- <button type="primary" @click="onClickPlayLogin()">观众观看登录</button> -->
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
					{
						"mixedPlayURL": "http://live.tjdami.com/live/1400384719_111031.flv",
						"roomCreator": "111031",
						"custom": "{}",
						"audienceCount": 99999,
						"pushers": [{
							"userName": "Wei xiao",
							"userID": "111031",
							"userAvatar": "http://tcloud.tjdami.com:32223/uploadfile/1592300212788.jpeg"
						}],
						"roomID": "111031",
						"roomInfo": {"roomAvatar":"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592817969337&di=271b9b9d32d4e72a62831087d117ef38&imgtype=0&src=http%3A%2F%2Fa3.att.hudong.com%2F16%2F18%2F300000932954129238184537620_950.jpg","liveTitle":"这是一个直播的标题 应该是够长的 不够长也不能太长","storeAvatar":"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592818014508&di=6e89966fbf68b2cd4af8a4742880bc0a&imgtype=0&src=http%3A%2F%2Fa3.att.hudong.com%2F53%2F88%2F01200000023787136831885695277.jpg","storeName":"这是一个有点长的店铺名称","storeId":"57"}
					},
					// {
					// 	"mixedPlayURL": "http://live.tjdami.com/live/1400384719_111031.flv",
					// 	"roomCreator": "111031",
					// 	"custom": "{}",
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
					// 	"custom": "{}",
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
					// 	"custom": "{}",
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
					// 	"custom": "{}",
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
				page: 0,
				count: 5
			}
		},
		onLoad() {
			console.log("onLoad")
			//初始化
			var licenceURL = "http://license.vod2.myqcloud.com/license/v1/463ed367160e4904c4a319860c45f1e5/TXLiveSDK.licence";
			var licenceKey = "b8852a6c9f5ac5b6230663a5917f1ada";
			sdk.setLicence(licenceURL, licenceKey);
			this.onClickPlayLogin()
			this.getMyStore()
			
			// // 模拟首次加载列表数据
			// setTimeout(() => {
			// 	this.list = this.roomLists
			// 	this.$refs.wfalls.init();
			// }, 1000)
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
			sdkwx.getRoomList(this.page, this.count, res => {
				console.log(res);
				if (res.code == 0) {
					//显示房间列表
					for (let s of res.data) {
						s.roomInfo = JSON.parse(s.roomInfo)
						s.audienceCount = s.audienceCount===undefined ? s.audiences.length : s.audienceCount
						// s.roomInfo.roomAvatar = "http://5b0988e595225.cdn.sohucs.com/images/20180921/58ad30d1a5f741f6a8dc4b81820a0c50.jpeg"
					}
					this.list = this.roomLists.concat(res.data);
					console.log(this.list)
					// APP上触发不了还是setTimeout万能
					setTimeout(() => {
						this.$refs.wfalls.handleViewRender();
					}, 0)
					this.page = this.page + this.count
					// setTimeout(() => {
					// 	this.$refs.wfalls.init();
					// }, 0)
					// this.roomLists.pushers = JSON.parse(res.data.pushers)
				}
			});
			// 模拟分页请求 (加载更多)
			// setTimeout(() => {
			// 	const nextData = JSON.parse(JSON.stringify(this.list.slice(0, 10)))
			// 	this.list.push(...nextData);
			// 	// this.$nextTick(()=>{
			// 	//     this.$refs.wfalls.handleViewRender();
			// 	// })
			// 	// APP上触发不了还是setTimeout万能
			// 	setTimeout(() => {
			// 		this.$refs.wfalls.handleViewRender();
			// 	}, 0)
			// }, 800)
		},
		onPullDownRefresh() {
			this.page = 0
			sdkwx.getRoomList(this.page, this.count, res => {
				console.log(res);
				if (res.code == 0) {
					//显示房间列表
					for (let s of res.data) {
						s.roomInfo = JSON.parse(s.roomInfo)
						s.audienceCount = s.audienceCount===undefined ? s.audiences.length : s.audienceCount
						// s.roomInfo.roomAvatar = "http://5b0988e595225.cdn.sohucs.com/images/20180921/58ad30d1a5f741f6a8dc4b81820a0c50.jpeg"
					}
					this.list = this.roomLists.concat(res.data);
					console.log(this.list)
					this.$refs.wfalls.init()
					uni.stopPullDownRefresh()
					uni.showToast({
						title: '刷新成功',
						icon: 'none'
					})
					this.page = this.page + this.count
					// setTimeout(() => {
					// 	this.$refs.wfalls.init();
					// }, 0)
					// this.roomLists.pushers = JSON.parse(res.data.pushers)
				}
			});
			// 模拟更新新数据
			const newData = JSON.parse(JSON.stringify(this.list.slice(0, 10)))
			setTimeout(() => {
				this.list = newData;
				this.$refs.wfalls.init()
				uni.stopPullDownRefresh()
				uni.showToast({
					title: '刷新成功',
					icon: 'none'
				})
			}, 800)
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
			onClickLiveLogin() {
				//主播登录
				// var uid = "test15";
				// var sig =
				// 	"eJwtzEELgjAYxvHvsnPINn1bEzpISSGDDllEt2BTX6QUt0yNvnuiHp-fA-8vSdXZa01DQsI9SlbTRm1eDjOc2BnrGCyP1eWjrlGTkAWU*ptAMDk-pquxMaMDAKeUzurwOZnkPgix5ksF8zFs42N1T*yn07xIM1WKVu3j7HLt4V308jRUyRDdZHSAXb4lvz9dzjIc";
				// sdkwx.login({
				// 	sdkAppID: "1400384719", //直播的appID
				// 	userID: uid, //用户ID
				// 	userName: "主播", //用户名称
				// 	userAvatar: "", //用户头像
				// 	userSig: sig //签名，参考腾讯官方文档
				// }, function(res) {
				// 	console.log(res);
				// 	if (res.code == 0) {
				// 		console.log("zhubo login success");
				// 		console.log(res);
				// 		uni.navigateTo({
				// 			url: '/pages/txmlvb/live_menu'
				// 		});
				// 	} else {
				// 		console.log("login failed! ", "errCode：" + res.code + ", errMsg:" + res.data);
				// 	}
				// });
				// permision.judgeIosPermission("camera") //判断iOS上是否给予摄像头权限，有权限返回true，否则返回false
				// permision.judgeIosPermission("record") //判断iOS上是否给予麦克风权限，有权限返回true，否则返回false
				uni.navigateTo({
					url: '/pages/txmlvb/live_menu'
				});
			},
			onClickPlayLogin() {
				//观众登录
				let httpData = {};
				$httpLive.post("live/IMCommunication/getImSig", httpData,{
					isPrompt: false,
				}).then(res => {
					console.log(res)
					if (res) {
						const sig = res.value
						// var sig =
						// 	"eJwtzNEKgjAYhuF72Wkh-*Y2p9ANlB6URnhYbM0-NYdJGtG9J9PD7-ng-ZIizYO36UlCWABk6zdq8xzwjp7HibLVX7q*OoeaJJQDhIpHNF4eMznszexCCAYAiw7YeosZj0KuxFpBO2c-7eZWpA23VWlKBfsaanl6nO1o8*aYCdlVnbSZvvQHuyO-P8gHMRQ_";
						sdkwx.login({
							sdkAppID: "1400384719", //直播的appID
							userID: uni.getStorageSync('firmId'), //用户ID
							userName: uni.getStorageSync('nickname'), //用户名称
							userAvatar: uni.getStorageSync('portrait'), //用户头像
							userSig: sig //签名，参考腾讯官方文档
						}, res => {
							if (res.code == 0) {
								console.log("login success",uni.getStorageSync('firmId'),uni.getStorageSync('nickname'));
								console.log(res);
								this.getRoomList(this.page, this.count)
							} else {
								console.log("login failed! ", "errCode：" + res.code + ", errMsg:" + res.data);
								uni.showToast({
									title: res.data,
									icon: "none"
								});
							}
						});
					} else {
						uni.showToast({
							title: res.message,
							icon: "none"
						});
					}
				});
			},
			getRoomList(index, count) {
				//获取房间列表
				// var index = 0; //开始索引
				// var count = 5; //房间个数
				sdkwx.getRoomList(index, count, res => {
					console.log(res);
					if (res.code == 0) {
						//显示房间列表
						for (let s of res.data) {
							s.roomInfo = JSON.parse(s.roomInfo)
							s.audienceCount = s.audienceCount===undefined ? s.audiences.length : s.audienceCount
							// s.roomInfo.roomAvatar = "http://5b0988e595225.cdn.sohucs.com/images/20180921/58ad30d1a5f741f6a8dc4b81820a0c50.jpeg"
						}
						this.list = this.roomLists.concat(res.data);
						console.log(this.list)
						this.$refs.wfalls.init();
						this.page = index + count
						// setTimeout(() => {
						// 	this.$refs.wfalls.init();
						// }, 0)
						// this.roomLists.pushers = JSON.parse(res.data.pushers)
					}
				});
			},
			// onClickRoom(item) {
			// 	//进入直播
			// 	uni.navigateTo({
			// 		url: '/pages/txmlvb/room?mixedPlayURL=' + item.mixedPlayURL + "&roomID=" + item.roomID
			// 	});
			// },
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
