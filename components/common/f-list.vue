<template>
	<view class="p-3 flex align-center border-bottom border-light-secondary">
		<text class="iconfont" :class="iconClass" :style="colorClass"></text>
		<!-- <text class="iconfont" :class="iconClass" style="font-size:60rpx"></text> -->
		<view class="flex flex-column ml-3" style="line-height: 1.2;">
			<text class="font-md">{{item.name}}</text>
			<text class="font-sm text-muted">{{item.create_time}}</text>
		</view>
		<!-- <view class="ml-auto flex align-center justify-center" style="width: 70rpx; height: 70rpx;">
			<text v-if="!item.checked" style="height: 25rpx; width: 25rpx;" class="rounded-circle border"></text>
			<text v-else class="iconfont icon-xuanze-yixuan text-primary" style="font-size: 40rpx;"></text>
		</view> -->
		<view class="ml-auto flex align-center justify-center" style="width: 70rpx;height: 70rpx;" @click.stop="select">
			<text v-if="!item.checked" style="height: 40rpx;width: 40rpx;" class="rounded-circle border-dark"></text>
			<text v-else class="iconfont icon-xuanze-yixuan text-primary" style="font-size: 40rpxs"></text>
		</view>
	</view>
</template>

<script>
	const icons = {
		dir: {
			icon: 'icon-file-wenjianjia',
			color: '#FFB400'
		},
		image: {
			icon: 'icon-file-jpg',
			color: '#00aa00'
		},
		video: {
			icon: 'icon-file-mp4',
			color: '#55aaff'
		},
		text: {
			icon: 'icon-file-TXTCopy',
			color: '#aaaa7f'
		},
		none: {
			icon: 'icon-file-rar',
			color: '#b18873'
		}
	};
	export default {
		props:{
			item: Object,
			index: [Number,String]
		},
		computed:{
			iconClass(){
				let item = icons[this.item.type];
				return item.icon
			},
			colorClass(){
				let item = icons[this.item.type];
				return 'font-size: 60rpx; color:'+`${item.color}`+';';
			}
			// iconClass() {
			// 	let item = icons[this.item.type];
			// 	return `${item.icon} ${item.color}`;
			// }
		},
		methods: {
			select() {
				this.$emit('select', {
					index: this.index,
					value: !this.item.checked
				});
			}
		},
	}
</script>

<style>
</style>
