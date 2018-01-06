遇到了一个业务场景：

>某个按钮按下去之前需要先判断它是否登陆，如果没有登陆需要跳转到对应的登陆页面，否则就继续该按钮之后的操作。

对于这种问题，很显然不能每个按钮都去判断，所以我思考了一下结合自定义指令和vuex完成了相应的实现。
**主要的代码实现**
```
const directive = Vue.directive('permission-click', {
  bind: (el, binding, vnode) => {
    el.addEventListener('click', (e) => {
      if (!store.getters.isLogin) {
        store.dispatch('showLogin')
      } else {
        typeof binding.value === 'function' && binding.value()
      }
    })
  }
})
```
这里封装了一个自定义指令，添加了一个点击事件，对于已经登陆的则调用传进来的函数，否则通过vuex去控制登陆（此处的登陆是通过弹窗实现的）
自定义组件使用的时候也极为简单
```
<div class="" v-permission-click="doSomething">
  ...
</div>
```
vuex里面的showLogin这个action无非就是对login的显示隐藏flag的操作。
这里只是完成了简单的登陆权限控制，从登陆权限出发，可以加入更多的权限控制，比如根据role角色判断，然后可以全局地控制权限，且实现起来极为精简。
>Github: https://github.com/lyh2668
>Authby: lyh2668