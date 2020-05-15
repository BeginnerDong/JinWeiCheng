<template>
	<view>
		
		<view class="mx-3 mt-3">
			<view class="cuponList" >
				<view class="item mb-3" v-for="(item,index) in mainData" :key="index">
					<view class="infor">
						<view class="bj"><image src="../../static/images/couponsl-img.png" mode=""></image></view>
						<view class="d-flex j-sb a-center">
							<view class="left d-flex j-center a-center">
								<view class="mny">{{item.value}}</view>
							</view>
							<view class="text pl-3">
								<view class="position-absolute top-0 right-0 mt-3 mr-3 takeBtn line-h-lg rounded50 main-bg-color text-white px-2 font-24" @click="submit(index)">点击领取</view>
								<view class="font-30 font-weight avoidOverflow">{{item.title}}</view>
								<view class="font-26 color6 mt-2">满{{item.condition}}减{{item.value}}元</view>
								<view class="font-22 color9 mt-1 ">有效期：领取即日起{{item.valid_time}}天</view>
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
				mainData:[]
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
			
			submit(index){
				const self = this;
				self.orderList = [
					{coupon_id:self.mainData[index].id,count:1,type:self.mainData[index].type}
				];
				self.couponAdd()
			},
			
			couponAdd() {
				const self = this;
				var now =  (new Date()).getTime();
				const postData = {
					tokenFuncName: 'getProjectToken',
				};
				postData.couponList = self.$Utils.cloneForm(self.orderList);
				postData.pay = {
					score: 0
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.$Utils.showToast('领取成功', 'none')
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.couponAdd(postData, callback);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.couponGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/cupon.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;}
	
	
		
</style>
