---
title: Vue导航守卫
tags: 路由
slug:  storywriter/tutorial
---


# 
vue-router提供的导航守卫是用来控制导航的，也就是控制window.location.href的变化，并在变化的前后做一些处理。目前vue提供的导航守卫有三种：全局的，路由的，组件的。
# 全局前置守卫

``` javascript
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

这段代码一般是应用在main.js中，

beforeEach钩子是在路由变化前做处理，该方法一共有三个参数，to，from，next。
参数的语义比较明显，to是即将要去往的路由，from是即将离开的路由，而next相当于一个关卡守卫，需要调用这个守卫（也就是守卫统一放行，哈哈）来resolve这个钩子。

要注意的是，next可以接收不同的参数：
next()表示放行，顺利进入下一个路由。
next(false)表示阻止，无法进入下一个路由，操作被中断。
next('/')表示中断当前的导航，进入另外的目标路由。
next(error),如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 router.onError() 注册过的回调。

``` javascript
router.afterEach((to, from) => {
  // ...
})
```

afterEach与beforeEach类似，不过afterEach是导航已经到达目标路由后的钩子。需要注意的是，afterEach没有next方法。
# 路由守卫
你可以在路由配置上直接定义 beforeEnter 守卫：

``` javascript
const router = new VueRouter({
         routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

这些守卫与全局前置守卫的方法参数是一样的。

# 组件守卫

``` q
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

需要注意是beforeRouteEnter 守卫 不能 访问 this，因为守卫在导航确认前被调用,因此即将登场的新组件还没被创建。

___

# 完整的导航流程（官方）


 1. 导航被触发。
 2. 在失活的组件里调用离开守卫。
 3. 调用全局的 beforeEach 守卫。
 4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
 5. 在路由配置里调用 beforeEnter。
 6. 解析异步路由组件。
 7. 在被激活的组件里调用 beforeRouteEnter。
 8. 调用全局的 beforeResolve 守卫 (2.5+)。
 9. 导航被确认。
 10. 调用全局的 afterEach 钩子。
 11. 触发 DOM 更新。
 12. 用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数。