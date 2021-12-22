<template>
	<view class="page p-30">
		<view class="b-r-12 box-shadow bg-fff p-30">
			<view class="row center p-25 b-r-12 m-b-20 bg-F8F8F8 f-s-26">当前绑定的手机号：<text class="main-color">139****01</text>
			</view>
			<view class="p-30">
				<u--form labelPosition="top" :model="form" ref="form" :labelStyle="labelStyle">
					<u-form-item label="手机号" :required="true" prop="phone" borderBottom ref="phone">
						<u--input type="number" maxlength="11" v-model="form.phone" border="none" placeholder="请输入手机"></u--input>
					</u-form-item>
					<u-form-item label="验证码" :required="true" prop="code" borderBottom ref="code">
						<u--input v-model="form.code" type="number" maxlength="6" border="none" placeholder="请填写验证码"></u--input>
						<u-button slot="right" @tap="getCode" :text="tips" class="custom-style" :disabled="disabled"></u-button>
					</u-form-item>
				</u--form>
				<u-code ref="uCode" @change="codeChange" seconds="60" @start="disabled = true" @end="disabled = false"></u-code>
			</view>
		</view>
		<view class="p-t-b-30 f-s-22 c9 m-t-b-30 row-center"> 更改绑定后，您可使用新的号码登录兼职猫和报名兼职 </view>
		<u-button text="保存" shape="circle" :color="allCheck===true?'#F56718' : 'rgba(245,103,24,.5)'" size="large" @click="submit"></u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				allCheck: false,
				disabled: false,
				tips: '',
				labelStyle:{
					fontSize:'26rpx',
					color: '#999',
					width:'200rpx',
					marginLeft: '10rpx'
				},
				form:{
					phone: '',
					code:''
				},
				rules: {
					phone: {
						type: 'string',
						required: true,
						message: '请输入手机号',
						trigger: ['blur', 'change']
					},
					code: {
						type: 'string',
						required: true,
						message: '请输入验证码',
						trigger: ['blur', 'change']
					}
				},
			};
		},
		watch:{
			'form.phone'(){
				const { phone, code } = this.form
				if(phone.length!==0 && code.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form.code'(){
				const { phone, code } = this.form
				if(phone.length!==0 && code.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			}
		},
		onReady() {
			this.$refs.form.setRules(this.rules)
		},
		methods:{
			codeChange(text) {
				this.tips = text;
			},
			getCode() {
				if (this.$refs.uCode.canGetCode) {
					// 模拟向后端请求验证码
					uni.showLoading({
						title: '正在获取验证码'
					})
					setTimeout(() => {
						uni.hideLoading();
						// 这里此提示会被this.start()方法中的提示覆盖
						uni.$u.toast('验证码已发送');
						// 通知验证码组件内部开始倒计时
						this.$refs.uCode.start();
					}, 2000);
				} else {
					uni.$u.toast('倒计时结束后再发送');
				}
			},
			submit(){
				console.log('submit--', this.form);
				this.$refs.form.validate().then(res => {
					uni.$u.toast('校验通过')
				}).catch(errors => {
					uni.$u.toast('校验失败')
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.custom-style {
			border: none;
			background-color: #fff;
			color: #F56718;
		}
</style>
