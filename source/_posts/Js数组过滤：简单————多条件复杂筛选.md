---
title: Js数组过滤：简单————>多条件复杂筛选
date: 2023-08-08 21:05:10
tags:
categories: 前端知识
---

在前端部分完成筛选功能，一次拿到所有数据，然后根据条件筛选。通常情况下筛选是后台给接口，在数据量不大的情况下，也有人可能会遇到前端筛选这样的情况。

```javascript
// 这个是例子中的被筛选数组
var aim = [
  { name: "Anne", age: 23, gender: "female" },
  { name: "Leila", age: 16, gender: "female" },
  { name: "Jay", age: 19, gender: "male" },
  { name: "Mark", age: 40, gender: "male" },
];
```

#### 1.单条件单数据筛选

```javascript
// 根据单个名字筛选
function filterByName(aim, name) {
  return aim.filter((item) => item.name == name);
}
// 输入 aim 'Leila' 期望输出为 [{name:'Leila', age: 16, gender:'female'}]
console.log(filterByName(aim, "leila"));
```

#### 2.单条件多数据筛选

根据多个名字筛选，这里是用 for 循环遍历目标数组，然后用 find 方法找到后 push 到结果数组里，用 find 方法是重名情况下也能得到想要的结果。for 循环可以用数组的一些遍历方法替代，代码可以更简化，示例就是大概表达个意思。

```javascript
// 根据多个名字筛选
function filterByName1(aim, nameArr) {
  let result = [];
  for (let i = 0; i < nameArr.length; i++) {
    result.push(aim.find((item) => (item.name = nameArr[i])));
  }
  return result;
}
// 输入 aim ['Anne','Jay']
//期望输出为 [{name:'Anne', age: 23, gender:'female'},{name:'Jay', age: 19, gender:'male'}]
console.log(filterByName1(aim, ["Leila", "Jay"]));
```

#### 3.多条件单数据筛选:

根据单个名字或者单个年龄筛选，用 filter 方法，判断条件之间是或的关系。

```javascript
// 根据名字或者年龄筛选
function filterByName2(aim, name, age) {
  return aim.filter((item) => item.name == name || item.age == age);
}
console.log(filterByName2(aim, "Leila", 19));
```

#### 4.多条件多数据筛选:

最初是用了很笨的双 for 循环去做，发现很慢，而且并没有达到预期的效果。具体的心路历程已经太遥远，简单介绍以下这个筛选算法。

首先是把筛选条件都塞到一个对象里，用 Object 对象的 keys 方法获取到筛选的条件名，及需要筛选的是哪个条件，是 name?age? gender?然后使用 Filter 方法对目标数据进行筛选，根据名字和年龄多元素筛选

```javascript
//根据名字和年龄多元素筛选
export function multiFilter(array, filters) {
  const filterKeys = Object.keys(filters);
  // filters all elements passing the criteria
  return array.filter((item) => {
    // dynamically validate all filter criteria
    return filterKeys.every((key) => {
      //ignore when the filter is empty Anne
      if (!filters[key].length) return true;
      return !!~filters[key].indexOf(item[key]);
    });
  });
}
/*
 * 这段代码并非我原创，感兴趣的可以去原作者那里点个赞
 * 作者是：@author https://gist.github.com/jherax
 * 这段代码里只加了一行，解决部分筛选条件清空时候整体筛选失效的问题
 */

var filters = {
  name: ["Leila", "Jay"],
  age: [],
};
/* 结果：
 * [{name: "Leila", age: 16, gender: "female"},
 *  {name: "Jay", age: 19, gender: "male"}]
 */
```

例如这里，判断每条数据的 name 值是否在 filters.name 数组里，是的话返回 true，判断 filters.age 是空数组的话直接返回 true，空数组是模拟了 age 条件被清空的情况，我们仍然能得到正确的筛选数据。
