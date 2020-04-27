<template>
	<view>
		
		<view class="d-flex text-center orderNav color6 bg-white">
			<view class="tt" :class="curr==1?'on':''" @click="currChange('1')">未使用</view>
			<view class="tt" :class="curr==2?'on':''" @click="currChange('2')">已使用</view>
			<view class="tt" :class="curr==3?'on':''" @click="currChange('3')">已过期</view>
		</view>
		<view class="mx-3 mt-3">
			<view class="cuponList" v-show="curr==1">
				<view class="item mb-3" v-for="(item,index) in cuponList" :key="index" >
					<view class="infor">
						<view class="bj"><image src="../../static/images/couponsl-img.png" mode=""></image></view>
						<view class="d-flex j-sb a-center">
							<view class="left d-flex j-center a-center">
								<view class="mny">100</view>
							</view>
							<view class="text pl-3">
								<view class="font-30 font-weight avoidOverflow">瑾味成</view>
								<view class="font-26 color6 mt-2">满20减5元</view>
								<view class="font-22 color9 mt-1 ">有效期：领取即日起7天</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="cuponList" v-show="curr==2">
				<view class="item mb-3" v-for="(item,index) in cuponList" :key="index" >
					<view class="infor">
						<view class="bj"><image src="../../static/images/couponsl-img1.png" mode=""></image></view>
						<view class="fixState"><image src="../../static/images/couponsl-img3.png" mode=""></image></view>
						<view class="d-flex j-sb a-center">
							<view class="left d-flex j-center a-center">
								<view class="mny">100</view>
							</view>
							<view class="text pl-3">
								<view class="font-30 font-weight avoidOverflow">瑾味成</view>
								<view class="font-26 color6 mt-2">满20减5元</view>
								<view class="font-22 color9 mt-1 ">有效期：领取即日起7天</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="cuponList" v-show="curr==3">
				<view class="item mb-3" v-for="(item,index) in cuponList" :key="index" >
					<view class="infor">
						<view class="bj"><image src="../../static/images/couponsl-img1.png" mode=""></image></view>
						<view class="fixState"><image src="../../static/images/couponsl-img4.png" mode=""></image></view>
						<view class="d-flex j-sb a-center">
							<view class="left d-flex j-center a-center">
								<view class="mny">100</view>
							</view>
							<view class="text pl-3">
								<view class="font-30 font-weight avoidOverflow">瑾味成</view>
								<view class="font-26 color6 mt-2">满20减5元</view>
								<view class="font-22 color9 mt-1 ">有效期：领取即日起7天</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据时显示 -->
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
				cuponList:[{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			currChange(curr){
				const self = this;	
				if(curr!=self.curr){
					self.curr = curr
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/cupon.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;}
	
</style>
