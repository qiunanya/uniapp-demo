<template>
	<view class="content">
		<view class="pr-box">
			<view class="box-item uni-flex" >
				<view @tap="openProjectDetail(item.goodsId)" class="pr-list" v-for="(item,index) in collectJson" :key="index">
					<image :src="item.goodsImage" mode="aspectFill"></image>
					<view>
						<view class="title">{{item.goodsName}}</view>
						<view class="pirce">浏览量8888：{{item.goodsVisit}}</view>
						<view class="pirce">时间:{{item.videoTime}}</view>
					</view>
				</view>
			</view>
			<uni-load-more v-if="isButtom" :status="status" :content-text="contentText" color="#007aff" />
		</view>
	</view>
</template>


<script>
	import uniLoadMore from '@/components/uni-load-more/uni-load-more.vue'
	export default {
		components: {
			uniLoadMore
		},
		data(){
			return{
				page:1,
				limit:6,
				collect:false,
				collectJson:[],
				isButtom:false,
				status: 'more',
				contentText: {
					contentdown: '上拉显示更多',
					contentrefresh: '正在加载中...',
					contentnomore: '--没有更多数据了--'
				}
			}
		},
		onShow() {
			uni.$on("cancelCollect",(e)=>{
				if(e.bool){
					this.getMyCollection()
				}
			})
		},
		onLoad() {
			this.getMyCollection()
		},
		onPullDownRefresh(){
			this.status === 'more';
			this.page = 1;
			this.collectJson = [],
			this.getMyCollection()
		},
		onReachBottom(){
			this.isButtom = true;
			if(this.status === 'more'){
				this.status = "loading";
				this.page ++;
				this.getMyCollection();
			}else if(this.status === 'noMore'){
				setTimeout(()=>{this.isButtom = false;this.$forceUpdate()},5000)
			}
		},
		methods:{
			openProjectDetail(goodsId){
				uni.navigateTo({
					url:'/pages/goods/goods-detail?goodsId='+goodsId
				}); 
			},
			getMyCollection(){
				uni.request({
					url: this.API.Customer.getMyGoodsCollection,
					data: {
						userId:this.Util.getUid(),
						pageNum:this.page,
						pageSize:this.limit,
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded',
						'accessToken': this.Util.getToken()
					},
					dataType: "json",
					method: 'POST',
					success:(res)=>{
						let response = res.data;
						if(response.code == 200){
							this.collectJson = response.data.list;
							this.status = "more";
							uni.hideLoading()
							uni.stopPullDownRefresh()
						} else {
							this.status = "noMore";
							this.page = 1;
							uni.stopPullDownRefresh()
						}
					},
					fail: (res)=>{
						this.Util.requestFail(res);
					},
					complete:(pding)=>{
						if(pding){
							uni.stopPullDownRefresh();
						}
					}
				})
			}
		}
	}
</script>

<style>
	page,.content{
		background-color: #F1F1F1;
	}
</style>
<style lang="scss" scoped>
	view{color: #555;font-size: 26rpx;}
	.list-box-item image,.pr-list image{background: #F1F1F1;}
	.js-box-list{
		background: #FFFFFF;
		margin-bottom: 10rpx;
		padding: 10px 15px;
		box-sizing: border-box;
		
		.nav-box{
			box-sizing: border-box;
			padding-top: 20px;
			position: relative;
			&:after{
				position: absolute;
				content: "";
				width: 100%;
				height: 1px;
				top: 20rpx;
				background: #e5e5e5;
			}
			.uni-flex>view{
				flex: 1;
				display: flex;
				align-items: center;
				
				image{
					width: 26rpx;
				    height: 26rpx;
				    display: block;
				    margin: 6rpx;
				}
				&:last-child{
					color: #15EA12;
				}
			}
			
			view:last-child{
				color: #007AFF;
				.loaction{
					margin-right: 40rpx;
					position: relative;
					
					&:before{
						position: absolute;
						content: "";
						height: 85%;
						width: 1px;
						right: -20rpx;
						background: #C0C0C0;
						top: 10%;
					}
				}
			}
		}
		image{
		    width: 160rpx;
		    height: 170rpx;
		    display: block;
		}
		.right-box{
			box-sizing: border-box;
			padding-left: 30rpx;
			width: 550rpx;
			display: flex;
		    flex-flow: column;
		    justify-content: center;
			.title{
				font-size: 32rpx;
				color: #585858;
				font-weight: 600;
				text{
					color: #007AFF;
					font-size: 26rpx;
					margin-left: 20rpx;
				}
			}
			.contents{
				font-size: 26rpx;
				color: #585858;
			}
			.status-box{
				margin: 14rpx 0;
				color: #585858;
				.status{
				    display: inline-block;
				    color: #32a6ff;
				    border: 1rpx solid #32a6ff;
				    text-align: center;
				    font-size: 26rpx;
				    height: 36rpx;
				    line-height: 20rpx;
				    border-radius: 8rpx;
				    box-sizing: border-box;
				    padding: 8rpx 10rpx;
					margin-right: 10rpx;
				}
			}
		}
	}
	.pr-box{
		box-sizing: border-box;
		padding: 120rpx 0; 
		.box-item{
			flex-wrap: wrap;
			box-sizing: border-box;
			padding: 20rpx 30rpx;
			background: #fff;
			
			.pr-list{
				width: 335rpx;
				margin-right: 20rpx;
				margin-bottom: 20rpx;
				background: #fff;
				
				image{
					width: 335rpx;
					height: 335rpx;
					display: block;
				}
				
				&:nth-child(even){
					margin-right: 0%;
				}
				
			}
			
		}
		
		.pr-list>view{
			padding: 10rpx;
			.contents{
				font-size: 26rpx;
				color: #585858;
				margin: 10rpx 0;
			}
			.pirce{
				color: #feaa27;
				font-size: 32rpx;
				font-weight: bold;
			}
			.title{
				font-size: 32rpx;
				font-weight: 600;
				color: #000000;
				white-space: nowrap;
				overflow: hidden;
				text-overflow: ellipsis;
			}
		}
	}
	.ellipsis{
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}
</style>
