<template>
	<view>
		<!-- 	自定义导航栏 -->
		<nav-bar>
			<text slot="left" class="font-md ml-3">首页</text>
			<template slot="right">
				<view style="width: 60rpx; height: 60rpx;" class="flex align-center justify-center bg-icon rounded-circle mr-3">
					<text class="iconfont icon-zengjia"></text>
				</view>
				<view style="width: 60rpx; height: 60rpx;" class="flex align-center justify-center bg-icon rounded-circle mr-3">
					<text class="iconfont icon-gengduo"></text>
				</view>
			</template>
		</nav-bar>

		<!-- 搜索框 -->
		<view class="px-3 py-2">
			<view class="position-relative">
				<view style="height: 70rpx; width: 70rpx; position: absolute; top: 0; left: 0;" class="flex align-center justify-center text-light-muted">
					<text class="iconfont icon-sousuo"></text>
				</view>
				<input style="height: 70rpx; padding-left: 70rpx;" type="text" class="bg-light font-md rounded-circle" placeholder="搜索网盘文件" />
			</view>
		</view>

		<!-- 资源列表 -->
		<view class="flex flex-column">
			<f-list v-for="item in list" :item="item"></f-list>
		</view>
	</view>
</template>

<script>
	import navBar from '@/components/common/nav-bar.vue'
	import fList from '@/components/common/f-list.vue'
	export default {
		components: {
			navBar,
			fList
		},
		data() {
			return {
				list: [{
						type: 'dir',
						name: '我的笔记',
						create_time: '2020-10-21 08:00',
						checked: true
					},
					{
						type: 'image',
						name: '风景.jpg',
						create_time: '2020-10-21 08:00',
						checked: false
					},
					{
						type: 'video',
						name: 'uniapp实战教程.mp4',
						create_time: '2020-10-21 08:00',
						checked: false
					},
					{
						type: 'text',
						name: '记事本.txt',
						create_time: '2020-10-21 08:00',
						checked: false
					},
					{
						type: 'none',
						name: '压缩包.rar',
						create_time: '2020-10-21 08:00',
						checked: false
					}
				]
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>

</style>
