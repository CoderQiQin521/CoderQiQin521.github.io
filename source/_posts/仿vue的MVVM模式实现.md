---
title: 仿vue的MVVM模式实现
date: 2020-03-28 11:25:35
tags: [vue,mvvm]
categories: web
---
> demo在线地址: [https://codesandbox.io/embed/like-vue-qs42n?fontsize=14&hidenavigation=1&theme=dark](https://codesandbox.io/embed/like-vue-qs42n?fontsize=14&hidenavigation=1&theme=dark)

![https://ae01.alicdn.com/kf/U299bf4c7980d42e39e325ac4c72c766d2.jpg](https://ae01.alicdn.com/kf/U299bf4c7980d42e39e325ac4c72c766d2.jpg)

## 分析
```
let vm = new Vue({
        el: "#app",
        data: {
          msg: " 改变",
          obj: {
            title: "<strong>仿写vue</strong>",
            o: {
              name: "asdasdas"
            }
          }
        },
        methods: {
          handle() {
            console.log(this);
            alert(123);
            this.msg = "shijiangaibiancanshu1";
          }
        }
      });
```

调用vue就是实例化一个`Vue`类的过程, vue的各个api继承自`Vue`类,option通过参数传入, 由于es5代码会比较啰嗦,采用es6方式, el是作用域的根节点,可以传入选择器或者`element`节点, data是响应的数据也是比较核心的部分,
可以看到首先判断元素绑定是否成功,除去数据代理先不讲,程序主要执行两个步骤,分别处理模版和数据:
1. new Observer: 将整个数据data传入
2. new Compile: 将根元素el传入

```
class Vue {
  constructor(options) {
    this.el = options.el;
    this.data = options.data;
    this.methods = options.methods;
    if (this.el) {
      // 数据劫持
      new Observer(this.data);
      // 数据代理
      this.proxy(this.data, this);
      // 模版编译
      new Compile(this.el, this);
    }
  }

  proxy(data, vm) { // 关于数据代理最后再讲
    // this.data.msg  <=> this.msg
    Object.keys(data).forEach(key => {
      Object.defineProperty(vm, key, {
        get() {
          return data[key];
        },
        set(newVal) {
          data[key] = newVal;
        }
      });
    });
  }
}
```

一个最基本的`MVVM`框架需要实现这几个模块: 数据劫持(observer), 模版编译(compile), 依赖收集(dep), 发布订阅(watcher,又称观察者), 代码都是比较常用的那类,难点在于设计模式的精巧和抽象,下面的图片,需要好好的理解


![https://ae01.alicdn.com/kf/Ue802cf0ff7ed4304b9d3f9fe09858113p.jpg](https://ae01.alicdn.com/kf/Ue802cf0ff7ed4304b9d3f9fe09858113p.jpg)

右边部分我的理解大致是这样的: 在实例化时将数据传入类的内部,检测传参是否为对象,遍历传参对象的属性并递归嵌套对象,然后处理数据,关键api是es6的`Object.defineProperty`[文档参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty),简单理解就是自定义对象属性,并通过getter和setter这个钩子进行我们的数据观察,(大喊一声:这里先占一个坑!)
在每一次遍历数据,劫持前,实例化一个依赖收集器, 该收集器具有添加观察者(watcher)存储,和通知更新两个方法,
在劫持数据的getter时收集器执行添加watcher, 在数据发生setter时,收集器通知更新

## watcher
watcher类主要功能接收一个回调,和获取对象数据的新旧值比对,在发生改变的时候,触发回调的执行

## 模版编译compile
模版编译是将根元素下的所有节点转换为文档片段`createDocumentFragment`提高性能,再插入根节点渲染,遍历文档片段下的每一个节点的nodeTye,区分是元素节点(如果是元素节点可能存在嵌套,递归处理)还是文本节点,拆分两种处理函数..
源码: 
```
class Compile {
  constructor(el, vm) {
    this.el = this.isElement(el) ? el : document.querySelector(el);
    this.vm = vm;
    let fragment = this.node2Fragment(this.el);
    this.compile(fragment);
    this.el.appendChild(fragment);
  }

  isElement(el) {
    return el.nodeType === 1;
  }

  node2Fragment(el) {
    let f = document.createDocumentFragment();
    let firstChild;

    while ((firstChild = el.firstChild)) {
      f.appendChild(firstChild);
    }
    return f;
  }

  compile(fragment) {
    let nodes = fragment.childNodes;
    [...nodes].forEach(node => {
      if (this.isElement(node)) {
        this.compileElement(node);
        this.compile(node);
      } else {
        this.compileText(node);
      }
    });
  }

  compileElement(node) {
    let attributes = node.attributes;
    [...attributes].forEach(attr => {
      let { name, value } = attr;
      if (this.isDirective(name)) {
        let [, directive] = name.split("-");
        let [dirName, eventName] = directive.split(":");
        // console.log(name, value, directive);
        compileUtil[dirName](node, value, this.vm, eventName);
        node.removeAttribute(`v-${directive}`);
      } else if (this.isEvent(name)) {
        let [, eventName] = name.split(":");
        compileUtil["on"](node, value, this.vm, eventName);
      }
    });
  }

  compileText(node) {
    let textContent = node.textContent;
    if (/\{\{(.+?)\}\}/.test(textContent)) {
      compileUtil["text"](node, textContent, this.vm);
    }
  }

  isDirective(name) {
    return name.startsWith("v-");
  }

  isEvent(name) {
    return name.startsWith("@");
  }
}
```
原创手打,留言支持一下吧~