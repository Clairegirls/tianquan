<template>
	<view class="home_content">
		<view class="home_top">
			<image class="logo_icon" src="../../static/home/logo.png"></image>
			<view class="home_tit">甜圈UI预览</view>
			<button class="check_img" @click="enterImg">查看甜圈图片资源</button>
		</view>
		<view>
			<view v-for="(item,index) in content"  :key="index">
				<view>{{item.title}}</view>
				<view class="item_img">
					<image :src="item.img1"></image>
					<image :src="item.img2"></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				content:[
					{title:'列表页',img1:'../../static/home/lie1.png',img2:'../../static/home/lie2.png'},
					{title:'直播间',img1:'../../static/home/zhi1.png',img2:'../../static/home/zhi2.png'},
					{title:'语音房',img1:'../../static/home/yu1.png',img2:'../../static/home/yu2.png'},
					{title:'弹框',img1:'../../static/home/tan1.png',img2:'../../static/home/tan2.png'},
				]
			}
		},
		onShow: function() {
		},
		onLoad(options) {
		},
		methods: {
			enterImg(){
				wx.redirectTo({
				  url: './enter'
				})
			}
		}
	}
</script>

<style>
	.check_img{
		font-weight: normal;
		font-size: 12px;
		width: 50%;
		margin-top: 10px;
		color: #5A57F7;
	}
	.home_content{
		text-align: center;
		margin-top: 10px;
		font-size: 16px;
		font-weight: bold;
	}
	.home_top{
		margin-bottom: 20px;
	}
	.logo_icon{
		height: 120px;
		width: 120px;
		margin-bottom: 5px;
		border-radius: 15px;
	}

	.item_img{
		display: flex;
		flex-direction: row;
		justify-content: space-around;
	}	
	.item_img image{
		margin: 25px 10px;
		height: 370px;
		width: 170px;
	}
</style>
