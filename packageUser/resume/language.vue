<template>
	<view class="page p-30">
		<view class="b-r-12 bg-fff box-shadow p-40">
			<view class="">
				<u--form labelPosition="top" :model="form" ref="form" :labelStyle="labelStyle">
					<u-form-item label="擅长语言" :required="true" prop="langage" ref="langage" borderBottom>
						<u--input :disabled="false" v-model="form.langage" border="none" placeholder="请输入, 如英语、粤语···"></u--input>
					</u-form-item>
					<u-form-item label="熟练度" :required="true" prop="skill" :borderBottom="true" ref="skill" @click="openSelect">
						<u--input :disabled="true" v-model="form.skill" border="none" placeholder="请选择熟练度" disabledColor="#fff"></u--input>
						<u-icon slot="right" name="arrow-right"></u-icon>
					</u-form-item>
					<u-form-item label="语言等级" :required="true" prop="grade" ref="grade" borderBottom>
						<u--input :disabled="false" v-model="form.grade" border="none" placeholder="请输入, 如四级、六级···"></u--input>
					</u-form-item>
				</u--form>
			</view>
		</view>
		<view class="p-t-b-30 f-s-22 c9 m-b-50">
			注：需填写打<text class="main-color">*</text>号的内容才能保存该模块
		</view>
		<u-button text="保存" shape="circle" :color="allCheck===true?'#F56718' : 'rgba(245,103,24,.5)'" size="large" @click="submit"></u-button>
		<u-picker :show="selectShow" :columns="shulian" keyName="name" confirmColor="#F56718" @cancel="dataClose" @confirm="dataSelect"></u-picker>
		
	</view>
</template>

<script>
	import { mapState } from 'vuex'
	export default {
		data() {
			return {
				allCheck: false, // 全部校验完毕
				form:{ // 擅长语言
					langage: '',
					skill: '',
					grade:''
				},
				labelStyle:{
					fontSize:'26rpx',
					color: '#999',
					width:'200rpx',
					marginLeft: '10rpx'
				},
				selectShow: false,
				shulian: [
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
					},
				}
			};
		},
		watch:{
			'form.langage'(){
				const { langage, skill, grade } = this.form
				if(langage.length!==0 && skill.length!==0 && grade.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form.skill'(){
				const { langage, skill, grade } = this.form
				if(langage.length!==0 && skill.length!==0 && grade.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form.grade'(){
				const { langage, skill, grade } = this.form
				if(langage.length!==0 && skill.length!==0 && grade.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			}
		},
		onReady() {
			this.$refs.form.setRules(this.goodRules)
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
				const { value } = e
				this.selectShow = false
				this.form.skill = value[0].name
			},
			hideKeyboard() {
				uni.hideKeyboard()
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
