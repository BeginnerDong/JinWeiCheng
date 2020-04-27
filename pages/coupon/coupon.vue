<template>
	<view>
		
		<view class="mx-3 mt-3">
			<view class="cuponList" >
				<view class="item mb-3" v-for="(item,index) in cuponList" :key="index"  @click="seltCurrChange(index)">
					<view class="infor">
						<view class="bj"><image src="../../static/images/couponsl-img.png" mode=""></image></view>
						<view class="d-flex j-sb a-center">
							<view class="left d-flex j-center a-center">
								<view class="mny">100</view>
							</view>
							<view class="text pl-3">
								<view class=" position-absolute top-0 right-0 mt-3 mr-3 takeBtn line-h-lg rounded50 main-bg-color text-white px-2 font-24" @click="toast4Tap">点击领取</view>
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
				cuponList:2
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			toast4Tap: function () {
				uni.showToast({
					title: "领取成功"
				})
			},
			seltCurrChange(index){
				const self = this;
				self.seltCurr = index
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/cupon.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;}
	
	
		
</style>
