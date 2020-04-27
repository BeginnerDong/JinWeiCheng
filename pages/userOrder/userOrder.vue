<template>
	<view>
		
		<view class="topNavFix f5bj">
			<view class="orderNav bg-white d-flex j-sb a-center shadow color6">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">待支付</view>
				<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">待发货</view>
				<view class="tt" :class="curr==4?'on':''" @click="changeCurr('4')">已发货</view>
				<view class="tt" :class="curr==5?'on':''" @click="changeCurr('5')">已完成</view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		<view class="mt-3">
			<view class="proRow ">
				<view class="item mb-3 bg-white">
					<view class="priList" v-for="(item,index) in orderData" :key="index">
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red" v-if="curr==1||curr==2">待支付</view>
							<view class="red" v-if="curr==3">待发货</view>
							<view class="red" v-if="curr==4">已发货</view>
							<view class="red" v-if="curr==5">已完成</view>
						</view>
						<view class="d-flex a-center j-sb">
							<view class="pic">
								<image src="../../static/images/shopping-img.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="tit avoidOverflow">墨西哥牛油果8枚单果200g左右</view>
								<view class="d-flex font-24 color6 mt-1">
									<view class="specsBtn mr-1">精装品5斤</view>
								</view>
								<view class="B-price d-flex a-center j-sb">
									<view class="price">56.00</view>
									<view class="font-26">×1</view>
								</view>
							</view>
						</view>
					</view>
					<view class="underBtn d-flex j-end a-center py-3" v-if="curr==1||curr==2" @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">
						<view class="Bbtn red">去支付</view>
					</view>
					<view class="underBtn d-flex j-end a-center py-3"  v-if="curr==4">
						<view class="Bbtn red">确认收货</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				orderData:2
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proRow.css";
	/* @import "../../assets/style/editInfor.css"; */
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height: 80rpx;position: fixed;top: 0rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height: 80rpx;}
	
	.proRow .item{padding: 0 30rpx;}
	.proRow .item .priList{padding: 30rpx 0;border-top: 1px solid #eee;}
	.proRow .item .priList:first-child{border-top: 0;}	
	.proRow .item .pic{width: 160rpx;height: 160rpx;}
	.proRow .item .infor{width: 72%;height: 160rpx;}
	.orderNav .tt.on::after{bottom: 0;}
</style>
