<template>
	<view>
		
		<view class="">
			<view class="">
				<scroll-view scroll-x class="whiteBj color6 boxShaow scroll-row orderNav shadow-sm"  >
					<view class="tt scroll-row-item mx-3" @click="change(-1)"  
					:class="!searchItem.category_id?'on':''" >全部</view>
					<view class="tt scroll-row-item mx-3" @click="change(item.id)" v-for="(item,index) in labelData" :key="index" 
					:class="searchItem.category_id==item.id?'on':''" >{{item.title}}</view>
				</scroll-view>
			</view>
			<view class="orderNavH"></view>
			
			<view class="productList d-flex j-sb flex-wrap mx-3">
				<view class="item rounded10 mt-3"  v-for="(item,index) in mainData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
					<view class="infor">
						<view class="tit avoidOverflow2 font-30">{{item.title}}</view>
						<view class="price font-32 font-weight mt-2">{{item.price}}</view>
					</view>
				</view>
			</view>
			
			<!-- 无数据 -->
			<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				is_show: false,
				wx_info:{},
				productData:[{},{},{},{},{}],
				labelData:[],
				searchItem:{
					thirdapp_id:2,
					
				},
				today:0,
				mainData:[],
				idArray:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.searchItem.category_id = options.id
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getLabelData'], self);
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
			
			change(id){
				const self = this;
				if(self.searchItem.category_id!=id){
					if(id==-1){
						self.searchItem.category_id = ['in',self.idArray]
					}else{
						self.searchItem.category_id = id;
					}
					self.getMainData(true)
				}
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3,
					parentid:1
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data);
						for (var i = 0; i < self.labelData.length; i++) {
							self.idArray.push(self.labelData[i].id)
						}
					}
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				var now = new Date().getTime();
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem)
				
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.productGet(postData, callback);
			},
			
			
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/productList.css";
	
	page{padding-bottom: 60rpx;background-color: #F5F5F5;}	
	.orderNav{position: fixed; left:0; right: 0;top: 0; z-index: 10;}
	.orderNavH{height: 80rpx;}
	.orderNav .tt{width: auto;}
	.orderNav .tt.on{color: #222;font-size: 28rpx;font-weight: bold;}
</style>
