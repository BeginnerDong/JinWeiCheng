<template>
	<view>
		<view class="myExtendTop text-center flexColumn main-bg-color text-white px-3">
			<view class="money ftw">{{userInfoData.score}}</view>
			<view class="fs13 pdt10 d-flex j-center a-center mt-3"><image class="mr-1" style="width: 34rpx;height: 34rpx;" src="../../static/images/integrall-icon.png" mode=""></image>积分</view>
		</view>
		<view class="">
			<view class="myRowBetween mx-3" >
				<view class="item d-flex j-sb a-center" v-for="(item,index) in mainData" :key="index">
					<view class="ll">
						<view class="">{{item.trade_info}}</view>
						<view class="font-26 color6 mt-1">{{item.create_time}}</view>
					</view>
					<view class="rr red">{{item.count}}</view>
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
				
				userInfoData:{},
				searchItem:{
					thirdapp_id:2,
					type:3,
				},
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
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
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if(isNew){
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.$Utils.finishFunc('getMainData');
						
				};
				self.$apis.flowLogGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.myExtendTop{padding-top:90rpx;height: 340rpx;}
	.myExtendTop .money{font-size:110rpx; line-height:110rpx;}
	.myRowBetween .item .rr{font-size: 30rpx;}
</style>
