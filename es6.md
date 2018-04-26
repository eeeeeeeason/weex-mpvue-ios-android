# es6	

- set去重。只支持简单数据类型，对象不能去重
  - 问题。set和map的区别

```js
obj.prodlist = obj.prodlist.reduce(function (item, next) {
  hash[next.prodid] ? '' : hash[next.prodid] = true && item.push(next)
  return item
}, [])
```

- assign 合并对象
- 解构赋值串联分页内容



