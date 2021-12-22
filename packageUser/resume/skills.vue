<template>
	<view class="page p-30">
		<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
			<view class="">
				<u--form labelPosition="top" :model="form" ref="form" :labelStyle="labelStyle">
					<u-form-item label="技能名/奖章名" :required="true" prop="name" ref="name" borderBottom>
						<u--input :disabled="false" v-model="form.name" border="none" placeholder="请输入,如PS、计算机证书···"></u--input>
					</u-form-item>
					<u-form-item label="相关介绍" :required="true" prop="content" :borderBottom="false" ref="content">
						<u--textarea placeholder="请描述一下该技能/证书/奖章" v-model="form.content" count height="250rpx" maxlength="200"></u--textarea>
					</u-form-item>
				</u--form>
			</view>
		</view>
		<view class="b-r-12 bg-fff box-shadow p-40 m-b-30">
			<view class="p-b-30">
				<u--form labelPosition="top" :model="form" ref="form" :labelStyle="labelStyle3">
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
				allCheck: false,
				form:{ // 技能/证书
					name: '',
					content: '',
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
				
			};
		},
		watch:{
			'form.name'(){
				const { name, content } = this.form
				if(name.length!==0 && content.length!==0) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			},
			'form.content'(){
				const { name, content } = this.form
				if(name.length!==0 && content.length!==0) {
					this.allCheck = true
				} else {
					this.allCheck = false
				}
			}
		},
		onReady() {
			this.$refs.form.setRules(this.jnRules)
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
				this.$refs.form4.validate().then(res => {
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
