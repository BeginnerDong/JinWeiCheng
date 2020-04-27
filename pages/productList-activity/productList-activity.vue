<template>
	<view>
		
		<view class="">
			<view class="productList d-flex j-sb flex-wrap mx-3">
				<view class="item rounded10 mt-3 pr" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">
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
				productData:4
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
</style>
