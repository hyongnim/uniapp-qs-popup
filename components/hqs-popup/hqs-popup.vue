<style lang="scss">
.hqs-popup {
	.qs-con {
		background: #fff;
	}
	.qs-header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		position: relative;
	}
	.qs-title {
		font-size: 16px;
		font-weight: bold;
	}
	.hidden { visibility: hidden; }
	.qs-side {
		min-width: 60px;
		padding: 15px;
		color: #999;
		&:active { opacity: .8; }
	}
	.qs-h-scroll {
		height: 100vh;
		// #ifdef H5
		margin-top: 44px;
		height: calc(100vh - 44px);
		// #endif
	}
	.ta-r { text-align: right; }
}
</style>

<template>
<uni-popup ref="popup" 
	:mask-click="maskClick" @change="onChange"
	:type="from" class="hqs-popup">
	<view class="qs-con" 
		:style="conStyle" 
		@mousedown="onTouch" @mousemove="onTouch" @mouseup="onTouch"
		@touchstart="onTouch" @touchmove="onTouch" @touchend="onTouch">
		<block v-if="isVertical">
			<view class="qs-header" v-if="from == 'bottom' && showHeader">
				<view class="qs-side" :class="{'hidden': !showBack}"
					@click="onBack">
					<slot name="back">
						<text>返回</text>
					</slot>
				</view>
				<slot name="title">
					<text class="qs-title">{{ title }}</text>
				</slot>
				<view class="qs-side ta-r" :class="{'hidden': !showClose}"
					@click="close">
					<slot name="close">
						<text>关闭</text>
					</slot>
				</view>
			</view>
			<slot name="sub-header"></slot>
			<scroll-view scroll-y :style="{ height }"
				@scroll="onScroll">
				<view>
					<slot></slot>
				</view>
			</scroll-view>
			<slot name="bottom"></slot>
		</block>
		<block v-else-if="!isVertical">
			<scroll-view scroll-y class="qs-h-scroll" :style="{ width }">
				<slot></slot>
			</scroll-view>
		</block>
	</view>
</uni-popup>
</template>

<script>
import UniPopup from './uni-popup.vue'

export default {
	components: {
		UniPopup,
	},
	props: {
		// 弹窗显示可通过v-model控制
		value: Boolean,

		// 弹窗打开开始方向
		from: {
			type: String,
			default: 'bottom',
		},
		
		// 内容区边缘圆角大小
		round: {
			type: Number,
			default: 10,
		},
		// 弹窗内容宽度(当from=left或right时起作用)
		width: {
			type: String,
			default: '60vw',
		},
		// 弹窗内容高度(当from=top或bottom时起作用)
		height: {
			type: String,
			default: '50vh',
		},
		
		// 显示默认头部标题栏（仅在底部弹出时有）
		showHeader: {
			type: Boolean,
			default: true,
		},
		// 弹窗标题
		title: String,

		// 显示返回按钮
		showBack: Boolean,

		// 显示关闭按钮
		showClose: {
			type: Boolean,
			default: true,
		},

		// 是否可点击模态背景关闭弹窗
		maskClick: {
			type: Boolean,
			default: true,
		},
	},
	data() {
		return {
			scrollTop: 0,
			panStyle: '',
			showPopup: false,
		}
	},
	computed: {
		isVertical() {
			return ['bottom', 'top'].includes(this.from)
		},
		conStyle() {
			let style = this.panStyle || ''
			let r = this.round
			if(r > 0) {
				r += 'px'
				if(this.from == 'bottom') r = [r, r, 0, 0]
				else if(this.from == 'left') r = [0, r, r, 0]
				else if(this.from == 'right') r = [r, 0, 0, r]
				else r = [0, 0, r, r]
				style += `border-radius: ${r.join(' ')};`
			}
			return style
		},
	},
	watch: {
		value(val) {
			if(val == this.showPopup) return
			if(val) this.open()
			else this.close()
		},
		showPopup(val) {
			this.$emit('input', val)
		},
	},
	mounted() {
		if(this.value) this.open()
	},
	methods: {
		onScroll(e) {
			this.scrollTop = e.detail.scrollTop
		},
		onTouch(ev) {
			// if(!this.maskClick) return
			const { pageX, pageY } = ev.changedTouches[0] || ev
			if(['touchstart', 'mousedown'].includes(ev.type)) {
				this.startX = pageX
				this.startY = pageY
				this.startTime = ev.timeStamp
			} else {
				if(!this.startTime) return
				const orien = this.isVertical ? 'Y' : 'X'
				let moveDis = pageY - this.startY
				if(this.from == 'left') moveDis = this.startX - pageX
				else if(this.from == 'right') moveDis = pageX - this.startX
				else if(this.from == 'top') moveDis = -moveDis
				if(!this.maskClick) moveDis /= 3
				const duration = (ev.timeStamp - this.startTime)
				const speed = moveDis/duration
				if(['touchend', 'mouseup'].includes(ev.type)) {
					if(this.panStyle) {
						if(this.maskClick && (moveDis > 120 || speed > 0.25)) {
							this.close()
						}
						else {
							this.panStyle = `transform: translate${orien}(0); transition: all ease 200ms;`
						}
						setTimeout(() => {
							this.panStyle = ''
						}, 300)
					}
					// conScrollTop = 0
					this.startTime = 0
					return
				}
				// if(this.scrollTop > 0) return
				
				if(moveDis > 0) {
					if(this.from == 'left' || this.from == 'top') moveDis *= -1
					this.panStyle = `transform: translate${orien}(${moveDis}px); transition: none;`
				}
			}
		},
		onChange({ show }) {
			this.showPopup = show
		},
		open() {
			this.$refs.popup.open()
			this.showPopup = true
		},
		close() {
			this.$refs.popup.close()
			this.showPopup = false
		},
		onBack() {
			this.$emit('back')
		},
	}
}
</script>
