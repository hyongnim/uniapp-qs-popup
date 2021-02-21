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
}
</style>

<template>
<view>
	<view class="demo-act">
		<button @click="showPop = true">显示弹窗 {{ showPop }}</button>
		<button @click="showPopupFrom('bottom')">从底部弹出</button>
		<button @click="showPopupFrom('left')">从左侧弹出</button>
		<button @click="showPopupFrom('right')">从右侧弹出</button>
		<button @click="showPopupFrom('top')">从上方弹出</button>
	</view>
	
	<hqs-popup title="向下滑动关闭" :from="popFrom"
		v-model="showPop">
		<template v-slot:close v-if="myClose">
			<text>X</text>
		</template>
		
		<view>
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
			myClose: false,
		}
	},
	methods: {
		showPopupFrom(from) {
			this.popFrom = from
			this.showPop = true
		},
	}
}
</script>
