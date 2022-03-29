<template>
	<view class="home_content">
		<view class="home_top">
			<image class="logo_icon" src="../../static/home/logo.png"></image>
			<view class="home_tit">贵族进场列表一览</view>
		</view>
		<view>
			<view v-for="(item,index) in content" :key="index">
				<view>{{item.title}}</view>
				<view class="item_img">
					<image :src="item.img1" mode="widthFix"></image>
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
					{title:'白银贵族',img1:'../../static/home/1.png'},
					{title:'黄金贵族',img1:'../../static/home/2.png'},
					{title:'铂金贵族',img1:'../../static/home/3.png'},
					{title:'宝石贵族',img1:'../../static/home/4.png'},
					{title:'钻石贵族',img1:'../../static/home/5.png'},
					{title:'至尊贵族',img1:'../../static/home/6.png'}
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
		justify-content: center;
	}	
	.item_img image{
		margin: 25px 0;
		/* height: 651px; */
		width: 370px;
	}
</style>
