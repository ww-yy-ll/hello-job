<template>
	<view class="page p-30">
		<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
			<view class="">
				<u--form labelPosition="top" :model="form" ref="form" :labelStyle="labelStyle">
					<u-form-item label="体重" :required="true" prop="name" ref="name" borderBottom>
						<u--input :disabled="false" v-model="form.weight" border="none" placeholder="请输入您的体重"></u--input>
						<text slot="right">kg</text>
					</u-form-item>
					<u-form-item label="胸围" :required="false" prop="xiongwei" :borderBottom="true" ref="xiongwei">
						<u--input :disabled="false" v-model="form.xiongwei" border="none" placeholder="请输入您的胸围"></u--input>						<text slot="right">cm</text>
					</u-form-item>
					<u-form-item label="鞋码" :required="false" prop="xiema" :borderBottom="false" ref="xiema">
						<u--input :disabled="false" v-model="form.xiema" border="none" placeholder="请输入您的鞋码"></u--input>						<text slot="right">码</text>
					</u-form-item>
				</u--form>
			</view>
		</view>
		<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
			<view class="p-b-30">
				<u--form labelPosition="top" :model="form" ref="form" :labelStyle="labelStyle3">
					<u-form-item label="形象照片上传(可上传jpg、png,小于5M)" :required="true" prop="zsImage" :borderBottom="false" ref="zsImage">
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
		
		<view class="p-t-b-30 f-s-22 c9 m-b-50">
			注：需填写打<text class="main-color">*</text>号的内容才能保存该模块
		</view>
		<u-button text="保存" shape="circle" :color="allCheck===true?'#F56718' : 'rgba(245,103,24,.5)'" size="large" @click="submit"></u-button>
		
	</view>
</template>

<script>
	import { mapState } from 'vuex'
	export default {
		data() {
			return {
				allCheck: false, // 全部校验完毕
				form:{ // 礼仪模特卡
					weight: '',
					xiongwei: '',
					xiema:'',
					zsImage:''
				},
				fileList1: [],
				labelStyle:{
					fontSize:'26rpx',
					color: '#999',
					width:'200rpx',
					marginLeft: '10rpx'
				},
				labelStyle3:{
					fontSize:'26rpx',
					color: '#999',
					width:'600rpx',
					margin: '10rpx 0'
				},
				rules: {
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
				
			};
		},
		watch:{
			'form.weight'(){
				const { weight, zsImage } = this.form
				if(weight.length!==0 && zsImage.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form.zsImage'(){
				const { weight, zsImage } = this.form
				if(weight.length!==0 && zsImage.length!==0 ) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			}
		},
		onReady() {
			this.$refs.form.setRules(this.rules)
		},
		methods: {
			init(){
				return {
					
				}
			},
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
