<template>
	<view class="page index-page">
		<!-- nav -->
		<CommonNav />
		<!-- 顶部tab -->
		<u-sticky v-if="showTop" :offset-top="sysNav.top + sysNav.height">
			<view class="bg-fff border-box top-box">
				<view class="top-tab-block m-b-20 p-l-r-30">
					<u-tabs 
						:list="topTabList" :current="tabIndex" lineWidth="20" lineHeight="4" lineColor="#F56718"
						:activeStyle="{color: '#F56718',fontWeight: 'bold', fontSize: '32rpx'}"
						:inactiveStyle="{color: '#303133', fontSize: '26rpx'}"
						itemStyle="height: 80rpx;" @change="(item)=>handler().click(item)">
					</u-tabs>
				</view>
			</view>
		</u-sticky>
		<!-- 推荐 -->
		<view class="" v-if="tabIndex===0">
			<RecomendItem />
		</view>
		<!-- 附近 -->
		<view class="" v-if="tabIndex===1">
			<!-- 地点 -->
			<view v-if="showTop" class="place-box row-between m-t-20 m-b-40 p-l-r-30">
				<view class="row-center place-box-left">
					<u-icon name="reload" size="15" color=""></u-icon>
					<text class="f-s-22 job-inline m-l-15">广西壮族自治区南宁市兴宁区金仑路32号广西壮族自治区南宁市兴宁区金仑路32号</text>
				</view>
				<view class="row-center place-box-right" @click="()=>handler().openLimit()">
					<text class="f-s-22 m-r-15 job-inline">{{limit}}</text>
					<u-icon name="arrow-down" size="12" color=""></u-icon>
				</view>
			</view>
			<u-sticky :offset-top="sysNav.top + sysNav.height">
				<view class="row center f-s-22 p-l-r-30 bg-fff near-select-block">
					<view class="row-center select-box m-r-20" v-for="(item,index) in tjTab" :key="index" 
						@click="()=>{handler().openSelect(index)}">
						<text class="m-r-10" :class="{'main-color': current === index }">{{item.label}}</text>
						<u-icon v-if="item.isDropdown===true" size="9"
							:name="current === index? 'arrow-up' : 'arrow-down'" 
							:color="current === index? '#F56718' : ''"></u-icon>
					</view>
				</view>
				<!-- 筛选下拉框 -->
				<DropdownMenu ref="dropdown" :top="10" @mask="()=>handler().closeMask()">
					<!-- 类型 -->
					<view v-if="current===1">
						<DropdownItem :list.sync="typeList" menuType="type" @submit="(e)=>handler().submitType(e)" />
					</view>
					<!-- 区域 -->
					<!-- <view v-if="current===1">
						<DropdownItem menuType="area" :list="areaList" @submitArea="(val)=>handler().submitArea(val)" />
					</view> -->
					<!-- 排序 -->
					<view v-if="current===2">
						<DropdownItem menuType="sort" :list="sortList" :sortIdx.sync="sortIdx" @selectSort="(val)=>handler().selectSort(val)" />
					</view>
				</DropdownMenu>
				
			</u-sticky>
			
			<JobList :list="jobList"></JobList>
		</view>
		<u-back-top :scroll-top="scrollTop" icon="arrow-up" :iconStyle="iStyle" :customStyle="cStyle"></u-back-top>
		
		<u-picker :show="showLimit" :columns="limits" keyName="name" confirmColor="#F56718" @cancel="()=>handler().limitCancel()" @confirm="(e)=>handler().limitComfirm(e)"></u-picker>
		
	</view>
</template>

