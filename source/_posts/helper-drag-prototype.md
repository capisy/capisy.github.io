---
title: 拖拽(原型版本)
date: 2017-12-30 00:00:00
tags: JS_HELPER
---

```
const inherit = (function() {
	var F = function() {}
	return function(Target, Origin) {
		F.prototype = Origin.prototype
		Target.prototype = new F()
		Target.prototype.constructor = Target
		Target.prototype.uber = Origin.prototype
	}
})()
```

```
function Drag(el) {
	this.$el = el
	this.clientX = 0
	this.clientY = 0
}

Drag.prototype.init = function() {
	this.$el.onmousedown = this.mouseDown.bind(this)
}

Drag.prototype.mouseDown = function(ev) {
	const left = this.$el.offsetLeft
	const top = this.$el.offsetTop

	this.$el.style.zIndex = 999
	this.$el.style.cursor = 'move'

	this.clientX = ev.clientX - left
	this.clientY = ev.clientY - top

	document.onmousemove = this.mouseMove.bind(this)
	document.onmouseup = this.mouseUp.bind(this)
}

Drag.prototype.mouseMove = function(ev) {
	const x = ev.clientX - this.clientX
	const y = ev.clientY - this.clientY

	this.$el.style.left = x + 'px'
	this.$el.style.top = y + 'px'

	return false
}

Drag.prototype.mouseUp = function() {
	this.$el.style.zIndex = 0
	this.$el.style.cursor = 'default'

	document.onmousemove = null
	document.onmouseup = null
}
```

```
function LimitDrag(el, limit) {
	Drag.call(this, el)
	limit = limit || {}
	this.minX = limit.minX || 0
	this.minY = limit.minY || 0
	this.maxX = limit.maxX || document.documentElement.clientWidth - this.$el.offsetWidth
	this.maxY = limit.maxY || document.documentElement.clientHeight - this.$el.offsetHeight
}

inherit(LimitDrag, Drag)

LimitDrag.prototype.mouseMove = function(ev) {
	let x = ev.clientX - this.clientX
	let y = ev.clientY - this.clientY

	x = Math.max(x, this.minX)
	x = Math.min(x, this.maxX)
	y = Math.max(y, this.minY)
	y = Math.min(y, this.maxY)

	this.$el.style.left = x + 'px'
	this.$el.style.top = y + 'px'

	return false
}
```