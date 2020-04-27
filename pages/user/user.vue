<template>
	<view>
		
		<view class="userHead  main-bg-color">
			<view class="infor mx-3 text-white">
				<view class="d-flex a-center mt-4">
					<view><image class="photo" src="../../static/images/about-img.png" mode=""></image></view>
					<view style="width: 70%;">
						<view class="font-32">哆啦A梦</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="mx-3">
			<view class="userBox2 shadow-sm rounded10 bg-white font-24" style="margin-top: -90rpx;">
				<view class="d-flex j-sb a-center py-3  mx-3 border-bottom">
					<view class="font-30 font-weight">我的订单</view>
					<view class="more d-flex j-end a-center color9" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">全部订单<image class="arrowR" src="../../static/images/detailsl-icon1.png" mode=""></image></view>
				</view>
				<view class="menu d-flex j-sb a-center font-24 py-3 text-center">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon.png"></image>
						</view>
						<view>待支付</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon1.png"></image>
						</view>
						<view>待发货</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon2.png"></image>
						</view>
						<view>已发货</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon3.png"></image>
						</view>
						<view>已完成</view>
					</view>
				</view>
			</view>
			
			<view class="userBox2 shadow-sm rounded10 bg-white mt-4 font-24">
				<view class="d-flex j-sb a-center py-3  mx-3 border-bottom">
					<view class="font-30 font-weight">管理工具</view>
				</view>
				<view class="menu d-flex j-sb a-center py-3 text-center">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-myIntegral/user-myIntegral'}})">
						<view class="icon">
							<image src="../../static/images/about-icon4.png"></image>
						</view>
						<view>我的积分</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="icon">
							<image src="../../static/images/about-icon5.png"></image>
						</view>
						<view>收货地址</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-coupon/user-coupon'}})">
						<view class="icon">
							<image src="../../static/images/about-icon6.png"></image>
						</view>
						<view>我的优惠券</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-ContactUs/user-ContactUs'}})">
						<view class="icon">
							<image src="../../static/images/about-icon7.png"></image>
						</view>
						<view>联系我们</view>
					</view>
				</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">购物车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">我的</view>
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
				showView: false,
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	/* @import "../../assets/style/editInfor.css"; */
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	
	.userHead{width: 100%;height: 270rpx;}
	.userHead .photo{width: 110rpx;height: 110rpx;border-radius: 50%;margin-right: 20rpx;border: 1px solid #ffa278;;}
	.userHead .infor{position: absolute;top: 0; right: 0; left: 0;}
	
	.userBox2{width: 100%;height: 264rpx;position: relative; z-index: 1; }
	.userBox2 .item{width: 30%;}
	.userBox2 .item .icon{width: 48rpx;height:48rpx;display: block;margin: 0 auto;margin-bottom: 20rpx;position: relative;}
	.userBox2 .item .icon image{width: 100%;height: 100%;}
	
</style>
