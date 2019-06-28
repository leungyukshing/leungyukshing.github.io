---
title: 前端处理JSON的正确姿势
abbrlink: 56958
date: 2019-06-19 13:03:10
tags:
---

## JSON

&emsp;&emsp;JSON（JavaScript Object Notation），JS对象简谱，是一种轻量级的数据交换格式，常用于前端页面与后端的数据交互，它易于人阅读和编写，也易于机器解析和生成，具有较好的效率。

<!-- more -->

---

## 问题描述

&emsp;&emsp;在挣闲钱应用中，实现问卷功能时，我采用了JSON数组的形式去存储每一个问卷，而对于每个问卷的题目列表，同样也是使用JSON数组。也就是说与后端交互的数据是一个JSON嵌套数组。大致格式如下：

```json
qslist: [{ "num": 3,
            "title": "第三份问卷",
            "time": "2017-3-27",
            "state": true,
            "stateTitle": "已发布",
            "checked": false,
            "question": [
              {"num": "Q1", "title": "单选题", "type": "radio", "isNeed": true, "options": ["选项一", "选项二"]},
              {"num": "Q2", "title": "多选题", "type": "checkbox", "isNeed": true, "options": ["选项一", "选项二", "选项三", "选项四"]},
              {"num": "Q3", "title": "文本题", "type": "textarea", "isNeed": true}
            ]
          }]
```

&emsp;&emsp;首先js提供了处理JSON数据的方法：

+ 封装成字符串：`JSON.stringify()`
+ 解析成JSON：`JSON.parse()`

&emsp;&emsp;但遇到的问题是JSON内`question`如何封装和解析。因为如果不特别处理的话，在前端获取数据后使用`JSON.parse()`解析得到的`question`是一个字符串，而页面需要一个JSON的数组才能够正确渲染。

---

## 解决思路

&emsp;&emsp;原因就出在`question`这里，我们需要首先对question进行JSON的封装处理，让他变成可解析的JSON字符串传给后端，再对外层的JSON进行封装。解析的时候也是先解析外面的，然后再解析`question`，这样就可以解决了。

&emsp;&emsp;上面这种解决思路是比较简单方便的，但是不断查看格式耗费了我好多时间，尤其是引号带来的格式、类型问题。之前想着如果JSON解决不了的话，还有另一种思路，这里也和大家分享一下。可以直接将内容进行编码，转成16进制传给后端，抓取数据的时候再解析，当然这就比较麻烦了，但是也不失为一种解决方法。

**特别注意：**JSON官网给出的标准是JSON中的键值对只要是字符串都是用`"`，而不要使用`'`，否则在解析的过程中会出现错误。

## 代码

封装并上传数据：

```js
// Save in the database
console.log(qsItem)
axios.post(url_post, JSON.stringify({
    title: qsItem.title,
    content: JSON.stringify(qsItem.question) // 封装JSON数据为字符串！！！
}))
 .then(response => {
    console.log(response.data)
  })
 .catch(error => {
    console.log(error)
  })
```

获取并解析数据：

```js
// 获取问卷列表
axios.get(url)
 .then(response => {
    temp = response.data.data
    // Handle list
    temp.forEach(item => {
        let questionnaire = {
            "num": item.id,
            "title": item.title,
            "stateTitle": item.state == 0 ? '未发布' : '已发布',
            "time": item.create_time.substr(0, 10),
            "state": item.state == 0 ? false: true,
            "checked": item.checked == 0 ? false: true,
            "question": JSON.parse(item.content) // 解析JSON字符串！！！
            }
         this.qslist.push(questionnaire)
     })
 })
 .catch(error => {
    console.log(error)
 })
```

---

## 小结

&emsp;&emsp;JSON格式的数据接触过很多了，之前Andriod也接触过，不过安卓比较友好，使用JSONObject就能够很方便地解决。想了解Android网络请求的可以到[Tarlesh](https://github.com/leungyukshing/AndroidFinalProject)参考。

&emsp;&emsp;这里遇到的问题虽然解决方法看似很简单，但是找错误的过程是比较繁琐的，也希望这篇博客能够帮助到您，谢谢！

---

## Reference

1. [JSON官网](http://json.com/)
2. [JSON.parse()使用文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)
3. [JSON.stringify()使用文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)
4. [JSON使用——单引号双引号问题](https://www.cnblogs.com/xianfangloveyangmei/p/4494597.html)