<template>
	<view class="page bg-fff">
		<u-sticky :offset-top="sysNav.top + sysNav.height">
			<view class="bottom-line p-30">
				<u-search placeholder="请输入学校" v-model="keyword" shape="round" :showAction="true" :clearabled="true" height="54"></u-search>
			</view>
		</u-sticky>
		
		<view class="">
			<view class="p-30 f-s-26" v-for="(item,index) in 30" :key="index" @click="setSchool(item)">
				广西民族大学
			</view>
		</view>
	</view>
</template>

<script>
	import { mapState, mapMutations } from 'vuex'
	export default {
		data() {
			return {
				keyword: '', // 搜索框绑定字段
				list:[]
			};
		},
		computed:{
			...mapState(['sysNav', 'eduVals'])
		},
		watch:{
			keyword(){
				const { keyword } = this
				// if(keyword.length)
			}
		},
		onReachBottom() {
			
		},
		methods: {
			...mapMutations(['SET_EDUVALS']),
			setSchool(item) {
				// console.log(item);
				let data = {
					school: '广西民族大学'
				}
				this.SET_EDUVALS(data)
				uni.navigateBack()
			}
		}
	}
</script>

<style lang="scss">

</style>
