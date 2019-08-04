---
title: swiper滚屏爬坑
date: 2019-07-17 11:18:55
tags: vue
catalog: true
---

#### 需求
- 全屏滚动，不是第一屏的时候显示 header

#### 问题
- 系统的 scrollTop 会被禁用
- 监听 this.\$refs.mouseWheel.swiper,这里的 activeIndex 不会实时更新

#### 解决方案
- 引入 swiper
```js
import Vue from "vue";
import VueAwesomeSwiper from "vue-awesome-swiper";
import "swiper/dist/css/swiper.css";

Vue.use(VueAwesomeSwiper);
export default VueAwesomeSwiper;
```
- 封装 mouse-wheel 组件
```js
<template>
  <swiper :options="swiperOption" class="mouse-wheel" ref="mouseWheel">
    <slot></slot>
    <div class="swiper-pagination" slot="pagination"></div>
  </swiper>
</template>

<script>
import '@/plugin/swiper'
import { mapMutations } from 'vuex'
let t
export default {
	name: 'mouseWheel',
	created() {
		t = this
	},
	data() {
		return {
			swiperOption: {
				direction: 'vertical',
				slidesPerView: 1,
				mousewheel: true,
				speed: 400,
				pagination: {
					el: '.swiper-pagination',
					clickable: true,
				},
				on: {
					init() {
						t.swiper = this
					},
					slideChangeTransitionEnd: function() {
						const curIdx = t.swiper.activeIndex
						t.SWIPER_INDEX(curIdx)
					},
				},
			},
		}
	},
	methods: {
		...mapMutations(['SWIPER_INDEX']),
	},
}
</script>

<style lang="scss">
$color: #d2d4d6;
.mouse-wheel {
	height: 100vh;
	.swiper-pagination {
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.swiper-pagination-bullet {
		width: 12px;
		height: 12px;
		background-color: $color !important;
		opacity: 1 !important;
	}
	.swiper-pagination-bullet-active {
		width: 20px;
		height: 20px;
		border: 3px solid $color;
		background: none !important;
	}
}
</style>
```
- 调用 mouse-wheel 组件
```js
<template>
  <v-mouse-wheel>
    <swiper-slide>
      <v-header />
      <v-banner />
    </swiper-slide>
    <swiper-slide>
      <v-investment />
    </swiper-slide>
  </v-mouse-wheel>
</template>
```
- 手动触发（header 组件中锚点指向某屏）
```js
computed: {
	swiper() {
		return this.$parent.$parent.$parent.$refs.mouseWheel.swiper
	},
},
methods: {
	moveToSlide(idx) {
		this.swiper.slideTo(idx, 500, false)
	},
},
```
