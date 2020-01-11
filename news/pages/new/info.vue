<template>
	<view class="content">
		<view class="userinfo">
			<view class="avatar">
				<image :src="newInfo.author_avatar" style="border-radius: 50%;width: 50px;height: 50px;"></image>
			</view>
			<view class="title">
				{{newInfo.title}}
			</view>
			
		</view>
		<view class="date-name">
			<label>{{newInfo.author_name}}</label>
			<label style="margin-left:20upx;">{{newInfo.created_at}}</label>
		</view>
		<view style="display:flex;justify-content: center;">
			<h3>{{newInfo.summary}}</h3>
		</view>
		<view>
			 <rich-text :nodes="newInfo.content"></rich-text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				newInfo:{}
			}
		},
		onLoad:function(option) {
			uni.request({
				url: 'https://unidemo.dcloud.net.cn/api/news/36kr/'+option.newsid,
				method: 'GET',
				data: {},
				success: res => {
					this.newInfo = res.data;
				},
				fail: () => {},
				complete: () => {}
			});
			
		},
		methods: {
         
		 }
	}
</script>

<style>
	.content{padding: 10upx 2%;width: 96%;flex-wrap:wrap ;}
	.userinfo{display: flex;align-items:center;}
	.avatar{border-radius: 50%;}
	.title{line-height: 2em;font-weight: 700;font-size: 38upx;}
	.date-name{color: #BEBEBE;}
</style>
