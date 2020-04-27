<template>
	<view>
		
		<view class="banner-box">
			<image class="slide-image" src="../../static/images/detailsl-img.png" />
		</view>
		
		<view class="mx-3 py-2">
			<view class="font-32 font-weight pb-2">墨西哥牛油果8枚单果200g左右</view>
			<view class="d-flex j-sb">
				<view class="price font-weight font-32">56</view>
				<view class="color9 font-24">预售期：4月15日</view>
			</view>
		</view>
		
		<view class="f5Bj-H20"></view>
		<view class="mx-3 py-3  color6">
			<view class="d-flex j-sb a-center" @click="spaceShow">
				<view class="font-26">规格选择</view>
				<view class="arrowR"><image src="../../static/images/detailsl-icon1.png" mode=""></image></view>
			</view>
			
			<view class="specsLable d-flex font-26">
				<view class="tt" v-for="(item,index) in specsData" :key="index">{{item}}</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		<view class="px-3 py-3 d-flex j-sb a-center">
			<view class="font-26 color6">预售说明</view>
			<view class="d-flex j-end a-center" @click="yushouTextShow">查看<image class="arrowR" src="../../static/images/detailsl-icon1.png" mode=""></image></view>
		</view>
		<view class="f5Bj-H20"></view>
		<view class="px-3">
			<view class="py-3 xqInfor">
				<view class="font-26 pb-3 color6">商品描述</view>
				<view class="cont fs14 text-center">
					<view>管理客服电话</view>
					<view>悲愤交加鹤骨鸡肤供货</view>
					<view>方点击可供货方都拉黑干活的放假</view>
					<view>价格过节费考虑到加工费</view>
					<view><image src="../../static/images/detailsl-img1.png" mode="widthFix"></image></view>
					<view>管理客服电话</view>
					<view>悲愤交加鹤骨鸡肤供货</view>
					<view>方点击可供货方都拉黑干活的放假</view>
					<view>价格过节费考虑到加工费</view>
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
				<view class="w-50 text-center hei" @click="spaceShow">加入购物车</view>
				<view class="w-50 text-center main-bg-color" @click="spaceShow">立即购买</view>
			</view>
		</view>
		
		
		<!-- 规格选择 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="spaceShow bg-white" v-show="is_spaceShow">
			<view class="closebtn" @click="spaceShow">×</view>
			<view class="d-flex">
				<view class="pic rounded10 overflow-h mr-3"><image src="../../static/images/home-img1.png" mode=""></image></view>
				<view class="infor">
					<view class="price font-weight font-36 mt-5 pt-2 mb-3">42</view>
					<view class="font-26">请选择规格</view>
				</view>
			</view>
			<view class="mt-3">
				<view class="font-26">规格</view>
				<view class="specsLable d-flex font-26 color6">
					<view class="tt" :class="specsCurr==index?'on':''" v-for="(item,index) in seltSpecsData" :key="index" @click="specsChange(index)">{{item}}</view>
				</view>
			</view>
			<view class="xqbotomBar px-3" style="box-shadow:initial">
				<view class="bottom-btnCont d-flex rounded50 overflow-h text-white font-30" style="width: 100%">
					<view class="w-50 text-center hei">加入购物车</view>
					<view class="w-50 text-center main-bg-color" @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">立即购买</view>
				</view>
			</view>
			
		</view>
		
		
		<!-- 预售说明弹框 -->
		<view class="yushouText" v-show="is_yushouTextShow">
			<view class="closebtn" @click="yushouTextShow">×</view>
			<view class="mb-3 font-30 pb-1 text-center">预售说明</view>
			<view class="mgt15">
				<scroll-view class="xqInfor font-26 color6" scroll-y>
					<view>1、九是国家开发的说法是国防生的客户给发的广泛地刚刚好共同繁荣</view>
					<view>1、九是国家开发的说法是国防生的客户给发的广泛地刚刚好共同繁荣</view>
					<view>1、九是国家开发的说法是国防生的客户给发的广泛地刚刚好共同繁荣</view>
					<view>1、九是国家开发的说法是国防生的客户给发的广泛地刚刚好共同繁荣</view>
					<view>1、九是国家开发的说法是国防生的客户给发的广泛地刚刚好共同繁荣</view>
					<view>1、九是国家开发的说法是国防生的客户给发的广泛地刚刚好共同繁荣</view>
					<view>1、九是国家开发的说法是国防生的客户给发的广泛地刚刚好共同繁荣</view>
					<view>1、九是国家开发的说法是国防生的客户给发的广泛地刚刚好共同繁荣</view>
				</scroll-view>
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
				specsCurr:0,
				specsData:['定制版120ML','定制版100ML'],
				is_spaceShow:false,
				seltSpecsData:['定制版120ML','定制版100ML','定制版','定制版','定制版100ML','定制版120ML',],
				is_yushouTextShow:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			specsChange(index){
				const self = this;
				self.specsCurr = index
			},
			spaceShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_spaceShow = !self.is_spaceShow 
			},
			yushouTextShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_yushouTextShow = !self.is_yushouTextShow 
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/detail.css";
	@import "../../assets/style/productList.css";
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
	
	.xqInfor{height: 440rpx;}
	.xqInfor view{width: 100%;margin-bottom: 20rpx;}
	
</style>
