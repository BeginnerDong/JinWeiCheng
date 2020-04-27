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
						<view class="price font-32 font-weight">56</view>
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
		
		<view class="d-flex j-sb a-center px-3 py-3 bg-white">
			<view>优惠券</view>
			<view class="d-flex j-end a-center color6 font-26" @click="couponShow">满20减5元<image class="arrowR ml-1" src="../../static/images/detailsl-icon1.png" mode=""></image>
			</view>
		</view>
		
		
		<view class="xqbotomBar px-3">
			<view class="d-flex a-center mr-3 font-26">总计<view class="price font-weight font-30">79</view></view>
			<view class="payBtn main-bg-color rounded50 text-white">立即支付</view>
		</view>
		
		<!-- 优惠券 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="couponShow whiteBj radius10" v-show="is_couponShow">
			<view class="d-flex j-sb px-3 py-3 border-bottom">
				<view class="font-26 color9" @click="couponShow">取消</view>
				<view class="">优惠券</view>
				<view class="font-26 main-text-color">确定</view>
			</view>
			<view class="pt-1 font-26">
				<view class="item d-flex j-sb a-center px-3 mt-3" v-for="(item,index) in couponData" :key="index" @click="couponChange(index)">
					<view>满20减5元</view>
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
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				count:1,
				is_show:false,
				is_couponShow:false,
				couponData:3,
				couponCurr:0
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			couponChange(index){
				const self = this;
				self.couponCurr = index;
			},
			couponShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_couponShow = !self.is_couponShow;
			},
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
			}
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
