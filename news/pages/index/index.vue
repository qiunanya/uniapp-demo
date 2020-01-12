<template>
	<view class="content">
		
		<!-- 自定义组件使用 -->
		<text style="font-size: 40rpx;color: #F76260;">1.自定义组件myInput</text>
		<myinput placeholder="请输入年龄" @getValue="getName" ref='myinput'></myinput>
		
		<!-- 新闻列表 -->
		<!-- <view class="uni-list">
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
		</view> -->
	
	
	
	</view>
</template>

<script>
	import myinput from '../../components/myInput.vue'
	export default {
		components:{
			myinput
		},
		data() {
			return {
				newList:[]
			}
		},
		onLoad() {
		  const val = '父组件传来的初始化值'
		  this.$refs.myinput.setValue(val);	
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
		 getName(val){
			console.log('子组件传值过来了---'+val) 
		 },	
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
