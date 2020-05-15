<template>
	<view>
		
		<view>
			<view class="banner-box">
				<view class="banner">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-color="#dcdcdc" indicator-active-color="#222222">
						<block v-for="(item,index) in sliderData.mainImg" :key="index">
							<swiper-item class="swiper-item">
								<image :src="item.url" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		
		<view class="indHome d-flex whiteBj pt-4 font-24 flex-wrap">
			<view class="item" v-for="(item,index) in labelData"  :key="index" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/productList/productList?id='+$event.currentTarget.dataset.id}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
				<view class="tit">{{item.title}}</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/coupon/coupon'}})">
				<image src="../../static/images/home-icon7.1.png"></image>
				<view class="tit">优惠券</view>
			</view>
		</view>
		
		<view class="px-3 mt-3">
			<view class="hotTab oh mb-3">
				<view class="name px-3 d-flex j-sb a-center" style="background-image: linear-gradient(to left,#f9b5b5,#f69392);">
					<view class="font-30 font-weight text-white">买一送一</view>
					<view class="more font-24 center" style="border: 1px solid #bd4240; color: #bd4240;" 
					@click="Router.navigateTo({route:{path:'/pages/productList-activity/productList-activity?type=2'}})">查看更多</view>
				</view>
				<view class="d-flex px-3 pt-3 pb-3 bg-white">
					<view class="item" v-for="(item,index) in productOneData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
						<view class="tit font-26 avoidOverflow2">{{item.title}}</view>
						<view class="price font-weight font-26">{{item.price}}</view>
					</view>
				</view>
			</view>
			<view class="hotTab oh mb-3">
				<view class="name px-3 d-flex j-sb a-center" style="background-image: linear-gradient(to left,#e7c68c,#e3bc78);">
					<view class="font-30 font-weight text-white">积分商城</view>
					<view class="more font-24 center" style="border: 1px solid #81511c; color: #81511c;" @click="Router.navigateTo({route:{path:'/pages/productList-integral/productList-integral'}})">查看更多</view>
				</view>
				<view class="d-flex px-3 pt-3 pb-3 bg-white">
					<view class="item" v-for="(item,index) in productTwoData" :key="index" :data-id="item.id"
					@click="Router.navigateTo({route:{path:'/pages/integralDetail/integralDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
						<view class="tit font-26 avoidOverflow2">{{item.title}}</view>
						<view class="priceF font-weight font-26">{{item.price}}</view>
					</view>
				</view>
			</view>
			<view class="hotTab oh mb-3">
				<view class="name px-3 d-flex j-sb a-center" style="background-image: linear-gradient(to left,#a3b9f7,#859ef0);">
					<view class="font-30 font-weight text-white">预售商品</view>
					<view class="more font-24 center" style="border: 1px solid #2e4798; color: #2e4798;" 
					@click="Router.navigateTo({route:{path:'/pages/productList-activity/productList-activity?type=3'}})">查看更多</view>
				</view>
				<view class="d-flex px-3 pt-3 pb-3 bg-white">
					<view class="item" v-for="(item,index) in productThreeData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
						<view class="tit font-26 avoidOverflow2">{{item.title}}</view>
						<view class="price font-weight font-26">{{item.price}}</view>
					</view>
				</view>
			</view>
			
			<view class="font-30 font-weight d-flex a-start pt-3 title-xian">
				<view class="xain"></view>
				<view>热销产品</view>
			</view>
			<view class="d-flex j-sb d-flex j-sb flex-wrap productList">
				<view class="item rounded10 mt-3" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
					<view class="infor">
						<view class="tit avoidOverflow2 font-30">{{item.title}}</view>
						<view class="price font-32 font-weight mt-2">{{item.price}}</view>
					</view>
				</view>
			</view>
			
			<!-- 无数据 -->
			<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
			
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">购物车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				sliderData:{},
				labelData:[],
				productOneData:[],
				productTwoData:[],
				productThreeData:[],
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getSliderData','getLabelData','getProductOneData','getMainData','getProductTwoData','getProductThreeData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					type:1
				};
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3,
					parentid:1
				};
				postData.paginate =  {
					count: 0,
					currentPage:1,
					pagesize:7,
					is_page:true,
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data);
					}
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:"首页轮播"
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.sliderData = res.info.data[0]
					}
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getProductOneData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:2,
				};
				postData.paginate =  {
					count: 0,
					currentPage:1,
					pagesize:3,
					is_page:true,
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.productOneData.push.apply(self.productOneData, res.info.data);
					}
					self.$Utils.finishFunc('getProductOneData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			getProductTwoData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:4,
				};
				postData.paginate =  {
					count: 0,
					currentPage:1,
					pagesize:3,
					is_page:true,
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.productTwoData.push.apply(self.productTwoData, res.info.data);
					}
					self.$Utils.finishFunc('getProductTwoData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			getProductThreeData() {
				const self = this;
				var now = new Date().getTime();
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3,
					start_time:['<', now]
				};
				postData.paginate =  {
					count: 0,
					currentPage:1,
					pagesize:3,
					is_page:true,
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.productThreeData.push.apply(self.productThreeData, res.info.data);
					}
					self.$Utils.finishFunc('getProductThreeData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/productList.css";
	
	page{padding-bottom: 140rpx;background: #F5F5F5;}	
	
	.swiper-box {height: 340rpx;box-sizing: border-box;}
	.swiper-box swiper-item{width: 100%;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item image{width: 100%;height: 100%;box-sizing: border-box;}
	
	/* 分类导航 */
	.indHome .item{width: 25%;text-align: center;color: #222;display: flex; flex-direction: column;margin-bottom: 40rpx;line-height: 36rpx; }
	.indHome .item image{width:90rpx;height:90rpx; margin: 0 auto 20rpx auto;}
	
	.title-xian{position: relative; z-index: 1;}
	.title-xian::before{content: '';width: 60rpx;height: 10rpx;background-color: #fe7f45;position: absolute;left: 0;bottom: 4rpx; z-index: -1;}
	/* .title-xian .xian{width: 60rpx;height: 10rpx;background-color: #fe7f45;position: absolute;left: 0;bottom: 4rpx; z-index: -1;}
	 */
	/* 热销产品 */
	.hotTab {border-radius: 30rpx 30rpx 20rpx 20rpx;height: 478rpx;overflow: hidden;}
	.hotTab .name{height: 90rpx;}
	.hotTab .name .more{width: 140rpx;height: 46rpx;line-height: 46rpx;border-radius: 25rpx;text-align: center;color: #73551f;border: 1px solid #73551f;}
	.hotTab .item{width: 190rpx;height: 328rpx;margin-right: 30rpx;}
	.hotTab .item:nth-of-type(3){margin-right: 0;}
	.hotTab .item .pic image{width: 100%;height: 190rpx;}
	.hotTab .item .tit{line-height: 36rpx;height: 72rpx;margin: 10rpx 0 20rpx 0;overflow: hidden;}
	.hotTab .item .price{line-height: 30rpx;}
	
	/* 特价商品 */
	/* .specialPro .item{margin-bottom: 30rpx;background: #fff;}
	.specialPro .item .pic{width: 100%;height: 300rpx;}
	.specialPro .item .pic image{width: 100%;height: 100%;}
	.specialPro .item .infor{padding: 24rpx 4%;} */
</style>
