---
title: vue-补充
date: 2017-06-20 20:00:12
tags: vue
---
### 1、关于this
#### 1)第一种情况
```
<script>
    import Hello from './components/Hello'
    export default {
      data () {
        return {
          list: ['a', 'b', 'c'],
          idx: 0,
          current: ''
        }
      },
      methods: {
        iter () {
          this.list.map(function (v, k) {
            if (k === this.idx) {
              this.current = v
    
              console.log(this.current)
            }
          })
        }
      },
      components: {
        Hello
      }
    }
</script>
```
在map里的this是指向当前map的迭代对象，而非我们vue的实例，this里没有我们需要的idx。解决方式有两种；
其一是通过保存this	
```
let _this = this
```
其二是使用es6箭头函数
```
methods: {
    iter () {
      this.list.map((v, k) => {
        if (k === this.idx) {
          this.current = v

          console.log(this.current)
        }
      })
    }
  },
```
#### 2)第二种情况

```
<div @click="check"></div>
```
```
methods: {
    check () {
        alert('ok')
    }
}
```
正确写法：
```
methods: {
    check () {
        window.alert('ok')
    }
}
```
### 2、方法传值

我们在input中的方法希望获取input的value，可以通过$event这个对象，通过将$event传入方法,我们可以成功的拿到我们希望的值
```
<input type="text" value="value" @input="change($event)"/>
```
```
change (e) {
  console.log(e.target.value)
  this.value = e.target.value
}
```
### 3、迭代判断

比如我们有一个列表，我们希望能显示我们当前选中的那一个，基本思路是通过$index来判断是否是当前迭代对象，然后去增减class或者style来实现不同的样式
```
<ul>
  <!-- 方法1 class-->
  <li v-for="item in list" :class="{'active': $index === activeId}">{{item}}</li>
  
  <!-- 方法2 style-->
  <li v-for="item in list" :style="{backgroundColor: $index === activeId ? 'red' : 'white'}">{{item}}</li>
</ul>

data () {
  return {
    list: ['a', 'b', 'c'],
    activeId: 0
  }
}
```
### 4、渲染
点击菜单一个组件加载出来表格列表,输入查询条件查询,当在单击这个菜单后表格的数据没有重置查询条件和查询结果.

Vue路由在页面渲染一个组件后加载后,再加载这个组件,组件不会摧毁后在重新生成这个组件,不会重新触发组件的生命周期中的方法；在开发中这个问题在两个菜单共用一个组件,设置传参来判断加载不同的数据的情况下,会出现另一个ready方法不走导致数据显示不真确.解决思路可以加监听路由地址触发ready事件.

而上面的解决方法是用v-if来重新加载组件。
### 5、多页面
多页面方式一样也可以使用vue强大的组件系统和脚手架。在github.com搜索“vue2 multipage”,会得到不少多页面脚手架
### 6、elemeui

### 7、异常1
用vue做项目的时候，抛出异常:
```
DOMException: Failed to execute 'insertBefore' on 'Node': The node before which the new node is to be inserted is not a child of this node.
```
和v-if和v-show有关系；对需要渲染的模板外层添加<div v-if="isShow"><div v-for=""></div></div>,点击按钮开始请求数据的时候@click="isShow=false", 当成功请求数据时,在回调函数中cb(isShow=ture). 保证 更新数据的时候先移除后插入    

### 8、存储问题
localstorage存储满了；

### 9、传值问题
页面之间传值，父子组件之间的传值；

### 10、跨域问题
跨域保证数据安全

### 11、移动设备兼容问题
2X和3X
安卓机兼容问题

### 12、多页面vue搭建

### 13、seo优化

[参考](http://cnodejs.org/topic/5750d752491b9c4f36910fec)

vue2 multipage