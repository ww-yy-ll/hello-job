<template>
	<view class="page p-30">
		<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
			<view class="p-b-30">
				<u--form labelPosition="top" :model="form" ref="form" :labelStyle="labelStyle">
					<u-form-item label="学校" :required="true" prop="school" ref="school" borderBottom>
						<view class="row-between" style="width: 100%;" @click="navigateTos('/packageUser/resume/schools')">
							<u--input :disabled="true" v-model="form.school" border="none" placeholder="请选择您的学校" disabledColor="#fff"></u--input>
							<u-icon name="arrow-right"></u-icon>
						</view>
					</u-form-item>
					<u-form-item label="主修专业" :required="false" prop="major" ref="major" borderBottom>
						<u--input :disabled="false" v-model="form.major" border="none" placeholder="请输入您的主修专业"></u--input>
					</u-form-item>
					<u-form-item label="学历" :required="true" prop="edu" ref="edu" borderBottom @click="openSelect">
						<u--input :disabled="true" v-model="form.edu" border="none" placeholder="请选择您的学历" disabledColor="#fff"></u--input>
						<u-icon slot="right" name="arrow-right"></u-icon>
					</u-form-item>
					<u-form-item label="入学时间" :required="true" prop="eduStartTime" borderBottom
						@click="openDate('eduStartTime')" ref="eduStartTime">
						<u--input v-model="form.eduStartTime" disabled disabledColor="#ffffff" placeholder="请选择入学时间" border="none"></u--input>
						<u-icon slot="right" name="arrow-right"></u-icon>
					</u-form-item>
					<u-form-item label="毕业时间" :required="true" prop="eduEndTime" borderBottom
						@click="openDate('eduEndTime')" ref="eduEndTime">
						<u--input v-model="form.eduEndTime" disabled disabledColor="#ffffff" placeholder="请选择毕业时间" border="none"></u--input>
						<u-icon slot="right" name="arrow-right"></u-icon>
					</u-form-item>
				</u--form>
			</view>
		</view>
		<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
			<view class="p-b-30">
				<u--form labelPosition="top" :model="form2" ref="form2" :labelStyle="labelStyle">
					<u-form-item label="教育经历" :required="true" prop="content" :borderBottom="false" ref="content">
						<u--textarea :placeholder="placeholder" v-model="form2.content" count height="500rpx" maxlength="800"></u--textarea>
					</u-form-item>
				</u--form>
			</view>
		</view>
		<view class="p-t-b-30 f-s-22 c9 m-b-50">
			注：需填写打<text class="main-color">*</text>号的内容才能保存该模块
		</view>
		<u-button text="保存" shape="circle" :color="allCheck===true?'#F56718' : 'rgba(245,103,24,.5)'" size="large" @click="submit"></u-button>
		
		<u-picker :show="selectShow" :columns="xueli" keyName="name" confirmColor="#F56718" @cancel="dataClose" @confirm="dataSelect"></u-picker>
		<u-calendar :show="showCalendar" mode="single" @confirm="calendarConfirm" @close="calendarClose" confirmDisabledText="请选择离店日期"></u-calendar>
		<!-- <u-action-sheet :show="selectShow" :actions="xueli" title="请选择" @close="dataClose" @select="dataSelect"></u-action-sheet> -->
		<u-datetime-picker ref="datetimePicker" :show="dateShow" mode="year-month" v-model="dateValue" :formatter="formatter" closeOnClickOverlay
			@confirm="dateConfirm" @cancel="dateClose" @close="dateClose"></u-datetime-picker>
	</view>
</template>

