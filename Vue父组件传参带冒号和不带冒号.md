---
title: Vue父组件传参带冒号和不带冒号
tags: 父子组件通信
slug:  storywriter/tutorial
---


``` elixir
<FormInput @input="val=>username=val" :name="username" :value="username" placeholder="Username" />
```

``` css
<FormInput @input="val=>username=val" name="username" value="username" placeholder="Username" />
```
第一段代码中，父组件传name和value给子组件，子组件得到的name和value的值会是username这个变量的值。

第二段diamante中，name和value前没有冒号，子组件得到的name和value值是单纯的字符串‘username’。

谨记！