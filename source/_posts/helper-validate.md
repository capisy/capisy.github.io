---
title: 表单验证
date: 2017-12-30 00:00:00
tags: JS_HELPER
---

```
class ValidateBase {
	constructor(rules) {
		this.errMsg = ''
		this.rules = rules
		this.ruleIdx = 0
	}

	init() {
		return this.parseRules().isValidate()
	}

	parseRules() {
		if (!this.rules.length) return this
		for (const rule of this.rules) {
			const err = this.createErrMsg(rule)
			if (!err) break
		}
		return this
	}

	createErrMsg(rule) {
		this.ruleIdx++
    const ruleTyp = typeof rule['_v']
    
		if (ruleTyp !== 'string') {
			this.errMsg = `R${this.ruleIdx}: Typeof _v is ${ruleTyp}, not string`
			return false
		}

		let flag = true
		const vItems = Object.keys(rule).slice(1)

		for (const vItem of vItems) {
			const transName = ($, $1, $2) => `is${$1.toUpperCase()}${$2}`
			const fnName = vItem.replace(/(\w)(.*)/, transName)

			let _flag = this[fnName](rule)
			if (_flag === false) {
				flag = false
				break
			}
		}
		return flag
	}

	isValidate() {
		return this.errMsg ? { msg: this.errMsg } : false
	}
}

class Validate extends ValidateBase {
	isRequired() {
		// 判断传入的值是否为空
		const { _v, required } = arguments[0]
		if (required[0] && _v.trim() === '') {
			this.errMsg = required[1]
			return false
		}
	}

	isRegExp() {
		// 判断是否通过正则校验
		const { _v, regExp } = arguments[0]
		if (!regExp[0].test(_v)) {
			this.errMsg = regExp[1]
			return false
		}
	}

	isPswAgain() {
		// 判断两次输入的密码是否相同
		const { _v, pswAgain } = arguments[0]
		if (pswAgain[0] !== _v) {
			this.errMsg = pswAgain[1]
			return false
		}
	}
}

export default function(rules) {
	return new Validate(rules).init()
}

```

```
// 调用
const errMsg = validate([
    {
    _v: this.msg,
    required: [true, '不为空'],
    regExp: [/^\w{2,5}$/i, '密码为2-5位'],
    },
	{
		_v: this.msg2,
		required: [true, '再次输入密码为空'],
		pswAgain: [this.msg, '两次输入的密码不同'],
	},
])

if(errMsg){
	// 处理错误信息
	return
}

// 无错误，继续执行...
```

