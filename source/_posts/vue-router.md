---
title: vue-router
date: 2018-01-31 16:07:00
tags: vue
---

#### 初始化
```js
new Router({
  routes, // 路由配置
  mode: "history",
  fallback:true,
  linkExactActiveClass: "exact-active",
  linkActiveClass: "active",
  scrollBehavior(to,from,savedPosition){
  	if(savedPosition){
		return savedPosition
	}else {
		return {x:0,y:0}
	}
  }
})
```

#### 路由配置
```js
const routes = [
	{
		path:'/',
		redirect:'/home'
	},
	{
		path:'/home',
		component: HomeCmp,
		name:'home',
		meta:{
			title:'首页',
			description:'这是首页'
		},
		children:[
			{
			 	path:'test',
			 	component:TestCmp,
			 	name:'test',
			}
		]
	}
]

<router-link to="/home">home</router-link>
<router-link :to="{name:'home'}">home</router-link>

<router-view></router-view>
```
```js
// 路由过渡动画
.fade-enter-active,.fade-leave-active{
	transition:opacity 0.5s;
}
.fade-enter,.fade-leave-to{
	opacity:0
}
<transition name="fade">
	<router-view />
</transition>
```

#### 路由传参
```js
[
	{
		path:'/app/:id',
		component:AppCmp,
		name:'app'
	},
	{
		path:'/app2/:id2',
		component:App2Cmp,
		name:'app2',
		props:true,
	}

]
let _id = this.$route.params.id // 获取参数
// 注册props
props:['id2']
// 获取id
let _id2 = this.props.id2
```