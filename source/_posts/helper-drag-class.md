---
title: 拖拽(class版本)
date: 2015-12-30 00:00:00
tags: JS_HELPER
---

```
class Drag {
	constructor(el) {
		this.$el = el
		this.clientX = 0
		this.clientY = 0
	}

	init() {
		this.$el.onmousedown = this.mouseDown.bind(this)
	}

	mouseDown(ev) {
		const left = this.$el.offsetLeft
		const top = this.$el.offsetTop

		this.$el.style.zIndex = 999
		this.$el.style.cursor = 'move'

		this.clientX = ev.clientX - left
		this.clientY = ev.clientY - top

		document.onmousemove = this.mouseMove.bind(this)
		document.onmouseup = this.mouseUp.bind(this)
	}

	mouseMove(ev) {
		const x = ev.clientX - this.clientX
		const y = ev.clientY - this.clientY

		this.$el.style.left = x + 'px'
		this.$el.style.top = y + 'px'

		return false
	}

	mouseUp() {
		this.$el.style.zIndex = 0
		this.$el.style.cursor = 'default'

		document.onmousemove = null
		document.onmouseup = null
	}
}
```

```
class LimitDrag extends Drag {
	constructor(el, limit = {}) {
		super(el)
		this.minX = limit.minX || 0
		this.minY = limit.minY || 0
		this.maxX = limit.maxX || document.documentElement.clientWidth - this.$el.offsetWidth
		this.maxY = limit.maxY || document.documentElement.clientHeight - this.$el.offsetHeight
	}

	mouseMove(ev) {
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
}
```

```
export { Drag, LimitDrag }
```

