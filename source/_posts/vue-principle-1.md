---
title: Object.defineProperty实现双向数据绑定
date: 2018-10-05 19:07:00
tags: vue
catalog: true
---

#### 原理

```html
<input type="text" name="" id="inp">
<span id='txt'></span>
```

```js
let obj = {}

Object.defineProperty(obj, 'txt', {
    set(val) {
        document.querySelector('#inp').value = val
        document.getElementById('txt').innerText = val
    }
})

document.getElementById('inp').onkeyup = function (e) {
    obj.txt = e.target.value
}
```

#### 数据初始化绑定

```html
<div id='app'>
    <input type="text" v-model="txt">
    {{txt}}
</div>
```

````js
function compile(node, vm) {
    var reg = /\{\{(.*)\}\}/

    if (node.nodeType === 1) { // nodeType：element
        var attr = node.attributes
        for (var i = 0, item; item = attr[i++];) {
            if (item.nodeName === 'v-model') {
                var name = item.nodeValue
                node.value = vm.data[name]
                node.removeAttribute('v-model')
            }
        }
    }

    if (node.nodeType === 3) { // nodeType：文本
        if (reg.test(node.nodeValue)) {
            var name = RegExp.$1
            name = name.trim()
            node.nodeValue = vm.data[name]
        }
    }

}

function nodeToFragment(node, vm) {
    var flag = document.createDocumentFragment()
    var child

    while (child = node.firstChild) {
        compile(child, vm)
        flag.appendChild(child)
    }
    return flag
}

function Vue(options) {
    this.data = options.data
    var id = options.el
    var dom = nodeToFragment(document.getElementById(id), this)
    document.getElementById(id).appendChild(dom)
}

var vm = new Vue({
    el: 'app',
    data: {
        txt: 'hello world'
    }
})
````

#### 响应式的数据绑定

```js
/**
* 把data中的属性设置为 vm 的访问器属性
* obj:vm
* key:data中的属性
* val：data属性对应的属性值
*/
function defineReactive(obj, key, val) {
    Object.defineProperty(obj, key, {
        get() {
            return val
        },
        set(newVal) {
            if (newVal === val) return
            val = newVal
        }
    })
}

/**
* obj:data
* vm:Vue实例
*/
function observe(obj, vm) {
    Object.keys(obj).forEach(item => {
        defineReactive(vm, item, obj[item])
    })
}

function compile(node, vm) {
    var reg = /\{\{(.*)\}\}/

    if (node.nodeType === 1) {
        var attr = node.attributes
        for (var i = 0, item; item = attr[i++];) {
            if (item.nodeName === 'v-model') {
                var name = item.nodeValue
                
                node.addEventListener('input', e => {
                    vm[name] = e.target.value
                })

                node.value = vm[name]
                node.removeAttribute('v-model')
            }
        }
    }

    if (node.nodeType === 3) {
        if (reg.test(node.nodeValue)) {
            var name = RegExp.$1
            name = name.trim()
            node.nodeValue = vm[name]
        }
    }

}

function nodeToFragment(node, vm) {
    var flag = document.createDocumentFragment()
    var child

    while (child = node.firstChild) {
        compile(child, vm)
        flag.appendChild(child)
    }
    return flag
}

function Vue(options) {
    this.data = options.data

    observe(this.data, this)

    var id = options.el
    var dom = nodeToFragment(document.getElementById(id), this)
    document.getElementById(id).appendChild(dom)
}
```

#### vm 的访问器属性发生改变时，更新视图

```js
/*
* 观察者模式：
*/
class Dep {
    constructor() {
        this.subs = [] 
    }
    addSub(sub) {
        this.subs.push(sub)
    }
    notify() {
        this.subs.forEach(item => item.update())
    }
}

var dep = new Dep()

function defineReactive(obj, key, val) {
    Object.defineProperty(obj, key, {
        get() {
            if (Dep.target) dep.addSub(Dep.target)
            return val
        },
        set(newVal) {
            if (newVal === val) return
            val = newVal
            dep.notify()
        }
    })
}

function observe(obj, vm) {
    Object.keys(obj).forEach(item => {
        defineReactive(vm, item, obj[item])
    })
}

class Watcher {
    constructor(vm, node, name) {
        Dep.target = this
        this.vm = vm
        this.node = node
        this.name = name
        this.update()
        Dep.target = null
    }
    update() {
        this.node.nodeValue = this.vm[this.name] // 触发对应访问器的get方法
    }
}

function compile(node, vm) {
    var reg = /\{\{(.*)\}\}/

    if (node.nodeType === 1) {
        var attr = node.attributes
        for (var i = 0, item; item = attr[i++];) {
            if (item.nodeName === 'v-model') {
                var name = item.nodeValue

                node.addEventListener('input', e => {
                    vm[name] = e.target.value
                })

                node.value = vm[name]
                node.removeAttribute('v-model')
            }
        }
    }

    if (node.nodeType === 3) {
        if (reg.test(node.nodeValue)) {
            var name = RegExp.$1
            name = name.trim()
            new Watcher(vm, node, name)
        }
    }

}

function nodeToFragment(node, vm) {
    var flag = document.createDocumentFragment()
    var child

    while (child = node.firstChild) {
        compile(child, vm)
        flag.appendChild(child)
    }
    return flag
}

function Vue(options) {
    this.data = options.data

    observe(this.data, this)

    var id = options.el
    var dom = nodeToFragment(document.getElementById(id), this)
    document.getElementById(id).appendChild(dom)
}

var vm = new Vue({
    el: 'app',
    data: {
        txt: 'hello world'
    }
})
```

