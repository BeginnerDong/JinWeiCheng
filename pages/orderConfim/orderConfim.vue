<template>
	<view>
		
		<view class="bg-white boxShaow overflow-h mt-3 mb-3">
			<view class="px-3 py-3">
				
				<view class="d-flex j-sb a-center" v-if="addressData.city"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
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
					<view class="tit avoidOverflow">{{item.product?item.product.title:''}}</view>
					<view class="d-flex font-24 color6 mt-1">
						<view class="specsBtn mr-1">{{item.product&&item.product.sku&&item.product.sku[item.skuIndex]?item.product.sku[item.skuIndex].title:''}}</view>
					</view>
					<view class="d-flex j-sb B-price">
						<view class="price font-32 font-weight">
						{{item.product&&item.product.sku&&item.product.sku[item.skuIndex]?item.product.sku[item.skuIndex].price:''}}</view>
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
		
		<view class="d-flex j-sb a-center px-3 py-3 bg-white">
			<view>优惠券</view>
			<view class="d-flex j-end a-center color6 font-26" v-if="couponData.length==0" @click="couponShow">
				暂无优惠券使用<image class="arrowR ml-1" src="../../static/images/detailsl-icon1.png" mode=""></image>
			</view>
			<view class="d-flex j-end a-center color6 font-26" v-if="couponData.length>0&&chooseCoupon.length==0" @click="couponShow">
				{{couponData.length}}张优惠券可用<image class="arrowR ml-1" src="../../static/images/detailsl-icon1.png" mode=""></image>
			</view>
			<view class="d-flex j-end a-center color6 font-26" v-if="couponData.length>0&&chooseCoupon.length>0" @click="couponShow">
				优惠券抵扣-{{pay.coupon[0].price}}<image class="arrowR ml-1" src="../../static/images/detailsl-icon1.png" mode=""></image>
			</view>
		</view>
		
		
		<view class="xqbotomBar px-3">
			<view class="d-flex a-center mr-3 font-26">总计<view class="price font-weight font-30">{{totalPrice}}</view></view>
			<button style="margin: 0;font-size: 15px;" class="payBtn main-bg-color rounded50 text-white" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">立即支付</button>
		</view>
		
		<!-- 优惠券 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="couponShow whiteBj radius10" v-show="is_couponShow">
			<view class="d-flex j-sb px-3 py-3 border-bottom">
				<view class="font-26 color9" @click="couponShow">取消</view>
				<view class="">优惠券</view>
				<view class="font-26 main-text-color" @click="useCoupon">确定</view>
			</view>
			<view class="pt-1 font-26">
				<view class="item d-flex j-sb a-center px-3 mt-3" v-for="(item,index) in couponData" :key="index" 
				@click="couponChange(index)">
					<view>满{{item.condition}}减{{item.value}}元</view>
					<view class="seltIcon"><image :src="couponCurr==index?'../../static/images/shopping-icon.png':'../../static/images/shopping-icon1.png'" mode=""></image></view>
				</view>
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
				
				is_couponShow:false,
				
				couponCurr:-1,
				addressData:{},
				pay:{
					coupon:[]
				},
				totalPrice:0,
				mainData:[],
				couponData:[],
				chooseCoupon:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
			console.log('self.mainData',self.mainData)
			self.countTotalPrice()
			self.$Utils.loadAll(['getUserCouponData'], self);
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
			
			useCoupon() {
				const self = this;	
				self.couponTotalPrice = self.$Utils.addItemInArray(self.pay.coupon, 'price');
			/* 	console.log(index)
				console.log(self.couponData);
				console.log(self.couponData[0]); */
				
				var id = self.couponData[self.couponCurr].id;
				
				var findCoupon = self.$Utils.findItemInArray(self.couponData, 'id', id);
				var findItem = self.$Utils.findItemInArray(self.pay.coupon, 'id', id);
				console.log('findCoupon', findCoupon)
				self.showCoupon = false;
				if(self.pay.coupon.length>=1){
					self.pay.coupon = []
					self.chooseCoupon = []
				};
				if (findCoupon) {
					findCoupon = findCoupon[1];
					var findSameCoupon =  self.$Utils.findItemsInArray(self.pay.coupon, 'product_id', id);
				} else {
					self.$Utils.showToast('优惠券错误', 'none');
					return;
				};
				if (findItem) {
					self.pay.coupon.splice(findItem[0], 1);
					self.chooseCoupon = []
				} else {
					console.log('self.data.price - self.data.couponTotalPrice',self.totalPrice - self.couponTotalPrice);
					console.log('findCoupon.condition',findCoupon.condition);
					if ((self.totalPrice - self.couponTotalPrice) < findCoupon.condition) {
						self.$Utils.showToast('未达满减标准', 'none');				
						return;
					};			
					 console.log('findSameCoupon.length', findSameCoupon.length)
					if (self.pay.coupon.length >= 1) {
						self.$Utils.showToast('叠加使用超限', 'none');
					
						return;
					};
					if (findCoupon.type == 1) {
						var couponPrice = findCoupon.value;
						console.log('findCoupon.discount', findCoupon.discount)
					} else if (findCoupon.type == 2) {
						
						var couponPrice = parseFloat(self.totalPrice).toFixed(2) - parseFloat(findCoupon.discount / 100 * self.totalPrice)
							.toFixed(2);
					};
					if (parseFloat(couponPrice) + parseFloat(self.couponTotalPrice) > parseFloat(self.totalPrice)) {
						couponPrice = parseFloat(self.totalPrice).toFixed(2) - parseFloat(self.couponTotalPrice).toFixed(2);
					};
					self.pay.coupon.push({
						id: id,
						price: couponPrice.toFixed(2),
					});
					self.chooseCoupon.push({
						id: id,
						price: couponPrice,
					});
				};
				self.is_show = !self.is_show;
				self.is_couponShow = !self.is_couponShow;
				self.countTotalPrice();
			},
			
			getUserCouponData() {
				const self = this;
				var now = Date.parse(new Date());
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					use_step: 1,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.couponData.push.apply(self.couponData, res.info.data)
					}
					self.$Utils.finishFunc('getUserCouponData');
				};
				self.$apis.userCouponGet(postData, callback);
			},
			
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
				self.couponTotalPrice = self.$Utils.addItemInArray(self.pay.coupon, 'price');
				for (var i = 0; i < self.mainData.length; i++) {
					self.totalPrice += self.mainData[i].product.sku[self.mainData[i].skuIndex].price*self.mainData[i].count
				}
				
				self.totalPrice = (parseFloat(self.totalPrice) - parseFloat(self.couponTotalPrice)).toFixed(2)
				//console.log('wxPay',wxPay)
				if (self.totalPrice > 0) {
					self.pay.wxPay = {
						price: self.totalPrice,
					};
				} else {
					  delete self.pay.wxPay;	 
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
					orderList.push({sku_id:self.mainData[i].sku_id,count:self.mainData[i].count,data: data,
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
				postData.type=1;
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
						var array = self.$Utils.getStorageArray('cartData');
						for (var i = 0; i < orderList.length; i++) {
							for (var j = 0; j < array.length; j++) {
								if(orderList[i].sku_id == array[j].sku[array[j].skuIndex].id){
									self.$Utils.delStorageArray('cartData', orderList[i], 'id');
								}
							}
						};
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
				postData.payAfter = [
					{
						tableName: 'FlowLog',
						FuncName: 'add',
						data: {
							type:3,
							thirdapp_id:2,
							user_no:uni.getStorageSync('user_info').user_no,
							account:1,
							count:self.totalPrice,
							trade_info:'购买商品返积分'
						},
					},
				];
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
			
			couponChange(index){
				const self = this;
				self.couponCurr = index;
			},
			
			couponShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_couponShow = !self.is_couponShow;
			},

		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;box-sizing: border-box;}
	
	
	.xqbotomBar .payBtn{width: 350rpx;}
	.couponShow{width: 100%;position: fixed;left: 0;right: 0;bottom: 0;height:500rpx;z-index: 50;padding-bottom: 80rpx;}
	.seltIcon{width: 36rpx;height: 36rpx;}
</style>
