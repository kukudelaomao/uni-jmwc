<script>
// #ifdef APP-PLUS
import APPUpdate from "@/js_sdk/zhouWei-APPUpdate/APPUpdate.js";
// #endif
export default {
	globalData: {
		personalData: {
			firmId: null,
			userId: null,
			cellPhone: null,
			sessionStr: null,
			nickname: null,
			portrait: null
		}
	},  
	onLaunch: function() {
		console.log('App Launch');
		// #ifdef APP-PLUS
		// 检查APP版本，若不同则升级到最新版本
		APPUpdate();
		// #endif
	},
	onShow: function() {
		console.log('App Show');
	},
	onHide: function() {
		console.log('App Hide');
	}
};
</script>

<style>
/* 解决头条小程序组件内引入字体不生效的问题 */
/* #ifdef MP-TOUTIAO */
@font-face {
	font-family: uniicons;
	src: url('/static/uni.ttf');
}
/* #endif */
</style>
