<template>
	<view>
		<view class="font-26 px-3">
			<view class="TopItem d-flex py-3 border-bottom">
				<view class=" d-flex a-center">
					<image class="icon" style="width: 27rpx;height: 33rpx;" src="../../static/images/contact-usl-icon.png" mode=""></image>
				</view>
				<view class="text">{{mainData.description}}</view>
			</view>
			<view class="TopItem d-flex a-center py-3">
				<view class=" d-flex a-center">
					<image class="icon" src="../../static/images/contact-usl-icon1.png" mode=""></image>
				</view>
				<view class="text">{{mainData.small_title}}</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		<view class="mx-3 py-3">
			<view class="font-30 font-weight">公司简介</view>
			<view class="xqInfor mt-2">
				<view class="cont font-26 color6">
					<view class="content ql-editor" style="padding:0;"
					v-html="mainData.content">
					</view>
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
				mainData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['联系我们']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					};
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/detail.css";
	page{padding-bottom: 60rpx;}
	
	.TopItem .icon{width: 33rpx;height:33rpx;margin-right: 20rpx;}
	.TopItem .text{width: 90%;}
	
</style>
