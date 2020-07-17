<template>
	<view>
		<view class="top-box">
			<view class="top-title">发起直播</view>
			<view class="top-cont">
				<view class="top-cont-one" @click="changeImage">
					<image :src="roomAvatar"></image>
					<view class="top-cont-one-but">更换封面</view>
				</view>
				<textarea class="title" :value="liveTitle" maxlength="20" placeholder="请输入直播标题(20字以内)" @input="titleInput"/>
			</view>
		</view>
		<view @click="open" class="but">发起直播</view>
	</view>
</template>

<script>
	import $httpLive from '@/js_sdk/zhouWei-request/requestConfigLiveRoom.js';
	import permision from "@/js_sdk/wa-permission/permission.js"
	export default{
		data(){
			return{
				// uploadLoading: false,
				liveTitle: null,
				// userID: uni.getStorageSync('firmId'), //用户ID
				// userName: uni.getStorageSync('nickname'), //用户名称
				roomAvatar: uni.getStorageSync('portrait') //用户头像
			}
		},

		methods:{
			// async requestAndroidPermission(permisionID) {
			//     var result = await permision.requestAndroidPermission(permisionID)
			//     var strStatus
			//     if (result == 1) {
			//         strStatus = "已获得授权"
			//     } else if (result == 0) {
			//         strStatus = "未获得授权"
			//     } else {
			//         strStatus = "被永久拒绝权限"
			//     }
			// 	console.log(permisionID + strStatus)
			// },
			async open(){
				let camera = ''
				let audio = ''
				switch(uni.getSystemInfoSync().platform){
				    case 'android':
				       console.log('运行Android上')
					   camera = await permision.requestAndroidPermission('android.permission.CAMERA')
					   audio = await permision.requestAndroidPermission('android.permission.RECORD_AUDIO')
					   console.log(camera,audio)
					   if(camera!==1 || audio!==1){
					   	uni.showToast({
					   		title: "开直播需获取设备摄像头和音频权限",
					   		icon: "none",
					   		duration: 3000
					   	});
					   	return
					   }
				       break;
				    case 'ios':
				       console.log('运行iOS上')
					   // camera = await permision.judgeIosPermission("camera")
					   // audio = await permision.judgeIosPermission("record")
					   // console.log(camera,audio)
					   // if(camera!==1 || audio!==1){
					   // 	uni.showModal({
					   // 	        content: "开直播需获取设备摄像头和音频权限",
					   // 	        confirmText: "去设置",
								// success: function (res) {
								//         if (res.confirm) {
								//             console.log('用户点击确定');
								// 			permision.gotoAppPermissionSetting()
								//         } else if (res.cancel) {
								//             console.log('用户点击取消');
								//         }
								//     }
					   // 	    });
					   // 	return
					   // }
				       break;
				    default:
				       console.log('运行在开发者工具上')
				       break;
				}
				if(this.liveTitle === null || this.liveTitle === ''){
					uni.showToast({
						title: "请输入直播标题",
						icon: "none"
					});
					return
				}
				//开始直播
				uni.navigateTo({
					url:'/pages/txmlvb/live_room?liveTitle=' + this.liveTitle + "&roomAvatar=" + this.roomAvatar
				});
			},
			titleInput(event) {
				this.liveTitle = event.detail.value
			},
			changeImage(){
				// $httpLive.urlImgUpload('upload/upload',{
				// 	count:1,//默认 9
				// 	onEachUpdate: res => {
				// 		console.log("单张上传成功返回：",res);
				// 	},
				// 	onProgressUpdate: res => {
				// 		console.log("上传进度返回：",res);
				// 	}
				// },{
				// 	isPrompt: true,//（默认 true 说明：本接口抛出的错误是否提示）
				// 	load: true,//（默认 true 说明：本接口是否提示加载动画）
				// 	// headers: { //默认 无 说明：请求头
				// 	// 	'Content-Type': 'application/json'
				// 	// },
				// 	isFactory: true, //（默认 true 说明：本接口是否调用公共的数据处理方法，设置false后isPrompt参数奖失去作用）
				// 	maxSize: 300000 //（默认 无 说明：上传的文件最大字节数限制，默认不限制）
				// }).then(res => {
				// 	console.log("全部上传完返回结果：",res);
				// });
				uni.chooseImage({
					count: 1, //最多可以选择的图片张数，默认9
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album', 'camera'], //album 从相册选图，camera 使用相机，默认二者都有。如需直接开相机或直接选相册，请只使用一个选项
					success: (chooseImageRes) => {
						this.uploadImage(chooseImageRes.tempFilePaths)
					}
				})
			},
			uploadImage(tempFilePaths){
				uni.showLoading({
				    title: '上传中...'
				});
				uni.uploadFile({
					url: 'http://192.168.1.195:32223/apigateway/upload/upload',
					// url: "http://222.174.16.30:32222/apigateway/upload/upload",
					filePath: tempFilePaths[0],
					name: 'file',
					// formData: {
					// 	'user': 'test'
					// },
					success: (uploadFileRes) => {
						console.log(uploadFileRes);
						this.roomAvatar = JSON.parse(uploadFileRes.data).value
						console.log(this.roomAvatar)
					},
					complete: () => {
						uni.hideLoading();
					}
				});
			}
		}
	}
</script>

<style>
	.top-box{
		width: 80%;
		border-radius: 20upx;
		background: rgba(193, 193, 193, 0.4);
		padding: 40upx 30upx;
		margin: 0 auto;
		margin-top: 70upx;
	}
	.but{
		position: fixed;
		bottom: 50upx;
		width: 690upx;
		border-radius: 40upx;
		padding-top: 20upx;
		padding-bottom: 20upx;
		text-align: center;
		font-size: 30upx;
		color: white;
		margin-left: 30upx;
		background: #31b630;
	}
	.top-title{
		color: black;
		font-size: 34upx;
		margin-bottom: 20upx;
	}
	.top-cont{
		padding-top: 20upx;
		display: flex;
		align-items: flex-start;
	}
	.top-cont-one{
		position: relative;
		width: 200upx;
		height: 200upx;
		display: flex;
		align-items: center;
		justify-content: flex-end;
		flex-direction: column;
	}
	.top-cont-one>image{
		width: 200upx;
		height: 200upx;
		position: absolute;
		top: 0upx;
	}
	.top-cont-one-but{
		width: 200upx;
		position: relative;
		z-index: 1;
		background: rgba(0,0,0,.2);
		color: white;
		padding-top: 10upx;
		padding-bottom: 10upx;
		text-align: center;
		font-size: 24upx;
	}
	.title{
		padding-left: 20upx;
		color: black;
		font-size: 30upx;
		height: 200upx;
	}
</style>
