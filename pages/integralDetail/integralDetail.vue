<template>
	<view>
		
		<view class="banner-box">
			<image class="slide-image" :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" />
		</view>
		
		<view class="mx-3 py-2">
			<view class="font-32 font-weight pb-2">{{mainData.title}}</view>
			<view class="d-flex">
				<view class="priceF font-weight font-32">{{mainData.price}}</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		<view class="px-3">
			<view class="py-3 xqInfor">
				<view class="font-26 pb-3 color6">商品描述</view>
				<view class="cont fs14 text-center">
					<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
					</view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar center px-3">
			<view class="d-flex fs12">
				<view class="ite flexColumn" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<view class="icon"><image src="../../static/images/detailsl-icon2.png" mode=""></image></view>
					<view class="mt-1">首页</view>
				</view>
			</view>
			<view class="bottom-btnCont d-flex rounded50 overflow-h text-white font-30">
				<view class="text-center w-100 main-bg-color"  
				@click="goBuy">立即兑换</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false,
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
		},
		
		methods: {
			
			goBuy() {
				const self = this;
				uni.setStorageSync('canClick', false);
				self.orderList.push({
					product_id: self.mainData.id,
					count: 1,
					type: self.mainData.type,
					product: self.mainData
				}, );
				uni.setStorageSync('payPro', self.orderList);
				self.Router.navigateTo({
					route: {
						path: '/pages/integralOrderConfim/integralOrderConfim'
					}
				})
				uni.setStorageSync('canClick', true);
			},
			
			getMainData() {
				const self = this;
				var now = new Date().getTime();
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id:self.id
				};

				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					self.$Utils.finishFunc('getMainData');
						
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/detail.css";
	page{padding-bottom:140rpx;}
	
	.banner-box {width: 100%;height: 400rpx;box-sizing: border-box;overflow: hidden;}
	
	.bottom-btnCont{width: 520rpx;line-height: 80rpx;}
	.bottom-btnCont .hei{background-color: #3c3c3c;}
	
	.specsLable{flex-wrap: wrap;}
	.specsLable .tt{margin: 30rpx 50rpx 0 0;border: 1px solid #ddd;line-height: 60rpx;padding: 0 16rpx;border-radius:10rpx;}
	
	.specsLable .tt.on{background-color: #fff1eb;color: #fe7f45;border: 1px solid #fe7f45;}
	
	.spaceShow{width: 100%;border-radius: 20rpx 20rpx 0 0;position: fixed;left: 0;right: 0;bottom: 0;height: 800rpx;z-index: 50;padding: 30rpx;padding-bottom: 140rpx;}
	.spaceShow .pic{width: 210rpx;height: 210rpx;}
	
	.yushouText{height: 600rpx;width: 80%;background-color: #fff;border-radius: 10rpx;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);box-sizing: border-box;padding: 30rpx;z-index: 50;}
	
	
</style>
