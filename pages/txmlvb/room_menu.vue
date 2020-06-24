<template>
	<view>
		<button type="primary" @click="getRoomList()">获取房间列表</button>
		<button type="primary" @click="onClickRoom()">进入房间</button>
		<button type="primary" @click="onClickPlay()">拉流</button>
	</view>
</template>

<script>
	//LVB插件对象
	var sdkwx = uni.requireNativePlugin('TXMLVB-LiveRoom');
	export default {
		data() {
			return {
				
			}
		},
		onUnload() {
			//退出登录
			sdkwx.logout();
		},
		methods: {
			getRoomList() {
				//获取房间列表
				var index = 0;//开始索引
				var count = 50;//房间个数
				sdkwx.getRoomList(index, count, function(res){
					console.log(res);
					if (res.code == 0) {
						//显示房间列表
						var roomInfos = res.data;
					}
				});
			},
			onClickRoom() {
				//进入直播
				uni.navigateTo({
					url:'/pages/txmlvb/room'
				});
			},
			onClickPlay() {
				//拉流
				uni.navigateTo({
					url:'/pages/txmlvb/player'
				});
			}
		}
	}
</script>

<style>

</style>
