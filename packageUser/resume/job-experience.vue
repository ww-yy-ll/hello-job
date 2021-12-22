<template>
	<view class="page p-30">
		<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
			<view class="p-b-30">
				<u--form labelPosition="top" :model="form" ref="form" :labelStyle="labelStyle">
					<u-form-item label="公司名" :required="true" prop="company" ref="company" borderBottom>
						<u--input :disabled="false" v-model="form.company" border="none" placeholder="请输入公司名"></u--input>
					</u-form-item>
					<u-form-item label="开始时间" :required="true" prop="compStartTime" borderBottom @click="openDate('compStartTime')" ref="comPStartTime">
						<u--input v-model="form.compStartTime" disabled disabledColor="#ffffff" placeholder="请选择开始时间" border="none"></u--input>
						<u-icon slot="right" name="arrow-right"></u-icon>
					</u-form-item>
					<u-form-item label="结束时间" :required="true" prop="compEndTime" borderBottom @click="openDate('compEndTime')" ref="compEndTime">
						<u--input v-model="form.compEndTime" disabled disabledColor="#ffffff" placeholder="请选择结束时间" border="none"></u--input>
						<u-icon slot="right" name="arrow-right"></u-icon>
					</u-form-item>
				</u--form>
			</view>
		</view>
		<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
			<view class="p-b-30">
				<u--form labelPosition="top" :model="form2" ref="form2" :labelStyle="labelStyle">
					<u-form-item label="具体内容" :required="true" prop="content" :borderBottom="false" ref="content">
						<u--textarea :placeholder="compPlaceholder" v-model="form2.content" count height="500rpx" maxlength="800"></u--textarea>
					</u-form-item>
				</u--form>
			</view>
		</view>
		
		<view class="p-t-b-30 f-s-22 c9 m-b-50">
			注：需填写打<text class="main-color">*</text>号的内容才能保存该模块
		</view>
		<u-button text="保存" shape="circle" :color="allCheck===true?'#F56718' : 'rgba(245,103,24,.5)'" size="large" @click="submit"></u-button>
		<u-datetime-picker ref="datetimePicker" :show="dateShow" mode="date" v-model="dateValue" :formatter="formatter" closeOnClickOverlay
			@confirm="dateConfirm" @cancel="dateClose" @close="dateClose"></u-datetime-picker>
	</view>
</template>

<script>
	import { mapState } from 'vuex'
	export default {
		data() {
			return {
				allCheck: false, // 全部校验完毕
				form:{
					company: '',
					compStartTime: '',
					compEndTime: '',
					content:''
				},
				form2:{
					content:''
				},
				compPlaceholder: `请输入您的工作经验或实习经验`,
				labelStyle:{
					fontSize:'26rpx',
					color: '#999',
					width:'200rpx',
					marginLeft: '10rpx'
				},
				selectKey:'school', // 选择菜单
				selectShow: false,
				// 时间选择器
				dateKey:'', 
				dateValue: Number(new Date()),
				dateShow: false,
				// 日历
				showCalendar: false,
				compRules: {
					company: {
						type: 'string',
						required: true,
						message: '请输入公司名',
						trigger: ['blur', 'change']
					},
					compStartTime: {
						type: 'string',
						required: true,
						message: '请输入开始时间',
						trigger: ['blur', 'change']
					},
					compEndTime: {
						type: 'string',
						required: true,
						message: '请输入结束时间',
						trigger: ['blur', 'change']
					}
				},
				compRules2: {
					content: {
						type: 'string',
						min: 10,
						required: true,
						message: '请输入您的工作具体内容',
						trigger: ['change']
					}
				},
				
			};
		},
		computed:{
			...mapState(['eduVals']),
		},
		watch:{
			'form.company'(){
				const { company, compStartTime, compEndTime } = this.form
				if(company.length!==0 && compStartTime.length!==0 && compEndTime.length!==0 && content.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form.compStartTime'(){
				const { company, compStartTime, compEndTime } = this.form
				const { content } = this.form2
				if(company.length!==0 && compStartTime.length!==0 && compEndTime.length!==0 && content.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form.compEndTime'(){
				const { company, compStartTime, compEndTime } = this.form
				const { content } = this.form2
				if(company.length!==0 && compStartTime.length!==0 && compEndTime.length!==0 && content.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form2.content'(){
				const { company, compStartTime, compEndTime } = this.form
				const { content } = this.form2
				if(company.length!==0 && compStartTime.length!==0 && compEndTime.length!==0 && content.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
		},
		onReady() {
			// 微信小程序需要用此写法
			this.$refs.datetimePicker.setFormatter(this.formatter)
			this.$refs.form.setRules(this.compRules)
			this.$refs.form2.setRules(this.compRules2)
		},
		onLoad(e) {},
		methods: {
			init(){
				return {
					
				}
			},
			// 日历
			calendarConfirm(e) {
				console.log(e);
				this.showCalendar = false
				this.form[this.dateKey] = `${e[0]} / ${e[e.length - 1]}`
				// this.$refs.form1.validateField('hotel')
			},
			calendarClose() {
				this.showCalendar = false
				// this.$refs.form1.validateField('hotel')
			},
			// 打开日期
			openDate(key){
				// console.log('key', key);
				this.dateKey = key
				this.dateShow = true; 
				// this.showCalendar = true; 
				this.hideKeyboard()
			},
			// 日期格式化
			formatter(type, value) {
				if (type === 'year') {
					return `${value}年`
				}
				if (type === 'month') {
					return `${value}月`
				}
				if (type === 'day') {
					return `${value}日`
				}
				return value
			},
			// 关闭选择日期
			dateClose(show) {
				this.dateShow = false
				// this.$refs.form.validateField('birthday')
			},
			// 确认选择日期
			dateConfirm(e) {
				this.dateShow = false
				this.dateValue = uni.$u.timeFormat(e.value, 'yyyy-mm-dd')
				this.form[this.dateKey] = this.dateValue
				// this.form[this.dateKey] = this.dateValue
				// console.log('e', e, this.dateValue, this.form);
				// this.userInfo.birthday = uni.$u.timeFormat(e.value, 'yyyy-mm-dd')
				// this.$refs.form.validateField('birthday')
			},
			hideKeyboard() {
				uni.hideKeyboard()
			},
			submit(){
				console.log('submit--', this.form);
				this.$refs.form.validate().then(res => {
					uni.$u.toast('form校验通过')
					this.$refs.form2.validate().then(res => {
						uni.$u.toast('form2校验通过')
					}).catch(errors => {
						uni.$u.toast('请输入您的工作具体内容~')
					})
				}).catch(errors => {
					uni.$u.toast('form校验失败')
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	/deep/.u-border {
		border: none;
	}
	/deep/.u-radio-group--row {
    flex-wrap: wrap;
	}
	.allTime-box{
		width: 100%;
		/deep/.u-radio-group--row {
			align-items: flex-end;
			justify-content: flex-end;
		}
	} 
</style>
