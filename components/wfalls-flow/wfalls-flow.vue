<template>
	<view class="list-container">
		<view id="wf-list" class="list" v-for="(list,listIndex) of viewList" :key="listIndex">
            <view class="item" v-for="(item,index) of list.list" :key="index" @click="onClickRoom(item)">
                <view style="position: relative;">
					<image @load="handleViewRender(listIndex,index)" @error="handleViewRender(listIndex,index)" :src="item.roomInfo.roomAvatar" mode="widthFix"></image>
					<view class="live_list_audienceCount">
<!-- 						<view class="live_icon">直播中</view> -->
						<text class="store_live_title">{{item.roomInfo.liveTitle}}</text>
						<view class="live_icon"><image class="live_icon_img" src="~@/static/live-video/lk_green.gif"></image></view>
						<view class="live_people">热度&nbsp;{{item.custom.hotDegree}}</view>
						
					</view>
				</view>
				<view class="desc">
                	<view class="store_icon_view">
						<image class="store_icon" :src="item.roomInfo.storeAvatar"></image>
					</view>
                	<text class="store_name">{{item.roomInfo.storeName}}</text>
                </view>
            </view>
        </view>
	</view>
</template>

<script>
	export default {
        props:{
            list:{
                type:Array, //实际请求获取的列表数据
            }
        },
		data() {
			return {
                viewList:[{list:[]},{list:[]}],  //展示到视图的列表数据
                everyNum:2
			}
		},
		methods: {
            init(){
                this.viewList = [{list:[]},{list:[]}];
                setTimeout(()=>{
                    this.handleViewRender(0,0)
					// console.log(this.viewList)
                },0)
				
            },
			handleViewRender(x,y){
                // console.log(x,y);
                // const num = (x+1)*(y+1);
                // console.log(num%(this.everyNum));
                // console.log(num);
                // if((num%(this.everyNum))!==0&&num!==1)return;
                // console.log(num,'----');
                const index = this.viewList.reduce((total,current)=>total + current.list.length,0)
                if(index>this.list.length-1) {
                    // 加载完成触发事件并返回加载过的图片数
                    this.$emit('finishLoad',index)
                    return
                };
				// console.log(this.viewList)
                const query = uni.createSelectorQuery().in(this);
                let listFlag = 0;
                query.selectAll('#wf-list').boundingClientRect(data => {
                    listFlag = data[0].bottom - data[1].bottom<=0?0:1;
                    this.viewList[listFlag].list.push(this.list[index])
                    // this.list.slice(index,index+this.everyNum).forEach((item,index)=>{
                    //     const flag = listFlag===0?index&1:Number(!(index&1))
                    //     this.viewList[flag].list.push(item)
                    // })
                }).exec()
				// console.log(this.viewList)
            },
			onClickRoom(item) {
				console.log(item)
				//进入直播
				uni.navigateTo({
					url: '/pages/txmlvb/room?roomID=' + item.roomID + "&storeName=" + item.roomInfo.storeName + "&storeAvatar=" + item.roomInfo.storeAvatar + "&storeId=" + item.roomInfo.storeId
				});
			}
		},
        mounted() {
			console.log(this.list)
            if(this.list.length){
				
                this.init()
            }
        }
	}
</script>

<style lang="stylus" scoped>
    .list-container
        display flex
        justify-content space-between
        align-items:flex-start
        padding 0 24upx
        padding-top 30upx
        .list
            width calc(50% - 8upx)
            display flex
            flex-direction column
            .item
                margin-bottom 18upx
                padding-bottom 18upx
                border 1px solid #31b630
                image
                    width 100%
</style>
<style scoped>
	.desc{
		padding: 0upx 12upx 0upx 12upx;
		font-size: 30upx;
		color: #999;
		text-align: center;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
	}

	.live_list_audienceCount{
		padding: 20upx 22upx 0upx 22upx;
		font-size: 30upx;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		position: absolute;
		top: 0;
		left: 0;
	}
	
	.live_icon{
		background-color: #31b630;
		color: #FFFFFF;
		text-align: left;
		font-size: 24upx;
		z-index: 100;
		width: 42upx;
		height: 37upx;
		overflow: hidden;
		border-top-left-radius: 15upx;
		border-bottom-left-radius: 15upx;
		border-bottom-right-radius: 15upx;
	}
	
	.live_icon_img{
		width: 50upx;
		height: 37upx;
	}
	
	.live_people{
		background-color: rgba(100, 100, 100, 0.3);
		color: #FFFFFF;
		text-align: left;
		font-size: 24upx;
		width: 130upx;
		height: 28upx;
		line-height: 28upx;
		padding: 5upx 20upx;
		border-top-right-radius: 10upx;
		border-bottom-right-radius: 10upx;
		position: relative;
		left: -12upx;
	}
	.store_live_title{
		width: 300upx;
		height: 80upx;
		margin-bottom: 20upx;
		margin-top: 20upx;
/* 		lines: 2;
		overflow: hidden;
		text-overflow: ellipsis; 超出部分省略号 */
		font-weight: bold;
		display: -webkit-box; /*值必须为-webkit-box或者-webkit-inline-box*/
		-webkit-box-orient: vertical; /*值必须为vertical*/
		-webkit-line-clamp: 2; /*值为数字，表示一共显示几行*/
		overflow: hidden;
		/* background-color: rgba(100, 100, 100, 0.7); */
	}
	.store_icon_view{
		width: 60upx;
		height: 60upx;
		overflow: hidden;
		border-radius: 30upx;
	}
	.store_icon {
		width: 60upx;
		height: 60upx;
	}
	.store_name {
		width: 215upx;
		height: 60upx;
		font-size: 26upx;
		line-height: 60upx;
		text-align: center;
/* 		lines: 1;
		overflow: hidden;
		text-overflow: ellipsis; 超出部分省略号 */
		padding: 0upx 10upx 0upx 20upx;
		display: -webkit-box; /*值必须为-webkit-box或者-webkit-inline-box*/
		-webkit-box-orient: vertical; /*值必须为vertical*/
		-webkit-line-clamp: 1; /*值为数字，表示一共显示几行*/
		overflow: hidden;
	}
</style>