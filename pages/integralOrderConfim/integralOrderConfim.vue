<template>
	<view>
		
		<view class="bg-white boxShaow overflow-h mt-3 mb-3">
			<view class="px-3 py-3">
				
				<view class="d-flex j-sb a-center" v-if="addressData.name"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
					<view class="font-28">
						<view class="d-flex mt-1">
							<view class="mr-3 color2">{{addressData.name}}</view>
							<view class="color6">{{addressData.phone}}</view>
						</view>
						<view class="mt-2">{{addressData.city+addressData.detail}}</view>
					</view>
					<view class="d-flex j-end" style="width: 20%;">
						<image class="arrowR" src="../../static/images/detailsl-icon1.png" mode=""></image>
					</view>
				</view>
				
				<view class="d-flex j-center pt-3 pb-4" v-else @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
					<view class="font-28 color6 d-flex">
						<view style="width: 36rpx;height:36rpx;margin-right: 20rpx;"><image src="../../static/images/the-orderl-icon3.png" mode=""></image></view>
						<view class="font-30">添加收货地址</view>
					</view>
				</view>
			</view>
			<view style="width: 100%;height: 8rpx;"><image src="../../static/images/the-orderl-img.png" mode=""></image></view>
		</view>
		<view class="proRow mt-3">
			<view class="item d-flex j-sb mb-3" v-for="(item,index) in mainData" :key="index">
				<view class="pic"><image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image></view>
				<view class="infor">
					<view class="tit avoidOverflow">{{item.product&&item.product.title?item.product.title:''}}</view>
					<!-- <view class="d-flex font-24 color6 mt-1">
						<view class="specsBtn mr-1">精装品5斤</view>
					</view> -->
					<view class="d-flex j-sb B-price">
						<view class="priceF font-32 font-weight">{{item.product&&item.product.price?item.product.price:''}}</view>
						<view class="flexEnd">
							<view class="numBox d-flex">
								<view class="btn" @click="counter(index,'-')">-</view>
								<view class="num">{{item.count}}</view>
								<view class="btn pubBj white add" @click="counter(index,'+')">+</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar px-3">
			<view class="d-flex a-center mr-3 font-26 flex"><view>总计</view><view class="priceF font-weight font-30 ml-1">{{totalPrice}}</view></view>
			<view class="payBtn main-bg-color rounded50 text-white" @click="exchangeShow">立即兑换</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="exchangeShow rounded20 bg-white" v-show="is_exchangeShow">
			<view class="closebtn" style="right: auto;left: 30rpx;" @click="exchangeShow">×</view>
			<view class="tit text-center font-30">瑾味成电子商务有限公司</view>
			<view class="d-flex j-center mt-5 border-bottom pb-3">
				<view class="font-26 mt-1">积分</view>
				<view class="munber font-big font-weight ml" >{{totalPrice}}</view>
			</view>
			<view class="d-flex j-sb a-center py-3">
				<view class="font-26 color6">支付方式</view>
				<view class="d-flex j-end a-center"><image class="mr-1" style="width: 36rpx;height: 36rpx;" src="../../static/images/the-orderl-icon2.png" mode=""></image>积分</view>
			</view>
			<view class="submitbtn" style="margin-top: 80rpx;">
				<button style="margin: 0;font-size:15px" class="Wbtn" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">确认兑换</button>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				count:1,
				is_exchangeShow:false,
				is_exchangeOk:false,
				addressData:{},
				pay:{
					
				},
				totalPrice:0,
				mainData:[],
			}
		},
		
		onLoad(options) {
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
			console.log('self.mainData',self.mainData)
			self.countTotalPrice()
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('choosedAddressData')){
				self.addressData = uni.getStorageSync('choosedAddressData')
			}else{
				self.getAddressData()
			};
		},
		
		methods: {
			
			getAddressData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					isdefault:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addressData = res.info.data[0];
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			counter(index, type) {
				const self = this;
				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.countTotalPrice();
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				
				for (var i = 0; i < self.mainData.length; i++) {
					self.totalPrice += self.mainData[i].product.price*self.mainData[i].count
				}
				
				self.totalPrice = (parseFloat(self.totalPrice)).toFixed(2)
				//console.log('wxPay',wxPay)
				if (self.totalPrice > 0) {
					self.pay.score = {
						price: self.totalPrice,
					};
				} else {
					  delete self.pay.score;	 
				};
				console.log(self.pay)
			},
			
			submit(){
				const self = this;
				
				uni.setStorageSync('canClick', false);
				if(JSON.stringify(self.addressData) == '{}'){
					uni.setStorageSync('canClick', true);
					
					self.$Utils.showToast('请选择收货地址','none')
					return
				}
				var data = {
					snap_address: self.addressData,
				}
				var orderList = []
				for (var i = 0; i < self.mainData.length; i++) {
					orderList.push({product_id:self.mainData[i].product_id,count:self.mainData[i].count,data: data,
					snap_address: self.addressData})
				}
				const callback = (user, res) => {
					self.addOrder(orderList)
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			addOrder(orderList) {
				const self = this;	
				
				/* if(self.orderId){
					self.goPay()
					return
				}; */
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.type=2;
				postData.data = {
					level:1,
					snap_address:self.addressData,
				};
				postData.parent = 1;
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.exchangeShow()
						self.goPay()
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay() {
				const self = this;	
				const postData = self.$Utils.cloneForm(self.pay);	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
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
										self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
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
								self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
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
			
			exchangeShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_exchangeShow = !self.is_exchangeShow
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;box-sizing: border-box;}
	
	
	/* .xqbotomBar .payBtn{width: 350rpx;} */
	.exchangeShow{width:80%;height:630rpx;padding: 100rpx 50rpx 50rpx 50rpx;position: fixed;left: 50%;right: 50%;transform: translate(-50%,-50%);z-index: 50;padding-bottom: 80rpx; z-index: 50;}
	.alertBox{position: absolute;top: 50%;left: 50%;transform: translate(-50%,-50%);width: 400rpx;height: 200rpx; z-index: 55;background-color: rgba(0,0,0,0.5);}
	
</style>
