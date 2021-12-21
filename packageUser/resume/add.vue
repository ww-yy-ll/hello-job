<template>
	<view class="page p-30">
		<!-- 教育经历 -->
		<view class="" v-if="title==='教育经历'">
			<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
				<view class="p-b-30">
					<u--form labelPosition="top" :model="form" ref="form" :labelStyle="labelStyle">
						<u-form-item label="学校" :required="true" prop="shcool" ref="shcool" borderBottom @click="openSelect('shcool')">
							<u--input :disabled="true" v-model="form.shcool" border="none" placeholder="请输入您的学校" disabledColor="#fff"></u--input>
							<u-icon slot="right" name="arrow-right"></u-icon>
						</u-form-item>
						<u-form-item label="主修专业" :required="false" prop="major" ref="major" borderBottom>
							<u--input :disabled="false" v-model="form.major" border="none" placeholder="请输入您的主修专业"></u--input>
							<!-- <u-icon slot="right" name="arrow-right"></u-icon> -->
						</u-form-item>
						<u-form-item label="学历" :required="true" prop="edu" ref="edu" borderBottom @click="openSelect('edu')">
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
					<u--form labelPosition="top" :model="form" ref="form" :labelStyle="labelStyle">
						<u-form-item label="教育经历" :required="true" prop="content" :borderBottom="false" ref="content">
							<u--textarea :placeholder="placeholder" v-model="form.content" count height="500rpx" maxlength="800"></u--textarea>
						</u-form-item>
					</u--form>
				</view>
			</view>
		</view>
		
		<!-- 工作经历 -->
		<view class="" v-if="title==='工作经历'">
			<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
				<view class="p-b-30">
					<u--form labelPosition="top" :model="form2" ref="form2" :labelStyle="labelStyle">
						<u-form-item label="公司名" :required="true" prop="company" ref="company" borderBottom>
							<u--input :disabled="false" v-model="form2.company" border="none" placeholder="请输入公司名"></u--input>
						</u-form-item>
						<u-form-item label="开始时间" :required="true" prop="comPStartTime" borderBottom @click="openDate('comPStartTime')" ref="comPStartTime">
							<u--input v-model="form2.comPStartTime" disabled disabledColor="#ffffff" placeholder="请选择开始时间" border="none"></u--input>
							<u-icon slot="right" name="arrow-right"></u-icon>
						</u-form-item>
						<u-form-item label="结束时间" :required="true" prop="compEndTime" borderBottom @click="openDate('compEndTime')" ref="compEndTime">
							<u--input v-model="form2.compEndTime" disabled disabledColor="#ffffff" placeholder="请选择结束时间" border="none"></u--input>
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
		</view>
		
		<!-- 工作期望 -->
		<view class="" v-if="title==='工作期望'">
			<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
				<view class="p-b-30">
					<u--form labelPosition="top" :model="form3" ref="form3" :labelStyle="labelStyle">
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
					<u--form labelPosition="top" :model="form3" ref="form3" :labelStyle="labelStyle">
						<u-form-item label="可上班时间" :required="true" prop="date" :borderBottom="false" ref="date" @click="openSelect('date')">
							<u--input :disabled="true" v-model="form3.date" border="none" placeholder="请选择可上班时间" disabledColor="#fff"></u--input>
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
		</view>
		
		<!-- 技能/证书 -->
		<view class="" v-if="title==='技能/证书'">
			<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
				<view class="">
					<u--form labelPosition="top" :model="form4" ref="form4" :labelStyle="labelStyle">
						<u-form-item label="技能名/奖章名" :required="true" prop="name" ref="name" borderBottom>
							<u--input :disabled="false" v-model="form4.name" border="none" placeholder="请输入,如PS、计算机证书···"></u--input>
						</u-form-item>
						<u-form-item label="相关介绍" :required="true" prop="content" :borderBottom="false" ref="content">
							<u--textarea placeholder="请描述一下该技能/证书/奖章" v-model="form4.content" count height="250rpx" maxlength="200"></u--textarea>
						</u-form-item>
					</u--form>
				</view>
			</view>
			<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
				<view class="p-b-30">
					<u--form labelPosition="top" :model="form3" ref="form3" :labelStyle="labelStyle3">
						<u-form-item label="证书/奖章照片上传(可上传jpg、png,小于5M)" :required="false" prop="zsImage" :borderBottom="false" ref="zsImage">
							<u-upload
								uploadIcon="plus-circle"
								:fileList="fileList1"
								@afterRead="afterRead"
								@delete="deletePic"
								name="1"
								multiple
								:maxCount="10"
						></u-upload>
						</u-form-item>
					</u--form>
				</view>
			</view>
		</view>
		
		<!-- 擅长语言 -->
		<view class="" v-if="title==='擅长语言'">
			<view class="b-r-12 bg-fff box-shadow p-40">
				<view class="">
					<u--form labelPosition="top" :model="form5" ref="form5" :labelStyle="labelStyle">
						<u-form-item label="擅长语言" :required="true" prop="langage" ref="langage" borderBottom>
							<u--input :disabled="false" v-model="form5.langage" border="none" placeholder="请输入, 如英语、粤语···"></u--input>
						</u-form-item>
						<u-form-item label="熟练度" :required="true" prop="skill" :borderBottom="true" ref="skill" @click="openSelect('skill')">
							<u--input :disabled="true" v-model="form5.skill" border="none" placeholder="请选择熟练度" disabledColor="#fff"></u--input>
							<u-icon slot="right" name="arrow-right"></u-icon>
						</u-form-item>
						<u-form-item label="语言等级" :required="true" prop="grade" ref="grade" borderBottom>
							<u--input :disabled="false" v-model="form5.grade" border="none" placeholder="请输入, 如四级、六级···"></u--input>
						</u-form-item>
					</u--form>
				</view>
			</view>
		</view>
		
		<!-- 礼仪模特卡 -->
		<view class="" v-if="title==='礼仪模特卡'">
			<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
				<view class="">
					<u--form labelPosition="top" :model="form6" ref="form6" :labelStyle="labelStyle">
						<u-form-item label="体重" :required="true" prop="name" ref="name" borderBottom>
							<u--input :disabled="false" v-model="form6.weight" border="none" placeholder="请输入您的体重"></u--input>
							<text slot="right">kg</text>
						</u-form-item>
						<u-form-item label="胸围" :required="false" prop="xiongwei" :borderBottom="true" ref="xiongwei">
							<u--input :disabled="false" v-model="form6.xiongwei" border="none" placeholder="请输入您的胸围"></u--input>							<text slot="right">cm</text>
						</u-form-item>
						<u-form-item label="鞋码" :required="false" prop="xiema" :borderBottom="false" ref="xiema">
							<u--input :disabled="false" v-model="form6.xiema" border="none" placeholder="请输入您的鞋码"></u--input>							<text slot="right">码</text>
						</u-form-item>
					</u--form>
				</view>
			</view>
			<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
				<view class="p-b-30">
					<u--form labelPosition="top" :model="form3" ref="form3" :labelStyle="labelStyle3">
						<u-form-item label="形象照片上传(可上传jpg、png,小于5M)" :required="false" prop="zsImage" :borderBottom="false" ref="zsImage">
							<u-upload
								uploadIcon="plus-circle"
								:fileList="fileList2"
								@afterRead="afterRead"
								@delete="deletePic"
								name="2"
								multiple
								:maxCount="10"
						></u-upload>
						</u-form-item>
					</u--form>
				</view>
			</view>
		</view>
		
		<!-- 相关附件 -->
		<view class="" v-if="title==='相关附件'">
			<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
				<view class="">
					<u--form labelPosition="top" :model="form7" ref="form7" :labelStyle="labelStyle">
						<u-form-item label="文件名" :required="true" prop="name" ref="name" borderBottom>
							<u--input :disabled="false" v-model="form7.name" border="none" placeholder="请输入,如我的简历···"></u--input>
						</u-form-item>
						<u-form-item label="备注" :required="true" prop="content" :borderBottom="false" ref="content">
							<u--textarea placeholder="可填写该附件的说明,让招聘者更好地了解" v-model="form7.content" count height="250rpx" maxlength="200"></u--textarea>
						</u-form-item>
					</u--form>
				</view>
			</view>
			<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
				<view class="p-b-30">
					<u--form labelPosition="top" :model="form7" ref="form7" :labelStyle="labelStyle3">
						<u-form-item label="附件上传(可上传jpg、png,小于5M)" :required="false" prop="zsImage" :borderBottom="false" ref="zsImage">
							<u-upload
								uploadIcon="plus-circle"
								:fileList="fileList3"
								@afterRead="afterRead"
								@delete="deletePic"
								name="3"
								multiple
								:maxCount="10"
						></u-upload>
						</u-form-item>
					</u--form>
				</view>
			</view>
		</view>
		
		<view class="p-t-b-30 f-s-22 c9 m-b-50">
			注：需填写打<text class="main-color">*</text>号的内容才能保存该模块
		</view>
		<u-button text="保存" shape="circle" color="#F56718" size="large" @click="submit"></u-button>
		
		
		<u-calendar :show="showCalendar" mode="single" @confirm="calendarConfirm" @close="calendarClose" confirmDisabledText="请选择离店日期"></u-calendar>
		<u-action-sheet :show="selectShow" :actions="xueli" title="请选择" @close="dataClose" @select="dataSelect"></u-action-sheet>
		<u-datetime-picker ref="datetimePicker" :show="dateShow" mode="date" v-model="dateValue" :formatter="formatter" closeOnClickOverlay
			@confirm="dateConfirm" @cancel="dateClose" @close="dateClose"></u-datetime-picker>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '', // 区分页面
				form:{ // 教育经历
					school:'',
					major:'',
					edu:'',
					eduStartTime:'',
					eduEndTime:'',
					content:''
				},
				form2:{ // 工作经历
					company: '',
					compStartTime: '',
					compEndTime: '',
					content:''
				},
				form3:{ // 工作期望
					type: '',
					dateType: '',
					date: '',
					allTime:''
				},
				form4:{ // 技能/证书
					name: '',
					content: '',
					zsImage:''
				},
				form5:{ // 擅长语言
					langage: '',
					skill: '',
					grade:''
				},
				form6:{ // 礼仪模特卡
					weight: '',
					xiongwei: '',
					xiema:'',
					zsImage:''
				},
				form7:{ // 相关附件
					name: '',
					content: '',
					zsImage:''
				},
				fileList1: [],
				fileList2: [],
				fileList3: [],
				placeholder:`请输入你的在校经历如示例:1.大学期间,连续四年获得年纪奖学金一等奖;2.大三下学期,在双单位xx部门实习三个月;获得最佳优秀实习生称号·`,
				compPlaceholder: `请输入您的工作经验或实习经验`,
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
				labelStyle3:{
					fontSize:'26rpx',
					color: '#999',
					width:'600rpx',
					margin: '10rpx 0'
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
				xueli: [{
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
					},
				],
				// 时间选择器
				dateKey:'', 
				dateValue: Number(new Date()),
				dateShow: false,
				// 日历
				showCalendar: false,
				
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
					},
					content: {
						type: 'string',
						min: 10,
						required: true,
						message: '请输入您的工作具体内容',
						trigger: ['change']
					}
				},
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
					},
					date: {
						type: 'string',
						required: true,
						message: '请选择可上班时间',
						trigger: ['blur', 'change']
					},
					allTime: {
						type: 'string',
						required: true,
						message: '请选择是否可支持全职上班',
						trigger: ['change']
					}
				},
				jnRules: {
					name: {
						type: 'string',
						required: true,
						message: '请输入技能名/奖章名',
						trigger: ['blur', 'change']
					},
					content: {
						type: 'string',
						required: true,
						message: '请输入相关介绍',
						trigger: ['blur', 'change']
					}
				},
				goodRules: {
					langage: {
						type: 'string',
						required: true,
						message: '请输入擅长语音',
						trigger: ['blur', 'change']
					},
					skill: {
						type: 'string',
						required: true,
						message: '请选择熟练度',
						trigger: ['blur', 'change']
					},
					grade: {
						type: 'string',
						required: true,
						message: '请输入语言等级',
						trigger: ['blur', 'change']
					}
				},
				liyiRules: {
					weight: {
						type: 'string',
						required: true,
						message: '请输入您的体重',
						trigger: ['blur', 'change']
					},
					zsImage: {
						type: 'string',
						required: true,
						message: '请上传形象照片',
						trigger: ['blur', 'change']
					},
				},
				fjRules: {
					name: {
						type: 'string',
						required: true,
						message: '请输入文件名',
						trigger: ['blur', 'change']
					},
					content: {
						type: 'string',
						required: true,
						message: '请输入备注',
						trigger: ['blur', 'change']
					},
					zsImage: {
						type: 'string',
						required: true,
						message: '请上传附件',
						trigger: ['blur', 'change']
					},
				},
				
			};
		},
		onReady() {
			// 微信小程序需要用此写法
			this.$refs.datetimePicker.setFormatter(this.formatter)
			this.init().setRules()
		},
		onLoad(e) {
			const {text} = e
			if(text) {
				this.init().setTitle(text)
			}
		},
		methods: {
			init(){
				return {
					// 标题栏文字
					setTitle:(text)=>{
						this.title = text ? text: '教育经历'
						uni.setNavigationBarTitle({
							title: text
						})
					},
					setRules:()=>{
						// 如果需要兼容微信小程序，并且校验规则中含有方法等，只能通过setRules方法设置规则
						const { title } = this
						switch(title) {
							case '教育经历':
								this.$refs.form.setRules(this.eduRules)
								break
							case '工作经历':
								this.$refs.form2.setRules(this.compRules)
								break
							case '工作期望':
								this.$refs.form3.setRules(this.qwRules)
								break
							case '技能/证书':
								this.$refs.form4.setRules(this.jnRules)
								break
							case '擅长语言':
								this.$refs.form5.setRules(this.goodRules)
								break
							case '礼仪模特卡':
								this.$refs.form6.setRules(this.liyiRules)
								break
							case '相关附件':
								this.$refs.form7.setRules(this.fjRules)
								break
							default: 
								break
						}
					},
				}
			},
			// 打开菜单
			openSelect(key){
				console.log('key', key);
				this.selectKey = key
				this.selectShow = true; 
				this.hideKeyboard()
			},
			// 关闭菜单
			dataClose(){
				this.selectShow = false
				this.actions = []
			},
			dataSelect(e){
				console.log('e', e);
				const { name } = e
				const { title } = this
				switch(title) {
					case '教育经历':
						this.form[this.selectKey] = name
						break
					case '工作经历':
						
						break
					case '工作期望':
						this.form3[this.selectKey] = name
						break
					case '擅长语言':
						this.form5[this.selectKey] = name
						break
					default: 
						break
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
				const { title } = this
				switch(title) {
					case '教育经历':
						this.form[this.dateKey] = this.dateValue
						break
					case '工作经历':
						this.form2[this.dateKey] = this.dateValue
						break
					default: 
						break
				}
				// this.form[this.dateKey] = this.dateValue
				// console.log('e', e, this.dateValue, this.form);
				// this.userInfo.birthday = uni.$u.timeFormat(e.value, 'yyyy-mm-dd')
				// this.$refs.form.validateField('birthday')
			},
			// 选择 期望公司类型
			typeChange(n) {
				this.form3.type = n
			},
			// 选择 期望工作时间
			timeChange(n) {
				this.form3.dateType = n
			},
			// 选择 是否可支持全职上班
			tfChange(n) {
				this.form3.allTime = n
			},
			// 上传图片
			// 删除图片
			deletePic(event) {
				this[`fileList${event.name}`].splice(event.index, 1)
			},
			// 新增图片
			async afterRead(event) {
				// 当设置 mutiple 为 true 时, file 为数组格式，否则为对象格式
				let lists = [].concat(event.file)
				let fileListLen = this[`fileList${event.name}`].length
				lists.map((item) => {
					this[`fileList${event.name}`].push({
						...item,
						status: 'uploading',
						message: '上传中'
					})
				})
				for (let i = 0; i < lists.length; i++) {
					const result = await this.uploadFilePromise(lists[i].thumb)
					let item = this[`fileList${event.name}`][fileListLen]
					this[`fileList${event.name}`].splice(fileListLen, 1, Object.assign(item, {
						status: 'success',
						message: '',
						url: result
					}))
					fileListLen++
				}
			},
			uploadFilePromise(url) {
				return new Promise((resolve, reject) => {
					let a = uni.uploadFile({
						url: '', // 仅为示例，非真实的接口地址
						filePath: url,
						name: 'file',
						formData: {
							user: 'test'
						},
						success: (res) => {
							setTimeout(() => {
								resolve(res.data.data)
							}, 1000)
						}
					});
				})
			},
		
			hideKeyboard() {
				uni.hideKeyboard()
			},
			submit(){
				console.log('submit--', this.form);
				const { title } = this
				switch(title) {
					case '教育经历':
						this.$refs.form.validate().then(res => {
							uni.$u.toast('校验通过')
						}).catch(errors => {
							uni.$u.toast('校验失败')
						})
						break
					case '工作经历':
						this.$refs.form2.validate().then(res => {
							uni.$u.toast('校验通过')
						}).catch(errors => {
							uni.$u.toast('校验失败')
						})
						break
					case '工作期望':
						this.$refs.form3.validate().then(res => {
							uni.$u.toast('校验通过')
						}).catch(errors => {
							uni.$u.toast('校验失败')
						})
						break
					case '技能/证书':
						this.$refs.form4.validate().then(res => {
							uni.$u.toast('校验通过')
						}).catch(errors => {
							uni.$u.toast('校验失败')
						})
						break
					case '擅长语言':
						this.$refs.form5.validate().then(res => {
							uni.$u.toast('校验通过')
						}).catch(errors => {
							uni.$u.toast('校验失败')
						})
						break
					case '礼仪模特卡':
						this.$refs.form6.validate().then(res => {
							uni.$u.toast('校验通过')
						}).catch(errors => {
							uni.$u.toast('校验失败')
						})
						break
					case '相关附件':
						this.$refs.form5.validate().then(res => {
							uni.$u.toast('校验通过')
						}).catch(errors => {
							uni.$u.toast('校验失败')
						})
						break
					default: 
						break
				}
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
