<template>
	<view class="page">
		<!-- 微信小程序端 map组件无层级问题 -->
		<view class="uni-common-mt main-bg c-white p-30">
			<view class="f-s-36">兼职猫为您保驾护航，做好防护提醒</view>
			<view class="f-s-20 m-t-60 p-b-10">数据来自国家及地方卫健委，更新至2021-12-14 10:26 21</view>
		</view>

		<view class="map-search">
			<u-search v-model="keyword" placeholder="请输入地址" bgColor="#fff" :show-action="false" :show-location="true" @change="change">
			</u-search>
		</view>

		<view>
			<map id="map" :latitude="latitude" :longitude="longitude" :markers="covers" :show-compass="true"></map>
		</view>

		<view class="map-bottom bg-fff top-shadow p-30">
			<view class="row-between m-t-b-20">
				<view class="row center">
					<u-icon name="map" size="14"></u-icon>
					<view class="f-s-28 m-l-10">所选位置<text class="main-color">500m</text>内</view>
				</view>
				<view class="row-between b-r-24 main-bg c-white p-t-10 p-b-10 p-l-20 p-r-20 f-s-22">
					<u-icon name="share" size="14" color="#fff"></u-icon>分享
				</view>
			</view>
			<view class="f-s-28 p-b-30">
				暂无疫情，请做好保护防护
			</view>
		</view>

	</view>
</template>
<script>
	const img = '@/static/logo.png';
	export default {
		data() {
			return {
				keyword: '',
				title: 'map',
				latitude: 39.909,
				longitude: 116.39742,
				covers: [{
					latitude: 39.9085,
					longitude: 116.39747,
					// #ifdef APP-PLUS
					iconPath: '../../../static/app-plus/location@3x.png',
					// #endif
					// #ifndef APP-PLUS
					iconPath: '../../../static/location.png',
					// #endif
				}, {
					latitude: 39.90,
					longitude: 116.39,
					// #ifdef APP-PLUS
					iconPath: '../../../static/app-plus/location@3x.png',
					// #endif
					// #ifndef APP-PLUS
					iconPath: '../../../static/location.png',
					// #endif
				}]
			}
		},
		onReady() {
			this._mapContext = uni.createMapContext("map", this);

			// 仅调用初始化，才会触发 on.("markerClusterCreate", (e) => {})
			this._mapContext.initMarkerCluster({
				enableDefaultStyle: false,
				zoomOnClick: true,
				gridSize: 60,
				complete(res) {
					console.log('initMarkerCluster', res)
				}
			});

			this._mapContext.on("markerClusterCreate", (e) => {
				console.log("markerClusterCreate", e);
			});

			this.addMarkers();
		},
		methods: {
			// 搜索框事件
			change(e) {
				console.log(e);
			},
			addMarkers() {
				const marker = {
					id: 1,
					iconPath: img,
					width: 50,
					height: 50,
					joinCluster: true, // 指定了该参数才会参与聚合
					label: {
						width: 50,
						height: 30,
						borderWidth: 1,
						borderRadius: 10,
						bgColor: '#ffffff'
					}
				};

				const positions = [{
					latitude: 23.099994,
					longitude: 113.324520,
				}, {
					latitude: 23.099994,
					longitude: 113.322520,
				}, {
			  latitude: 23.099994,
					longitude: 113.326520,
				}, {
					latitude: 23.096994,
					longitude: 113.329520,
				}]

				const markers = []

				positions.forEach((p, i) => {
					const newMarker = Object.assign(marker, p)
					newMarker.id = i + 1
					newMarker.label.content = `label ${i + 1}`
					markers.push(newMarker)

					this._mapContext.addMarkers({
						markers,
						clear: false,
						complete(res) {
							console.log('addMarkers', res)
						}
					})
				})
			},
			moveToLocation(){
				const { latitude, longitude } = this
				this._mapContext.moveToLocation({
					latitude,
					longitude
				})
			}
		}
	}
</script>
<style lang="scss" scoped>
	map {
		width: 100%;
		// height: 600rpx;
		height: calc(100vh - 110rpx);
	}

	.map-search {
		position: fixed;
		/* #ifdef MP-WEIXIN */
		top: 19%;
		/* #endif */
		/* #ifdef H5 */
		top: 24%;
		/* #endif */
		left: 30rpx;
		z-index: 99;
		width: 540rpx;
	}

	.map-bottom {
		position: fixed;
		bottom: 0;
		left: 0;
		width: 750rpx;
		box-sizing: border-box;
		border-radius: 24rpx 24rpx 0 0;
		z-index: 99;
	}
</style>
