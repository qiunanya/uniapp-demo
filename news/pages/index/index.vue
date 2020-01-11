<template>
	<view class="content">
		<view class="uni-list">
			<view @tap="goNewInfo" :data-newsid='item.post_id' class="uni-list-cell" hover-class="uni-list-cell-hover" 
			v-for="(item,index) in newList" :key="index">
				<view class="uni-media-list">
					<image class="uni-media-list-logo" :src="item.author_avatar"></image>
					<view class="uni-media-list-body">
						<view class="uni-media-list-text-top">{{item.title}}</view>
						<view class="uni-media-list-text-bottom uni-ellipsis">{{item.created_at}}</view>
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
				newList:[]
			}
		},
		onLoad() {
          uni.request({
          	url: 'https://unidemo.dcloud.net.cn/api/news',
          	method: 'GET',
          	data: {},
          	success: res => {
			
				this.newList = res.data;
			},
          	fail: () => {},
          	complete: () => {}
          });
		},
		methods: {
          goNewInfo(e){
			let newid = e.currentTarget.dataset.newsid;  
			uni.navigateTo({
				url:'../new/info?newsid='+newid
			});
		  }
		}
	}
</script>

<style>
	/* 新闻列表api：https://unidemo.dcloud.net.cn/api/news 
	   详情：https://unidemo.dcloud.net.cn/api/news/36kr/+id(新闻id) 
	   post_id:新闻id
	   title:标题
	   created_at；创建时间
	   author_avatar 图标
	*/
   .uni-media-list-body{
	   height: auto;
   }
   .uni-media-list-text-top{
	   line-height: 1.6em;
   }
</style>