<script>
	import { mapState, mapMutations } from 'vuex'
	export default {
		data() {
			return {
				allCheck: false, // 全部校验完毕
				form:{ // 教育经历
					school:'',
					major:'',
					edu:'',
					eduStartTime:'',
					eduEndTime:'',
					content:''
				},
				form2:{
					content:''
				},
				placeholder:`请输入你的在校经历如示例:1.大学期间,连续四年获得年纪奖学金一等奖;2.大三下学期,在双单位xx部门实习三个月;获得最佳优秀实习生称号·`,
				labelStyle:{
					fontSize:'26rpx',
					color: '#999',
					width:'200rpx',
					marginLeft: '10rpx'
				},
				selectShow: false,
				actions:[],
				xueli: [
					[{
						name: '初中',
					},
					{
						name: '高中',
					},
					{
						name: '本科',
					},
					{
						name: '研究生',
					},
					{
						name: '博士',
					}],
				],
				// 时间选择器
				dateKey:'', 
				dateValue: Number(new Date()),
				dateShow: false,
				// 日历
				showCalendar: false,
				// 校验规则
				eduRules: {
					school: {
						type: 'string',
						required: true,
						message: '请输入您的学校',
						trigger: ['blur', 'change']
					},
					edu: {
						type: 'string',
						required: true,
						message: '请选择您的学历',
						trigger: ['blur', 'change']
					},
					eduStartTime: {
						type: 'string',
						required: true,
						message: '请输入您的入学时间',
						trigger: ['blur', 'change']
					},
					eduEndTime: {
						type: 'string',
						required: true,
						message: '请输入您的毕业时间',
						trigger: ['blur', 'change']
					},
					content: {
						type: 'string',
						min: 10,
						required: true,
						message: '请输入您的在校经历',
						trigger: ['change']
					}
				},
				eduRules2: {
					content: {
						type: 'string',
						min: 10,
						required: true,
						message: '请输入您的在校经历',
						trigger: ['change']
					}
				},
				
			};
		},
		computed:{
			...mapState(['eduVals']),
		},
		watch:{
			'form.school'(){
				const { school, major, edu, eduStartTime, eduEndTime } = this.form
				const { content } = this.form2
				if(school.length!==0 && edu.length!==0  && eduStartTime.length!==0  && eduEndTime.length!==0  && content.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form.major'(){
				const { school, major, edu, eduStartTime, eduEndTime } = this.form
				const { content } = this.form2
				if(school.length!==0 && edu.length!==0  && eduStartTime.length!==0  && eduEndTime.length!==0  && content.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form.edu'(){
				const { school, major, edu, eduStartTime, eduEndTime } = this.form
				const { content } = this.form2
				if(school.length!==0 && edu.length!==0  && eduStartTime.length!==0  && eduEndTime.length!==0  && content.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form.eduStartTime'(){
				const { school, major, edu, eduStartTime, eduEndTime } = this.form
				const { content } = this.form2
				if(school.length!==0 && edu.length!==0  && eduStartTime.length!==0  && eduEndTime.length!==0  && content.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form.eduEndTime'(){
				const { school, major, edu, eduStartTime, eduEndTime } = this.form
				const { content } = this.form2
				if(school.length!==0 && edu.length!==0  && eduStartTime.length!==0  && eduEndTime.length!==0  && content.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form2.content'(){
				const { school, major, edu, eduStartTime, eduEndTime } = this.form
				const { content } = this.form2
				if(school.length!==0 && edu.length!==0  && eduStartTime.length!==0  && eduEndTime.length!==0  && content.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
		},
		onReady() {
			// 微信小程序需要用此写法
			this.$refs.datetimePicker.setFormatter(this.formatter)
			this.$refs.form.setRules(this.eduRules)
			this.$refs.form2.setRules(this.eduRules2)
		},
		onLoad(e) {
			
		},
		onShow() {
			this.init().setSchool()
		},
		methods: {
			...mapMutations(['SET_EDUVALS']),
			init(){
				return {
					setSchool:()=>{
						this.form.school = this.eduVals.school
					}
				}
			},
			// 打开菜单
			openSelect(){
				this.selectShow = true; 
				this.hideKeyboard()
			},
			// 关闭菜单
			dataClose(){
				this.selectShow = false
			},
			dataSelect(e){
				// console.log('e', e);
				const {value} = e
				this.form.edu = value[0].name
				this.selectShow = false
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
				this.dateValue = uni.$u.timeFormat(e.value, 'yyyy-mm')
				this.form[this.dateKey] = this.dateValue
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
						uni.$u.toast('请输入您的教育经历~')
					})
				}).catch(errors => {
					uni.$u.toast('form校验失败')
				})
			}
		},
		onHide() {
			this.SET_EDUVALS({school: ''})
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
