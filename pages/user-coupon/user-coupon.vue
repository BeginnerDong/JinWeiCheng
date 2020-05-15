<template>
	<view>
		
		<view class="d-flex text-center orderNav color6 bg-white shadow-sm">
			<view class="tt" :class="curr==1?'on':''" @click="currChange('1')">未使用</view>
			<view class="tt" :class="curr==2?'on':''" @click="currChange('2')">已使用</view>
			<view class="tt" :class="curr==3?'on':''" @click="currChange('3')">已过期</view>
		</view>
		<view class="orderNavH"></view>
		
		<view class="mx-3 mt-3">
			<view class="cuponList">
				<view class="item mb-3" v-for="(item,index) in mainData" :key="index" >
					<view class="infor">
						<view class="bj"><image :src="item.use_step==1?'../../static/images/couponsl-img.png':'../../static/images/couponsl-img1.png'" mode=""></image></view>
						<view class="fixState" v-if="item.use_step==2"><image src="../../static/images/couponsl-img3.png" mode=""></image></view>
						<view class="fixState" v-if="item.use_step==-1"><image src="../../static/images/couponsl-img4.png" mode=""></image></view>
						<view class="d-flex j-sb a-center">
							<view class="left d-flex j-center a-center">
								<view class="mny">{{item.value}}</view>
							</view>
							<view class="text pl-3">
								<view class="font-30 font-weight avoidOverflow">{{item.title}}</view>
								<view class="font-26 color6 mt-2">满{{item.condition}}减{{item.value}}元</view>
								<view class="font-22 color9 mt-1 ">有效期：{{item.invalid_time}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据时显示 -->
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
				cuponList:[{},{}],
				mainData:[],
				searchItem:{
					thirdapp_id: 2,
					use_step:1,
				}
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
			
			currChange(curr){
				const self = this;	
				if(curr!=self.curr){
					self.curr = curr;
					if(self.curr==1){
						self.searchItem.use_step = 1
					}else if(self.curr==2){
						self.searchItem.use_step = 2
					}else if(self.curr==3){
						self.searchItem.use_step = -1
					}
					self.getMainData(true)
				}
			},
			
			getMainData(isNew) {
				const self = this;
				var now =  (new Date()).getTime();
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
				postData.searchItem = self.$Utils.cloneForm(self.searchItem)
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].create_time = self.mainData[i].create_time.substr(0,10);
							self.mainData[i].invalid_time = self.$Utils.timeto(self.mainData[i].invalid_time,'ymd')
						}
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userCouponGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/cupon.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;}
	.orderNav{width: 100%;height: 80rpx;position: fixed;top: 0rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.orderNavH{height: 80rpx;}
	
</style>
