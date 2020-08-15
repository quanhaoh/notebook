### 1.view
### 2.scroll-view
可滚动视图区域
### 3.movable-view
可移动的视图，可拖拽
- movable-view必须作为movable-area的子节点
### 4.movable-area
<movable-view>的可移动区域
### 5.cover-view
覆盖在原生组件上的文本
- 可覆盖的原生组件包括 map、video、canvas、camera、live-player、live-pusher
- 只支持嵌套 cover-view、cover-image，可在 cover-view 中使用 button。
### 6.cover-image
覆盖在原生组件上的图片
- 覆盖组件范围与cover-view相同
- 仅支持嵌套在cover-view中
