<template>
	<view>
		<!-- 	自定义导航栏 -->
		<nav-bar>
			<template v-if="checkCount === 0">
				<text slot="left" class="font-md ml-3">首页</text>
				<template slot="right">
					<view style="width: 60rpx; height: 60rpx;" class="flex align-center justify-center bg-icon rounded-circle mr-3">
						<text class="iconfont icon-zengjia"></text>
					</view>
					<view style="width: 60rpx; height: 60rpx;" class="flex align-center justify-center bg-icon rounded-circle mr-3">
						<text class="iconfont icon-gengduo"></text>
					</view>
				</template>
			</template>

			<template v-else>
				<view slot='left' class="font-md ml-3 text-primary" @click="handleCheckAll(false)">取消</view>
				<text class="font-md font-weight-bold">已选中{{checkCount}}</text>
				<view slot='right' class="font-md mr-3 text-primary" @click="handleCheckAll(true)">全选</view>
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
			<!-- <f-list v-for="item in list" :item="item"></f-list> -->
			<!-- 列表 -->
			<f-list v-for="(item, index) in list" :key="index" :item="item" :index="index" @select="select"></f-list>
		</view>

		<!-- 底部操作条 -->
		<!-- 选中个数大于0才会出现这个操作条 -->
		<view v-if="checkCount > 0">

			<!-- 这里要留一定的高度，因为底部操作条需要被固定在底部，并空出底部tabbar高度的地方 -->
			<view style="height: 115rpx;;"></view>
			<!-- 操作条容器的样式，高度，颜色，固定在底部，垂直方向拉升效果 -->
			<view style="height: 115rpx;" class="flex align-stretch bg-primary text-white fixed-bottom">
				<!-- 根据元素个数等分容器，所以要么四等分，要么二等分，行高的修改可以让图标和文字之间的距离变得合理，点击还会变色：hover-class -->
				<view
					class="flex-1 flex flex-column align-center justify-center"
					style="line-height: 1.5;"
					v-for="(item, index) in actions"
					:key="index"
					hover-class="bg-hover-primary"
					@click="handleBottomEvent(item)"
				>
					<text class="iconfont" :class="item.icon"></text>
					{{ item.name }}
				</view>
			</view>
		</view>
	
		<!-- 是否要删除 -->
		<f-dialog ref="dialog">是否删除选中的文件？</f-dialog>
	</view>
</template>

<script>
	import navBar from '@/components/common/nav-bar.vue'
	import fList from '@/components/common/f-list.vue'
	import fDialog from '@/components/common/f-dialog.vue'
	export default {
		components: {
			navBar,
			fList,
			fDialog
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
		computed: {
			// 选中列表
			checkList() {
				return this.list.filter(item => item.checked);
			},
			// 选中数量
			checkCount() {
				return this.checkList.length;
			},
			// 操作菜单
			actions() {
				if (this.checkCount > 1) {
					return [{
						icon: "icon-xiazai",
						name: "下载"
					}, {
						icon: "icon-shanchu",
						name: "删除"
					}]
				}
				return [{
					icon: "icon-xiazai",
					name: "下载"
				}, {
					icon: "icon-fenxiang-1",
					name: "分享"
				}, {
					icon: "icon-shanchu",
					name: "删除"
				}, {
					icon: "icon-chongmingming",
					name: "重命名"
				}]
			}
		},
		methods: {
			select(e) {
				//接收到子组件传递过来的索引选中状态，将对应的list中的数据更新
				this.list[e.index].checked = e.value
			},
			// 全选/取消全选
			handleCheckAll(checked) {
				this.list.forEach(item => {
					item.checked = checked;
				});
			},
			// 处理底部操作条事件,这里仅对"删除"做了处理
			handleBottomEvent(item) {
				switch (item.name) {
					case '删除':
						this.$refs.dialog.open(close => {
							//对list进行过滤，留下来的未被选中的
							this.list = this.list.filter(item => !item.checked);
							close();
							uni.showToast({
								title: '删除成功',
								icon: 'none'
							});
						});
						break;
						case '重命名':
							//重命名只能对单个文件进行，所以取this.checkList[0],也就是选中的唯一元素
							this.renameValue = this.checkList[0].name;
							this.$refs.rename.open(close => {
								if (this.renameValue == '') {
									return uni.showToast({
										title: '文件名称不能为空',
										icon: 'none'
									});
								}
								//更新钙元素的name值，实时看到效果
								this.checkList[0].name = this.renameValue;
								close();
							});
							break;
					default:
						break;
				}
			}
		}
	}
</script>

<style>

</style>
