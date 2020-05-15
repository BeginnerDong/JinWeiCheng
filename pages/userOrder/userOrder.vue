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
				<view class="item mb-3 bg-white" v-for="(item,index) in mainData" :key="index">
					<view class="priList">
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：{{item.create_time}}</view>
							<view class="red" v-if="item.pay_status==0">待支付</view>
							<view class="red" v-if="item.transport_status==0&&item.pay_status==1">待发货</view>
							<view class="red" v-if="item.transport_status==1&&item.pay_status==1">已发货</view>
							<view class="red" v-if="item.transport_status==2&&item.pay_status==1">已完成</view>
						</view>
						<view class="d-flex a-center j-sb" v-for="(c_item,c_index) in item.child" :key="c_index">
							<view class="pic" v-if="item.type==2">
								<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product.mainImg&&c_item.orderItem[0].snap_product.mainImg[0]?c_item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="pic" v-if="item.type==1">
								<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product&&
								c_item.orderItem[0].snap_product.product.mainImg&&c_item.orderItem[0].snap_product.product.mainImg[0]?c_item.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">	
								<view class="tit avoidOverflow" v-if="item.type==1">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product?c_item.orderItem[0].snap_product.product.title:''}}</view>
								<view class="tit avoidOverflow" v-if="item.type==2">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}</view>
								<view class="d-flex font-24 color6 mt-1" v-if="item.type==1">
									<view class="specsBtn mr-1">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}</view>
								</view>
								<view class="d-flex font-24 color6 mt-1" v-if="item.type==2">
									<view class="specsBtn mr-1">积分兑换</view>
								</view>
								<view class="B-price d-flex a-center j-sb">
									<view class="price">{{c_item.unit_price?c_item.unit_price:''}}</view>
									<view class="font-26">×{{c_item.count?c_item.count:''}}</view>
								</view>
							</view>
						</view>
					</view>
					<view class="underBtn d-flex j-end a-center py-3" v-if="item.pay_status==0" 
					@click="goPay(index)">
						<view class="Bbtn red">去支付</view>
					</view>
					<view class="underBtn d-flex j-end a-center py-3" @click="orderUpdate(index)"  v-if="item.transport_status==1">
						<view class="Bbtn red">确认收货</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
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
				orderData:2,
				searchItem:{
					thirdapp_id:2,
					level:1
				},
				mainData:[],
				Utils:this.$Utils,
				pay:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
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
			
			goPay(index) {
				const self = this;	
				
				if(self.mainData[index].type==1){
					self.pay.wxPay = {
						price: self.mainData[index].price,
					};
				}else{
					self.pay.score = {
						price: self.mainData[index].price,
					};
				}
				const postData = self.$Utils.cloneForm(self.pay);	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.mainData[index].id
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
					
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.getMainData(true)
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.getMainData(true)
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr;
					if(self.curr==1){
						delete self.searchItem.pay_status
						delete self.searchItem.transport_status
					}else if(self.curr==2){
						self.searchItem.pay_status = 0
						self.searchItem.transport_status = 0
					}else if(self.curr==3){
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 0
					}else if(self.curr==4){
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 1
					}else if(self.curr==5){
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 2
					};
					self.getMainData(true)
				}
			},
			
			orderUpdate(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					transport_status:2,
				};
				postData.searchItem = {
					id:self.mainData[index].id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
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
