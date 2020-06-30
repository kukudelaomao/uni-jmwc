<template>
	<!-- 这是商品弹窗 -->
	<view class="shop-list-box" style="width: 100%;">
		<text style="width: 100%;" class="shop-mark-title">直播商品({{totalElements}})</text>
		<!-- <text class="add-shop">添加商品</text> -->
		<scroll-view style="width: 100%;" :scroll-top="scrollTop" scroll-y="true" class="scroll-Ys">
			<view class="shop-list-list" v-for="(it,i) in shopArr" :key='i'>
				<view>
					<image class="shop-list-img" :src="it.goodsGroupCoverImageUrl"></image>
					<uni-badge class="img-badge" :text="shopArr.length-i"></uni-badge>
				</view>
				<view class="shop-text-view">
					<text class="shop-list-title">{{it.goodsGroupName}}</text>
					<text class="shop-list-num">已售{{it.goodsGroupSalesAmount}}件</text>
					<text class="shop-list-price">￥{{it.minProductPrice}}</text>
					<text v-if="isStore==='notStore'" class="shop-list-btn" @click="gotoDetail(it)">去看看</text>
					<text v-else class="shop-list-zhutuiBtn" @click="setPrimary(it)">设为主推</text>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	import $httpLive from '@/js_sdk/zhouWei-request/requestConfigLiveRoom.js';
	import uniBadge from "@/components/uni-badge/uni-badge.vue"
	export default {
		components: {uniBadge},
		props: {
			storeId: {
				type: [String, Number], //实际请求获取的列表数据
				default: ''
			},
			isStore: {
				type: String, //实际请求获取的列表数据
				default: 'notStore'
			}
		},
		data() {
			return {
				scrollTop: 0,
				totalElements: 0,
				shopArr: []
			}
		},
		mounted() {
			// setTimeout(() => {
			// 	this.scrollToBottom()
			// }, 1000)
			this.getGoodsList()
		},
		updated() {
			// this.scrollToBottom()
		},
		methods: {
			gotoDetail(it){
				uni.navigateTo({
					url: '/pages/main-webview/main-webview?goodsId=' + it.goodsGroupId
					// url: '/pages/main-webview/main-webview?goodsId=55',
				})
			},
			setPrimary(it){
				this.$emit('setPrimary', it)
			},
			getGoodsList(){
				console.log(this.storeId)
				let httpData = {
					page: 0,
					size: 20,
					storeId: this.storeId
				};
				$httpLive.post("ishop-store-query/store/getStoreGoodsGroupList", httpData,{
					isPrompt: false
				}).then(res => {
					console.log(res)
					if (res && res.code === 0) {
						this.shopArr = res.value.content
						this.totalElements = res.value.totalElements
					} else {
						uni.showToast({
							title: "res.message",
							icon: "none"
						});
					}
				});
			}
		}
	}
</script>

<style>
	.scroll-Ys {
		background-color: white;
		height: 650rpx;
		width: 100%;
	}
	
	.shop-mark-title {
		padding: 40rpx 40rpx;
		text-align: left;
		font-size: 32rpx;
	}
	
	.img-badge {
		position: absolute;
		left: 35rpx;
		top: 5rpx;
	}
	.shop-list-img {
		margin: 0rpx 30rpx;
		width: 240rpx;
		height: 240rpx;
		border-radius: 30rpx;
		overflow: hidden;
		/* background-color: #000000; */
	}
	
	.shop-text-view{
		height: 240rpx;
	}
	
	.shop-list-title {
		width: 430rpx;
		font-size: 30rpx;
		color: #333333;
		lines: 2;
		overflow: hidden;
		text-overflow: ellipsis;  /* 超出部分省略号 */
	}
	
	.shop-list-num {
		font-size: 28rpx;
		color: #C0C0C0;
		padding-top: 10rpx;
		position: absolute;
		bottom: 60rpx;
	}
	
	.shop-list-price {
		font-size: 38rpx;
		color: #FD7400;
		position: absolute;
		bottom: 0rpx;
	}
	
	.shop-list-btn{
		position: absolute;
		bottom: 0rpx;
		right: 0rpx;
		width: 140rpx;
		height: 60rpx;
		font-size: 30rpx;
		line-height: 64rpx;
		text-align: center;
		color: white;
		background-color: #FD7400;
		border-radius: 30upx;
		border-color: #FD7400;
	}
	.shop-list-zhutuiBtn{
		position: absolute;
		bottom: 0rpx;
		right: 0rpx;
		width: 140rpx;
		height: 60rpx;
		font-size: 30rpx;
		line-height: 64rpx;
		text-align: center;
		color: white;
		background-color: #34b930;
		border-radius: 30upx;
		border-color: #34b930;
	}
	
	.shop-list-box {
		position: relative;
		background-color: white;
		width: 100%;
		height: 760rpx;
		border-top-left-radius: 30rpx;
		border-top-right-radius: 30rpx;
	}
	
	.shop-list-list {
		position: relative;
		padding-top: 25rpx;
		padding-bottom: 20rpx;
		align-items: flex-start;
		flex-direction: row;
		border-top-width: 1px;
		border-top-color: #eeeeee;
	}

</style>
