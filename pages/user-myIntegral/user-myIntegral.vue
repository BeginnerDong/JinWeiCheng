<template>
	<view>
		<view class="myExtendTop text-center flexColumn main-bg-color text-white px-3">
			<view class="money ftw">126</view>
			<view class="fs13 pdt10 d-flex j-center a-center mt-3"><image class="mr-1" style="width: 34rpx;height: 34rpx;" src="../../static/images/integrall-icon.png" mode=""></image>积分</view>
		</view>
		<view class="">
			<view class="myRowBetween mx-3" >
				<view class="item d-flex j-sb a-center" v-for="(item,index) in spendData" :key="index">
					<view class="ll">
						<view class="">购买商品</view>
						<view class="font-26 color6 mt-1">2020.01.17</view>
					</view>
					<view class="rr red">+5</view>
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
				spendData:5
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.myExtendTop{padding-top:90rpx;height: 340rpx;}
	.myExtendTop .money{font-size:110rpx; line-height:110rpx;}
	.myRowBetween .item .rr{font-size: 30rpx;}
</style>
