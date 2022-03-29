<template>
	<view class="pay_content">
		<image class="tip_img" src="../../static/pay/fail.png"/>
		<view class="pay_res">支付失败，重新支付</view>
		<view class="product_name"><view>商品名称</view><view>{{content}}</view></view>
		<button class="btn pay_btn" type="primary" @click="wxPays">重新支付</button>
		<button class="btn cancel_btn" open-type="launchApp" app-parameter="app_parameter" binderror="launchAppError">取消</button>
	</view>
</template>

<script>
	var util = require('../../common/util.js')
	export default {
		data() {
			return {
				app_parameter:{type:0,success:false},
				openid:'',
				content:'名字'
			}
		},
		onShow: function() {
			wx.hideHomeButton()
			console.log('App Show')
		},
		onLoad(options) {
			console.log('option11')
			console.log(options)
			this.openid = options.openid
			this.content = options.content
		},
		methods: {
			launchAppError (e) {
			    console.log(e)
			  },
			wxPays(){
				var that = this
				var pages = getCurrentPages(); 				//获取加载的页面
				var currentPage = pages[pages.length - 1] 	//获取当前页面的对象
				var options = currentPage.options 		//获取当前页面的参数	
				wx.request({
				  url: 'http://api.tianyou.test/pay/wxLittleAppPay',
				  method:'post',
				  data: util.JSON_to_URLEncoded(options)+'&openid='+that.openid,
				  header: {
				    'content-type':'application/x-www-form-urlencoded',
				  },
				  success (res) {
					console.log('下单下单啦')
				    console.log(res.data)
					if(res.data.code==1){
						var data = res.data.data.credential.pay
						uni.requestPayment({
							appId:data.appId,
						    provider: 'wxpay',
						    timeStamp: data.timeStamp,
						    nonceStr: data.nonceStr,
						    package: data.package,
						    signType: data.signType,
							paySign:data.paySign,
						    success: function (res) {
						        console.log('success:' + JSON.stringify(res));
								wx.redirectTo({
								  url: 'success'
								})
						    },
						    fail: function (err) {
						        console.log('fail:' + JSON.stringify(err));
								wx.showToast({
								  title: err,
								  icon: 'error',
								  duration: 2000
								})
						    }
						});
					}else{
						wx.showToast({
						  title: res.data.msg,
						  icon: 'error',
						  duration: 2000
						})
						wx.redirectTo({
						  url: 'fail?openid='+that.openid
						})
					}
					
				  },
				  fail(err) {
				  	console.log(err)
					wx.showToast({
					  title: err,
					  icon: 'error',
					  duration: 2000
					})
				  }
				})
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
	button::after{ border: none; }
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
	.cancel_btn{
		background: #FFFFFF;
		font-size: 15px;
		font-weight: 600;
		color: #333333;
	}
	.tip_img{
		height: 62px;
		width: 62px;
		margin: 34px 0 10px;
	}
</style>
