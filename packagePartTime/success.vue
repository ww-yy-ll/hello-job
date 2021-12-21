<template>
	<view class="page">
		<view class="bg-fff p-40 row center">
			<u-icon name="checkmark-circle" size="50" color="#F56718"></u-icon>
			<view class="m-l-20">
				<view class="f-s-28 color m-b-10">投递成功，企业将收到你的申请</view>
				<view class="f-s-24 c9">今日还可报名<text class="main-color">99</text>次</view>
			</view>
		</view>
		<view class="m-t-40 p-30">
			<u-button text="告诉好友" shape="circle" color="#F56718" size="large" class="m-b-30" @click=""></u-button>
			<u-button text="回到首页" shape="circle" color="#F56718" size="large" plain class="m-b-30" @click=""></u-button>
			<u-button text="添加到我的小程序,实时查看报名情况" shape="circle" color="#F56718" plain class="m-b-30" size="large" @click=""></u-button>
		</view>
		
		
		<view class="column-center m-t-40">
			<u--image :showLoading="false" src="../static/images/icon/index/search-more.png" width="180rpx" height="180rpx" @click=""></u--image>
			<view class="m-t-30 f-s-22 c9">长按保存二维码，下载兼职猫APP</view>
			<view class="m-t-10 f-s-22 c9">更多的兼职请上兼职猫</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">

</style>