<script>
	import CommonNav from '@/components/layout/common-nav.vue'
	import JobList from '@/components/list/job-list.vue'
	import RecomendItem from './components/recomend-item.vue'
	import DropdownMenu from '@/pages/partTime/components/dropdown-menu.vue'
	import DropdownItem from '@/pages/partTime/components/dropdown-item.vue'
	import {mapState} from 'vuex'
	import {getUserAddress} from '@/utils/util'
	export default {
		components: { CommonNav, JobList, RecomendItem, DropdownMenu, DropdownItem },
		data() {
			return {
				scrollTop: 0, 
				// mode: 'circle',
				iStyle: {
					fontSize: '32rpx',
					color: '#ffffff',
				},
				cStyle: {
					backgroundColor: '#F56718',
				},
				tabIndex: 0, // 当前选中tab
				topTabList:[
					{
						name: '推荐'
					}, {
						name: '附近'
					}
				],
				showTop: true, // 推荐tab是否吸顶
				current: -1, // 推荐tab选中
				sortIdx: 0,
				typeList:[ // 类型下拉框数据
					{
						title: '热门推荐',
						children:[
							{label: '主播', check: false},
							{label: '客服', check: false},
							{label: '手机任务', check: false},
							{label: '厂工', check: false},
							{label: '派单', check: false},
						]
					},{
						title: '市场推广',
						children:[
							{label: '网点', check: false},
							{label: '短视频', check: false},
							{label: '电商', check: false},
							{label: '地推', check: false},
							{label: '广告监测', check: false},
							{label: '代理', check: false},
							{label: '促销', check: false},
							{label: '销售导购', check: false},
						]
					},{
						title: '服务行业',
						children:[
							{label: '店员', check: false},
							{label: '厨师', check: false},
							{label: '美容美甲', check: false},
							{label: '送餐员', check: false},
							{label: '服务员', check: false},
						]
					},{
						title: '教育行业',
						children:[
							{label: '助教', check: false},
							{label: '机构教师', check: false},
							{label: '才艺教师', check: false},
							{label: '课程顾问', check: false},
							{label: '线上教师', check: false},
							{label: '家教', check: false},
						]
					},{
						title: '会展演出',
						children:[
							{label: '助教', check: false},
							{label: '机构教师', check: false},
							{label: '才艺教师', check: false},
							{label: '课程顾问', check: false},
							{label: '线上教师', check: false},
							{label: '家教', check: false},
						]
					}
				],
				sortList:[ // 排序下拉框数据
					{label: '综合排序'},
					{label: '最新发布'},
					{label: '离我最近'},
				],
				tjTab:[ // 推荐tab列表
					{
						label:'兼职猫自营',
						isDropdown: false,
						open: false
					},{
						label:'类型',
						isDropdown: true,
						open: false
					},{
						label:'排序',
						isDropdown: true,
						open: false
					},
				],
				jobList:[
					{
						title:'在家线上学习PS做兼职',
						level: 0,
						money: '200元/天',
						labels:[
							{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},
						],
						company: '上海为课网络科技有限公司',
						good: 0,
						place: '江南'
					},{
						title:'在家线上学习PS做兼职在家线上学习PS做兼职在家线上学习PS做兼职',
						level: 1,
						money: '200元/天',
						labels:[
							{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},
						],
						company: '上海为课网络科技有限公司上海为课网络科技有限公司',
						good: 1,
						place: '江南江南江南江南江南江南江南江南江南江南'
					},{
						title:'在家线上学习PS做兼职',
						level: 0,
						money: '200元/天',
						labels:[
							{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},
						],
						company: '上海为课网络科技有限公司',
						good: 0,
						place: '江南'
					},{
						title:'在家线上学习PS做兼职在家线上学习PS做兼职在家线上学习PS做兼职',
						level: 1,
						money: '200元/天',
						labels:[
							{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},
						],
						company: '上海为课网络科技有限公司上海为课网络科技有限公司',
						good: 1,
						place: '江南江南江南江南江南江南江南江南江南江南'
					},{
						title:'在家线上学习PS做兼职',
						level: 0,
						money: '200元/天',
						labels:[
							{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},
						],
						company: '上海为课网络科技有限公司',
						good: 0,
						place: '江南'
					},{
						title:'在家线上学习PS做兼职在家线上学习PS做兼职在家线上学习PS做兼职',
						level: 1,
						money: '200元/天',
						labels:[
							{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},
						],
						company: '上海为课网络科技有限公司上海为课网络科技有限公司',
						good: 1,
						place: '江南江南江南江南江南江南江南江南江南江南'
					},{
						title:'在家线上学习PS做兼职',
						level: 0,
						money: '200元/天',
						labels:[
							{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},
						],
						company: '上海为课网络科技有限公司',
						good: 0,
						place: '江南'
					},{
						title:'在家线上学习PS做兼职在家线上学习PS做兼职在家线上学习PS做兼职',
						level: 1,
						money: '200元/天',
						labels:[
							{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},
						],
						company: '上海为课网络科技有限公司上海为课网络科技有限公司',
						good: 1,
						place: '江南江南江南江南江南江南江南江南江南江南'
					},{
						title:'在家线上学习PS做兼职',
						level: 0,
						money: '200元/天',
						labels:[
							{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},
						],
						company: '上海为课网络科技有限公司',
						good: 0,
						place: '江南'
					},{
						title:'在家线上学习PS做兼职在家线上学习PS做兼职在家线上学习PS做兼职',
						level: 1,
						money: '200元/天',
						labels:[
							{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},{
								text: '企业直招',
								blue: 1
							},{
								text: '有提成',
								blue: 0
							},
						],
						company: '上海为课网络科技有限公司上海为课网络科技有限公司',
						good: 1,
						place: '江南江南江南江南江南江南江南江南江南江南'
					},
				],
				showLimit: false, // 地点区域范围
				limit: '1公里内',
				limits: [
					[{
						name: '1公里内',
					},
					{
						name: '2公里内',
					},
					{
						name: '3公里内',
					},
					{
						name: '4公里内',
					},
					{
						name: '5公里内',
					}],
				],
			}
		},
		computed:{
			...mapState(['sysNav'])
		},
		onLoad() {
			
		},
		onPageScroll(e) {
			// console.log(e);
			const { scrollTop } = e
			this.scrollTop = scrollTop;
			if(this.current > -1) {
				this.showTop = scrollTop < 10 ? true : 10 < scrollTop < 100 ? false: true
			} else {
				this.showTop = scrollTop < 5 ? true : false
			}
			/* if(scrollTop < 100 && this.current > -1) {
				this.showTop = false
			}
			if(scrollTop < 10 && this.current > -1) {
				this.showTop = true
			}
			if( this.current === -1 && scrollTop < 5) { // tab吸顶并选择后
				this.showTop = true
			} */
			
		},
		onPullDownRefresh() {
			this.handler().stopPullDownRefresh()
		},
		onReachBottom() {
			
		},
		methods: {
			isGetLocation(a = "scope.userLocation") { // 3. 检查当前是否已经授权访问scope属性，参考下截图
				let _this = this;
				uni.getSetting({
					success(res) {
						if (!res.authSetting[a]) { //3.1 每次进入程序判断当前是否获得授权，如果没有就去获得授权，如果获得授权，就直接获取当前地理位置
							uni.setStorageSync('locationData', '')
							openUrl('/pages/login/unlocate', '1')
						} else { // 如果授权了 直接获取定位信息
							getUserAddress(function(val) {
								let locationData = uni.getStorageSync('locationData')
								if (locationData) {
									let params = {
										city_name: locationData.ad_info.city,
										longitude: locationData.location.lng,
										latitude: locationData.location.lat,
										page: 1,
										page_size: 10,
										pon_type: 'tencent'
									}
									getS(params).then(suc => { // 获取距离最近的门店
										console.log(suc, 'suc')
										let {
											data
										} = suc
										if (data.data.length == 0) {
											_this.setStoreID('')
											_this.setStoreData({})
										} else {
											_this.setStoreID(data.data[0].id)
											_this.setStoreData(data.data[0])
										}
									})
								}
							})
						}
					}
				});
			},
			
			init(){
				return {
					getData: () => {
						
					},
					network: ()=> {
						
					}
				}
			},
			handler(){
				return {
					click: (item)=> {
						const { index, name } = item
						this.tabIndex = index
						// console.log(`点击了`,item, this.tabIndex);
					},
					stopPullDownRefresh: ()=> {
						uni.stopPullDownRefresh()
					},
					// 遮罩点击事件
					closeMask: ()=>{
						this.handler().closeSelect()
					},
					// 关闭下拉框
					closeSelect:()=>{
						this.$refs.dropdown.show = false
						this.current = -1
					},
					// 点击打开下拉框
					openSelect: (index)=> {
						let idx = this.current
						this.current = index
						this.showTop = false
						// console.log('this.current--', this.current, idx);
						if(index===0) {
							this.current = idx===index? -1 : index // 是否选中兼职猫自营
							return
						}
						this.$refs.dropdown.show = true
						if(idx == index) {
							this.$refs.dropdown.show = false
							this.current = -1
						}
					},
					// 确定类型提交
					submitType:(val)=>{
						console.log('确定类型提交', val);
						this.handler().closeSelect()
					},
					// 选中排序
					selectSort:(val) => {
						console.log('选中排序2', val);
						const {item,index} = val
						this.sortIdx = index
						this.handler().closeSelect()
					},
					openLimit: ()=> {
						this.showLimit = !this.showLimit
					},
					// 关闭菜单
					limitCancel:()=>{
						this.showLimit = false
					},
					limitComfirm:(e)=>{
						// console.log('e', e);
						const {value} = e
						this.limit = value[0].name
						this.showLimit = false
					},
				}
			},
			network(){
				return {
					
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.index-page {
		padding-bottom: 30rpx;
		background-color: #FFFFFF;
	}
	
	.top-box {
		width: 750rpx;
		height: 100rpx;
		.top-tab-block {
			width: 300rpx;
		}
		
	}
	
	.select-box {
		padding: 6rpx 20rpx;
		background-color: #eee;
		border-radius: 20rpx;
	}
	
	.place-box {
		color: #999;
		box-sizing: border-box;
		.place-box-left {
			width: 460rpx;
		}
		.place-box-right {
			width: 180rpx;
		}
	}
	
	.near-select-block {
		box-sizing: border-box;
		width: 750rpx;
		// height: 88rpx;
		height: 91rpx;
		/* .select-box {
			height: 40rpx;
		} */
	}
	
	.fixed-top {
		position: fixed;
		top: 0;
		left: 0;
	}
	
</style>
