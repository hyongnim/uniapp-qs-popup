<style lang="scss">
.list-item {
	padding: 15px;
	border-bottom: 1px solid #eee;
}
.demo-act {
	padding: 15px;
	button {
		margin-top: 15px;
	}
	.item {
		margin-top: 15px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		switch {
			
		}
	}
}
</style>

<template>
<view>
	
	<view class="demo-act">
		<view class="item">
			<text>显示弹窗</text>
			<switch :checked="showPop" @change="onShowPopup"></switch>
		</view>
		<view class="item">
			<text>点击背景可否关闭(maskClick)</text>
			<switch v-model="maskClick" @change="onMaskClick"/>
		</view>
		
		<button @click="showPopupFrom('bottom')">从底部弹出</button>
		<button @click="showPopupFrom('left')">从左侧弹出</button>
		<button @click="showPopupFrom('right')">从右侧弹出</button>
		<button @click="showPopupFrom('top')">从上方弹出</button>
	</view>
	
	<hqs-popup title="向下滑动关闭" :from="popFrom" :mask-click="maskClick"
		v-model="showPop">
		<view>
			<view class="list-item" @click="showPop = false">关闭弹窗</view>
			<view class="list-item"
				v-for="(it, i) in list" :key="i">
				<text>第{{ i }}行</text>
			</view>
		</view>
		<template v-slot:bottom v-if="popFrom == 'top'">
			<view style="padding: 10px; color: #999;">
				从上方弹窗时，如果内容较长，建议使用bottom插槽，可用于上滑关闭操作。
			</view>
		</template>	
	</hqs-popup>
</view>
</template>

<script>
export default {
	data() {
		return {
			showPop: false,
			popFrom: 'bottom',
			list: new Array(20).fill(0),
			maskClick: true,
		}
	},
	methods: {
		onShowPopup(e) {
			this.showPop = e.detail.value
		},
		onMaskClick(e) {
			this.maskClick = e.detail.value
		},
		showPopupFrom(from) {
			this.popFrom = from
			this.showPop = true
		},
	}
}
</script>
