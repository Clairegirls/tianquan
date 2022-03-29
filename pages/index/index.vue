<template>
	<view class="pay_content">
		<view class="pay_tit">{{options.title}}</view>
		<view class="pay_amount">￥{{options.total_fee}}</view>
		<view class="product_name"><view>商品名称</view><view>{{options.content}}</view></view>
		<button class="btn pay_btn" type="primary" @click="pay">立即支付</button>
		<!-- <button class="btn pay_btn" @click="wxPays">重新支付</button> -->
		<!-- <button class="btn pay_btn" @click="wxPays">完成</button> -->
		<button class="btn cancel_btn" open-type="launchApp" app-parameter="{type:0,success:false}" binderror="launchAppError">取消</button>
	</view>
</template>

<script>
	var util = require('../../common/util.js')
	export default {
		data() {
			return {
				openid:'',
				options:{title:'能量',total_fee:1},
				pay_type:true,
			}
		},
		onLoad() {
			// // 微信登录获取openid
			var pages = getCurrentPages(); 				//获取加载的页面
			var currentPage = pages[pages.length - 1] 	//获取当前页面的对象
			var options = currentPage.options 		//获取当前页面的参数			
			console.log(currentPage)
			console.log(options)
			console.log(JSON.stringify(options))
			// var obj = wx.getLaunchOptionsSync()
			// console.log(obj)
			// console.log(obj.scene)
			// console.log(this)
			this.options = options
			this.options.title = options.title?options.title:this.options.title
			
			var that = this
			wx.login({
			  success (res) {
			    if (res.code) {
			      wx.request({
			        url: util.getApi() +'/passport/getWechatOpenId',
					method:'post',
					header:{
						 'content-type':'application/x-www-form-urlencoded',
					},
			        data:'code='+res.code,
					success: function(res) {
						console.log(res.data)
						if(res.data.code==1){
							if(res.data.data.hasOwnProperty('openid')){
								that.openid = res.data.data.openid
							}else{
								that.toast(res.data.data.errmsg)
							}
							
						}else{
							that.toast(res.data.msg)
						}
					},
					fail:function(res){
						console.log(res);
						that.toast(res.data.data.errmsg)
					}
			      })
			    } else {
					that.toast('登录失败！' + res.errMsg)
			    }
			  }
			})
		},
		methods: {
			pay(){
				if(this.pay_type){
					this.wxPays()
				}
			},
			toast(msg){
				wx.showToast({
				  title: msg,
				  icon: 'error',
				  duration: 2000
				})
			},
			launchAppError (e) {
			    console.log(e)
			  },
			wxPays(){
				var that = this
				var pages = getCurrentPages(); 				//获取加载的页面
				var currentPage = pages[pages.length - 1] 	//获取当前页面的对象
				var options = currentPage.options 		//获取当前页面的参数	
				var option1 = {
					fs:'1',
					a:'2',
					version_code:'111',
					d:'3'
				}
				console.log('option1拼接')
				console.log(util.JSON_to_URLEncoded(option1));
				console.log('请求options拼接')
				console.log(util.JSON_to_URLEncoded(options)+'&openid='+that.openid)
				that.pay_type = false
				wx.request({
				  url: util.getApi() +'/pay/wxLittleAppPay',
				  method:'post',
				  data: util.JSON_to_URLEncoded(options)+'&openid='+that.openid,
				  header: {
				    'content-type':'application/x-www-form-urlencoded',
				  },
				  success (res) {
					console.log('下单下单啦')
				    console.log(res)
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
								  url: 'success?content='+ that.options.content
								})
						    },
						    fail: function (res) {
						        console.log('fail:' + JSON.stringify(res));
								that.toast(res.data.data.errmsg)
						    }
						});
					}else{
						that.toast(res.data.msg)
						// wx.redirectTo({
						//   url: 'fail?openid='+that.openid+'&content='+ that.options.content
						// })
					}
					that.pay_type = true
				  },
				  fail(err) {
					  that.toast(err)
					console.log(err)
					that.pay_type = true
				  }
				})
			},
		
		}
	}
</script>

<style>
	@font-face {
	    font-family: MyFontName;
	    src: url(DIN\ Alternate\ Bold.ttf)
	}
	body{
		background: #F5F5F5;
	}
	.pay_tit{
		margin: 40px 0 12px;
		font-size: 18px;
		font-weight: 500;
		color: #333333;
		line-height: 25px;
	}
	.pay_amount{
		font-size: 36px;
		font-family: MyFontName;
		font-weight: bold;
		color: #333333;
		line-height: 42px;
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
</style>
