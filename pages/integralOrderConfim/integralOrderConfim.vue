<template>
	<view>
		
		<view class="bg-white boxShaow overflow-h mt-3 mb-3">
			<view class="px-3 py-3">
				<view class="d-flex j-center pt-3 pb-4" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
					<view class="font-28 color6 d-flex">
						<view style="width: 36rpx;height:36rpx;margin-right: 20rpx;"><image src="../../static/images/the-orderl-icon3.png" mode=""></image></view>
						<view class="font-30">添加收货地址</view>
					</view>
				</view>
				<view class="d-flex j-sb a-center"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
					<view class="font-28">
						<view class="d-flex mt-1">
							<view class="mr-3 color2">张思德</view>
							<view class="color6">15689562352</view>
						</view>
						<view class="mt-2">陕西省西安市雁塔区高新大都荟</view>
					</view>
					<view class="d-flex j-end" style="width: 20%;">
						<image class="arrowR" src="../../static/images/detailsl-icon1.png" mode=""></image>
					</view>
				</view>
			</view>
			<view style="width: 100%;height: 8rpx;"><image src="../../static/images/the-orderl-img.png" mode=""></image></view>
		</view>
		<view class="proRow mt-3">
			<view class="item d-flex j-sb mb-3" >
				<view class="pic"><image src="../../static/images/shopping-img.png" mode=""></image></view>
				<view class="infor">
					<view class="tit avoidOverflow">墨西哥牛油果8枚单果200g左右</view>
					<view class="d-flex font-24 color6 mt-1">
						<view class="specsBtn mr-1">精装品5斤</view>
					</view>
					<view class="d-flex j-sb B-price">
						<view class="priceF font-32 font-weight">56</view>
						<view class="flexEnd">
							<view class="numBox d-flex">
								<view class="btn" @click="counter('-')">-</view>
								<view class="num">{{count}}</view>
								<view class="btn pubBj white add" @click="counter('+')">+</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar px-3">
			<view class="d-flex a-center mr-3 font-26 flex"><view>总计</view><view class="priceF font-weight font-30 ml-1">56</view></view>
			<view class="payBtn main-bg-color rounded50 text-white" @click="exchangeShow">立即兑换</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="exchangeShow rounded20 bg-white" v-show="is_exchangeShow">
			<view class="closebtn" style="right: auto;left: 30rpx;" @click="exchangeShow">×</view>
			<view class="tit text-center font-30">瑾味成电子商务有限公司</view>
			<view class="d-flex j-center mt-5 border-bottom pb-3">
				<view class="font-26 mt-1">积分</view>
				<view class="munber font-big font-weight ml" >56</view>
			</view>
			<view class="d-flex j-sb a-center py-3">
				<view class="font-26 color6">支付方式</view>
				<view class="d-flex j-end a-center"><image class="mr-1" style="width: 36rpx;height: 36rpx;" src="../../static/images/the-orderl-icon2.png" mode=""></image>积分</view>
			</view>
			<view class="submitbtn" style="margin-top: 80rpx;">
				<view class="Wbtn" @click="exchangeOk">确认兑换</view>
			</view>
			
			<view class="alertBox font-26 text-white p-3 rounded20 text-center d-flex flex-column a-center j-center" v-show="is_exchangeOk">
				<view class="closebtn text-white"  @click="exchangeOk">×</view>
				<view class="py-1">您的积分还不足兑换商品！</view>
				<view class="">兑换成功！</view>
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
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				count:1,
				is_show:false,
				is_exchangeShow:false,
				is_exchangeOk:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
			},
			exchangeShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_exchangeShow = !self.is_exchangeShow
			},
			exchangeOk(){
				const self = this;
				self.is_exchangeOk = !self.is_exchangeOk
			}
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
