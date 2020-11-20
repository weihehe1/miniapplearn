<template>
	<view class="tab-box">
		<view class="tab-item" :id="'tab-item-' + index" v-for="(item,index) in tabs" :key="index" v-on:click="tabClick" :data-index="index">
			{{item}}
		</view>
		<view :style="[{ left: market_left, width: market_width + 'px' }]" class="market-underline"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				market_left: 0,
				market_width: 0,
			}
		},
		props: {
			tabs: {
				type: Array,
				default: () => ["全部","待付款","待发货","待收货","待评价",]
			},
			currentActive: {
				type: Number,
				default: 0
			}
		},
		mounted(){
			uni.createSelectorQuery().in(this).select('#tab-item-'+ this.currentActive).boundingClientRect().exec((res)=>{
				this.market_left = res[0].left + 'px';
				this.market_width = res[0].width;
			});
		},
		methods: {
			tabClick(e){
				let target = e.target;
				this.activeAction(target);
				this.$emit('click',{index: target.dataset.index})
			},
			activeAction(target){
				let index = Number(target.dataset.index);
				console.log(index)
				this.market_left = target.offsetLeft + 'px';
				uni.createSelectorQuery().in(this).select('#tab-item-' + index).boundingClientRect().exec((res)=>{
					this.market_width = res[0].width;
				});
			}
		}
	}
</script>

<style scoped>
	.tab-box{
		position: relative;
		width: 100%;
		height: 98rpx;
		display: flex;
		z-index: 10;
		background-color: white;
		font-size: 28rpx;
		align-items: center;
		justify-content: space-around;
		color: #333333;
	}
	.tab-item{
		line-height: 98rpx;
		height: 100%;
		flex: 1;
		text-align: center;
	}
	.market-underline{
		height: 6rpx;
		background-color: #797979;
		transition: 0.5s;
		width: 100%;
		position: absolute;
		bottom: 0;
	}
</style>
