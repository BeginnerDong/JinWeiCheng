<template>
	<view class="">
		<view class="myRowBetween">
			<view class="item d-flex j-sb a-center">
				<view class="ll">姓名</view>
				<view class="rr fs13">
					<input type="text" placeholder="请输入姓名" placeholder-class="placeholder">
				</view>
			</view>	
			<view class="item d-flex j-sb a-center">
				<view class="ll">手机号</view>
				<view class="rr fs13" >
					<input type="number" placeholder="请输入手机号" placeholder-class="placeholder">
				</view>
			</view>	
			<view class="item d-flex j-sb a-center">
				<view class="ll">所在地区</view>
				<view class="rr fs13 color9">
					请选择<image class="arrowR" src="../../static/images/detailsl-icon1.png" mode=""></image>
				</view>
			</view>	
			<view class="item d-flex j-sb a-center">
				<view class="ll">详细地址</view>
				<view class="rr fs13" >
					<input type="text" placeholder="请填写" placeholder-class="placeholder">
				</view>
			</view>	
			<view class="item d-flex j-sb a-center">
				<view class="ll">设为默认地址</view>
				<view class="rr">
					<switch style="transform:scale(0.75);" color="#FE7F45" />
				</view>
			</view>	
			
			<view class="submitbtn pdtb15"  style="margin-top: 200rpx;">
				<button class="btn" type="button"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">新增</button>
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
				is_show:false,
				score: '',
				wx_info: {}
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = ''
		},

		methods: {
			setDefault(){
				const self = this;
				self.is_show=!self.is_show
			},
			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.myRowBetween .item{padding:30rpx 4%}
</style>
