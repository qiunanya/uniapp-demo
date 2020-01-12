<template name='my-input'>
	<view>
		<input v-model="age" type="text" value="" :placeholder="placeholder" class="my-input"/>
		<button type="primary" @click="submitForm">提交</button>
	</view>
</template>

<script>
	export default {
		name:'my-input',
		data() {
			return {
				age:'',
			};
		},
		props:{
			placeholder:{
				type:String,
		        default:"请输入姓名..."
			},
		},
		created() {
			console.log('created')
		},
		watch:{},
		methods:{
			submitForm(){
				this.$emit('getValue',this.age)
			},
			setValue(val){
				this.age = val;
			}
		},
		computed:{
			
		},
		mounted() {
			console.log('mounted')
		}
	}
</script>

<style lang="less" scoped>
.my-input{
	padding: 20rpx;
	background: #F4F5F6;
}
</style>
