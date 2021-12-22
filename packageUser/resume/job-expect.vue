<template>
	<view class="page p-30">
		<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
			<view class="p-b-30">
				<u--form labelPosition="top" :model="form" ref="form" :labelStyle="labelStyle">
					<u-form-item label="期望公司类型" :required="true" prop="type" ref="type" borderBottom>
						<u-radio-group v-model="typeValue" placement="row" @change="typeChange">
							<u-radio :customStyle="{margin: '30rpx 20rpx 10rpx 30rpx'}" activeColor="#F56718"
								v-for="(item, index) in typelist" :key="index" :label="item.name" :name="item.name">
							</u-radio>
						</u-radio-group>
					</u-form-item>
					<u-form-item label="期望工作时间" :required="true" prop="dateType" borderBottom ref="dateType">
						<u-radio-group v-model="timeValue" placement="row" @change="timeChange">
							<u-radio :customStyle="{margin: '30rpx 20rpx 10rpx 30rpx'}" activeColor="#F56718"
								v-for="(item, index) in timelist" :key="index" :label="item.name" :name="item.name">
							</u-radio>
						</u-radio-group>
					</u-form-item>
				</u--form>
			</view>
		</view>
		<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
			<view class="p-b-30">
				<u--form labelPosition="top" :model="form2" ref="form2" :labelStyle="labelStyle">
					<u-form-item label="可上班时间" :required="true" prop="date" :borderBottom="false" ref="date" @click="openSelect">
						<u--input :disabled="true" v-model="form2.date" border="none" placeholder="请选择可上班时间" disabledColor="#fff"></u--input>
						<u-icon slot="right" name="arrow-right"></u-icon>
					</u-form-item>
				</u--form>
				<u--form labelPosition="left" :model="form3" ref="form3" :labelStyle="labelStyle2">
					<u-form-item label="是否可支持全职上班" :required="true" prop="allTime" borderBottom ref="allTime">
						<view class="allTime-box">
							<u-radio-group v-model="tfValue" placement="row" @change="tfChange">
								<u-radio :customStyle="{margin: '0 30rpx'}" activeColor="#F56718"
									v-for="(item, index) in tflist" :key="index" :label="item.name" :name="item.name">
								</u-radio>
							</u-radio-group>
						</view>
						
					</u-form-item>
				</u--form>
			</view>
		</view>
	
		<view class="p-t-b-30 f-s-22 c9 m-b-50">
			注：需填写打<text class="main-color">*</text>号的内容才能保存该模块
		</view>
		<u-button text="保存" shape="circle" :color="allCheck===true?'#F56718' : 'rgba(245,103,24,.5)'" size="large" @click="submit"></u-button>
		<u-picker :show="selectShow" :columns="times" keyName="name" confirmColor="#F56718" @cancel="dataClose" @confirm="dataSelect"></u-picker>
	</view>
</template>

<script>
	import { mapState } from 'vuex'
	export default {
		data() {
			return {
				allCheck: false, // 全部校验完毕
				form:{ // 工作期望
					type: '短期兼职',
					dateType: '工作日',
					date: '',
					allTime:''
				},
				form2:{
					date: ''
				},
				form3:{
					allTime: '是'
				},
				labelStyle:{
					fontSize:'26rpx',
					color: '#999',
					width:'200rpx',
					marginLeft: '10rpx'
				},
				labelStyle2:{
					fontSize:'26rpx',
					color: '#999',
					width:'300rpx',
					marginLeft: '10rpx'
				},
				selectKey:'school', // 选择菜单
				selectShow: false,
				actions:[],
				schools: [
					{
						name: '广西民族大学',
					},
					{
						name: '广西大学',
					},
					{
						name: '桂林理工大学',
					},
				],
				times: [
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
					}]
				],
				// 期望公司类型
				typelist: [{ // 单选
						name: '短期兼职',
						disabled: false
					},
					{
						name: '长期兼职',
						disabled: false
					},
					{
						name: '实习兼职',
						disabled: false
					},
					{
						name: '周末兼职',
						disabled: false
					},
					{
						name: '不限',
						disabled: false
					}
				],
				typeValue: '短期兼职',
				// 期望工作时间
				timelist: [{ // 单选
						name: '工作日',
						disabled: false
					},
					{
						name: '周末',
						disabled: false
					},
					{
						name: '节假日',
						disabled: false
					},
					{
						name: '寒暑假',
						disabled: false
					},
					{
						name: '不限',
						disabled: false
					}
				],
				timeValue: '工作日',
				// 是否可支持全职上班
				tflist: [{
						name: '是',
						disabled: false
					},
					{
						name: '否',
						disabled: false
					}
				],
				tfValue: '是',
				// 校验规则
				qwRules: {
					type: {
						type: 'string',
						required: true,
						message: '请选择期望公司类型',
						trigger: ['blur', 'change']
					},
					dateType: {
						type: 'string',
						required: true,
						message: '请选择期望工作时间',
						trigger: ['blur', 'change']
					}
				},
				qwRules2: {
					type: {
						type: 'string',
						required: true,
						message: '请选择期望公司类型',
						trigger: ['blur', 'change']
					},
					dateType: {
						type: 'string',
						required: true,
						message: '请选择期望工作时间',
						trigger: ['blur', 'change']
					},
				},
				qwRules3: {
					date: {
						type: 'string',
						required: true,
						message: '请选择可上班时间',
						trigger: ['blur', 'change']
					},
				},
				
			};
		},
		watch:{
			'form.type'(){
				const { type, dateType } = this.form
				const { date } = this.form2
				const { allTime } = this.form3
				if(type.length!==0 && dateType.length!==0 && date.length!==0 && allTime.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form.dateType'(){
				const { type, dateType } = this.form
				const { date } = this.form2
				const { allTime } = this.form3
				if(type.length!==0 && dateType.length!==0 && date.length!==0 && allTime.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form2.date'(){
				const { type, dateType } = this.form
				const { date } = this.form2
				const { allTime } = this.form3
				if(type.length!==0 && dateType.length!==0 && date.length!==0 && allTime.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form3.allTime'(){
				const { type, dateType } = this.form
				const { date } = this.form2
				const { allTime } = this.form3
				if(type.length!==0 && dateType.length!==0 && date.length!==0 && allTime.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
		},
		onReady() {
			// 微信小程序需要用此写法
			this.$refs.form.setRules(this.qwRules)
			this.$refs.form2.setRules(this.qwRules2)
			this.$refs.form3.setRules(this.qwRules3)
		},
		methods: {
			init(){
				return {
					
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
				this.form2.date = value[0].name
				this.selectShow = false
			},
			// 选择 期望公司类型
			typeChange(n) {
				this.form.type = n
			},
			// 选择 期望工作时间
			timeChange(n) {
				this.form.dateType = n
			},
			// 选择 是否可支持全职上班
			tfChange(n) {
				this.form3.allTime = n
			},
			hideKeyboard() {
				uni.hideKeyboard()
			},
			submit(){
				// console.log('submit--', this.form);
				this.$refs.form.validate().then(res => {
					uni.$u.toast('校验通过')
					this.$refs.form2.validate().then(res => {
						uni.$u.toast('form2校验通过')
						this.$refs.form3.validate().then(res => {
							uni.$u.toast('form3校验通过')
						}).catch(errors => {
							uni.$u.toast('form3校验失败')
						})
					}).catch(errors => {
						uni.$u.toast('form2校验失败')
					})
				}).catch(errors => {
					uni.$u.toast('校验失败')
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
