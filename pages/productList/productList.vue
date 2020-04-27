<template>
	<view>
		
		<view class="">
			<view class="">
				<scroll-view scroll-x class="whiteBj color6 boxShaow scroll-row orderNav shadow-sm"  >
					<view class="tt scroll-row-item mx-3" v-for="(item,index) in navList" :key="index" :class="curr==index?'on':''" @click="changeCurr(index)">{{item}}</view>
				</scroll-view>
			</view>
			<view class="orderNavH"></view>
			
			<view class="productList d-flex j-sb flex-wrap mx-3">
				<view class="item rounded10 mt-3" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">
					<view class="pic"><image src="../../static/images/home-img1.png" mode=""></image></view>
					<view class="infor">
						<view class="tit avoidOverflow2 font-30">墨西哥牛油果8枚单果200g左右</view>
						<view class="price font-32 font-weight mt-2">56</view>
					</view>
				</view>
			</view>
			
			<!-- 无数据 -->
			<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				wx_info:{},
				is_show:false,
				curr:0,
				navList:['全部','服装','饰品','箱包','家具','数码','生活','母婴'],
				productData:5
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(index){
				const self = this;
				self.curr = index
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
	@import "../../assets/style/productList.css";
	
	page{padding-bottom: 60rpx;background-color: #F5F5F5;}	
	.orderNav{position: fixed; left:0; right: 0;top: 0; z-index: 10;}
	.orderNavH{height: 80rpx;}
	.orderNav .tt{width: auto;}
	.orderNav .tt.on{color: #222;font-size: 28rpx;font-weight: bold;}
</style>
