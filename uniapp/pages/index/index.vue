<template>
	<view class="content">
		<view class="open" @tap="openinfo">
			index
		</view>
		<view class="unload" @tap="unload">
			图片
		</view>
		<view class="localtion" @tap="getlocal">当前位置</view>
		<!-- <navigator url="'../info/info?index='+navData">跳转info</navigator> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				news:[]
			}
		},
		onLoad() {
			uni.request({
				url:'http://192.168.0.168:11301/api/base/sys/dict/getDictItems/company_nature',
				method:'GET',
				success: res => {
					this.news = res.data.result;
					// console.log(this.news)
				},
				fail:()=>{},
				complete:res=>{
					// console.log("complete")
				}
			}),
			uni.setTabBarItem({
			  index: 0,
			  text: 'text',
			})
			
		},
		methods: {
			openinfo(){//路由跳转
				uni.navigateTo({
					url: '../info/info',
				});
			},
			unload(){//图片
				uni.chooseImage({
					count:1,
					sourceType:['album'],
					 success: function (res) {
						 var tempFilePaths = res.tempFilePaths;
						 uni.saveFile({
							tempFilePath: tempFilePaths[0],
							success: function (res) {
								console.log(res)
								var savedFilePath = res.savedFilePath;
							}
						});
					}
				})
			},
			getlocal(){//地图选点
				uni.chooseLocation({
				    success: function (res) {
				        console.log('位置名称：' + res.name);
				        console.log('详细地址：' + res.address);
				        console.log('纬度：' + res.latitude);
				        console.log('经度：' + res.longitude);
				    }
				});
			}
		}
	}
</script>

<style>
	.open,.localtion{
		width: 10ups;
		height: 10ups;
		background-color: #007AFF
	}
	.unload{
		width: 10ups;
		height: 10ups;
		background-color: #09BB07
	}
</style>
