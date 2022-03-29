<template>
	<view class="pay_content">
		<image class="tip_img" src="../../static/pay/ok.png"></image>
		<view class="pay_res">充值成功</view>
		<view class="product_name"><view>商品名称</view><view>{{content}}</view></view>
		<button class="btn pay_btn" type="primary" open-type="launchApp" app-parameter="{type:0,success:true}" binderror="launchAppError">完成</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				content:'名字'
			}
		},
		onLoad() {
		},
		onShow: function() {
			wx.hideHomeButton()
			console.log('App Show')
		},
		onLoad(options) {
			console.log(options)
			this.content = options.content
		},
		methods: {
			launchAppError (e) {
			    console.log(e)
			  },	
		}
	}
</script>

<style>
	body{
		background: #F5F5F5;
	}
	.pay_content{
		text-align: center;
		padding: 0 15px;
	}
	.product_name{
		display: flex;
		justify-content: space-between;
		padding: 13px 15px;
		background: #FFFFFF;
		border-radius: 8px;
		margin: 40px 0;
		font-size: 13px;
		font-weight: 600;
		color: #333333;
	}
	button{
		border: none;
	}
	.btn{
		margin-bottom: 16px;
		height: 45px;
		line-height: 45px;
		border-radius: 8px;
	}
	.pay_btn{
		font-size: 15px;
		font-weight: 600;
		color: #FFFFFF;
	}
	.pay_res{
		font-size: 14px;
		font-weight: 600;
		color: #333333;
	}
	.tip_img{
		height: 62px;
		width: 62px;
		margin: 34px 0 10px;
	}
</style>
