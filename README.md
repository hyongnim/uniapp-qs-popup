## uniapp-qs-popup

### 说明

hqs-popup弹窗组件是基于uni-popup组件开发的，增加了手势滑动关闭弹窗功能，体验和抖音评论下滑关闭类似，欢迎使用~

### 使用方式

```js
<hqs-popup title="弹窗标题" v-model="showPopup">
	<view>弹窗内容</view>
</hqs-popup>
```

### 组件属性

```js
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
// 弹窗内容容器宽度(当from=left或right时起作用)
width: {
	type: String,
	default: '60vw',
},
// 弹窗内容容器高度(当from=top或bottom时起作用)
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
```


### 组件插槽

`close`：关闭按钮插槽；

`sub-header`：从底部弹出时，默认有头部标题栏，此插槽在标题栏下方；

`bottom`： 垂直方向弹窗，内容区底部插槽；

