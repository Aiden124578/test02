# 配置Vue

Vue是一个渐进式的框架(声明式编程)，传统的为命令式编程，模板和页面显示的数据基本一致称为声明式编程

- JavaScript框架
- 简化Dom操作
- 响应式数据驱动



## 	Vue的生命周期

- beforeCreate，表示实例被完全创建出来之前，会执行它，data和methods中的数据都还没有初始化

- created是组件被创建时回调，data和methods都已经被初始化，例：改标题，每个组件页面都调用created方法

```javascript
created(){
	document.title="首页"
}
```

- beforMount表示模板已经在内存中编译完成了，但是尚未把模板渲染到页面中，即数据未能渲染
- mouted是template挂载到DOM时回调，表示内存中的模板已经真实的挂载到了页面中，用户已经可以看到渲染好的页面了，实例创建期间的最后一个生命周期函数，此时，如果没有其他操作的话，这个实例就躺在我们的内存中，一动不动，操作页面的DOM节点时调用此函数，用来拿后台接口数据
- 运行阶段，beforeUpdate，updated当data中数据改变时两个才会触发，执行0次或多次
  - beforeUpdate页面上的数据是旧的，data中的数据是最新的
  - updated是数据发生改变时回调，页面中的数据和data中的数据都是最新的

- 销毁阶段，需要手动销毁（this.$destroy()）
  - 执行beforDestroy，实例上的所有data和所有的methods，以及过滤器等都处于可用状态
  - 执行destroyed，实例上的所有data和所有的methods，以及过滤器等都已经不可用了



## Vue组件化思想

组件化规范：Web Components

- 我们希望尽可能多的重用代码
- 自定义组件的方式不太容易（html、css、js）
- 多次使用组件可能导致冲突

解决：WebComponents通过创建封装好功能的定制元素解决上述问题

![image-20200811173657426](images/image-20200811173657426.png)



## 注册组件

- 组件不可以访问Vue实例数据

- 全局组件步骤,Vue.component
  - 创建组件构造器
  - 注册组件
  - 使用组件,放到vue的实例中使用

![image-20200811174803873](images/image-20200811174803873.png)

- 局部组件，在vue中注册组件，只能id为app的使用

![image-20200812100602428](images/image-20200812100602428.png)

- vue实例为根组件

- 父组件，第二个组件构造器
- 子组件，第一个组件构造器，在父组件中注册，只能在父组件中使用

![image-20200812101110648](images/image-20200812101110648.png)

![image-20200812101502217](images/image-20200812101502217.png)

![image-20200812101124068](images/image-20200812101124068.png)

- 全局组件注册的语法糖

![image-20200812102101114](images/image-20200812102101114.png)

- 局部组件的语法糖

![image-20200812102226248](images/image-20200812102226248.png)



## 模板分离写法

- 组件模板template

- 第一个种：script标签，类型必须是text/x-template，通过id来获取

![](images/image-20200812103222817.png)

![image-20200812102843464](images/image-20200812102843464.png)

第二种：template标签，id来获取

![image-20200812103103491](images/image-20200812103103491.png)



## 组件中的data

- data必须是函数，data返回的是一个对象
- 使用时必须放在一个div里面使用
- data必须是函数的原因，每次返回的都是不同的对象，即使相同也不会相互影响,每一个组件实例都有自己的一个对象，用于保存自己的数据，保持自己的状态

![image-20200812104024270](images/image-20200812104024270.png)



## 父组件的通信

- props向子组件传递数据
- 在props中使用驼峰形式，普通模板中需要使用短横线的形式
- 字符串形式模板中没有限制
- v-bind接收数据
- 注意：模板都不能写驼峰，props的驼峰标，v-bind不支持驼峰标识，v-on也是，全部小写，如果有驼峰的地方用-表示，后面接小写，cInfo->c-info

![](images/image-20200812114118501.png)

- 通过数组传递

![image-20200812140525793](images/image-20200812140525793.png)

![](images/image-20200812140447449.png)

- 通过对象传递

  - type类型
  - default默认值
  - required必须传

  ![](images/image-20200812142254460.png)



## 子组件向父通信

自定义事件emit，通过this.$emit('名字',参数);传递

```html
<!-- 父组件模板 -->
<div id="app">
    <cpn @itemclick="cpnClick"></cpn>
</div>
<!-- 子组件模板 -->
<template id="cpn">
    <div>
        <button @click="cpnClick(item)" v-for="item in categaries">{{item.name}}</button>
    </div>
</template>
```

```javascript
// 子组件
    const cpn = {
            template: '#cpn',
            data() {
                return {
                    categaries: [{
                        id: 1,
                        name: '热门家电'
                    }, {
                        id: 1,
                        name: '家用电器'
                    }, ]
                }
            },
            methods: {
                cpnClick(item) {
                    this.$emit('itemclick', item)
                }
            },
        }
        // 父组件
    const app = new Vue({
        el: '#app',
        data: {
            movies: ['海王', '航海王', '海贼王'],
            message: '你好啊'
        },
        components: {
            cpn
        },
        methods: {
            cpnClick(item) {
                console.log(item);
            }
        }
    })
```



## 父子组件通信结合双向绑定

- 父向子组件通信，在通过input的v-model本质原理:value和@input来动态的改变data中的数据
- 需要改变的数据都用data来存，改变data中的数据，不要改变props中的数据
- number1，number2接收父组件num1，num2数据
- 方法一

![image-20200812170419840](images/image-20200812170419840.png)

![image-20200812170437715](images/image-20200812170437715.png)

![](images/image-20200812170513362.png)

- 方法二：v-model结合watch

![image-20200812172857131](images/image-20200812172857131.png)

![image-20200812172930616](images/image-20200812172930616.png)



## 父组件访问子

通过属性来传，然后通过props

$children/$ref

![image-20200812173251358](images/image-20200812173251358.png)

- $children来访问子组件的属性和方法

```html
 <!-- 父组件模板 -->
    <div id="app">
        <cpn></cpn>
        <cpn></cpn>
        <cpn></cpn>
        <button @click="btnClick">按钮</button>
    </div>
    <!-- 子组件模板 -->
    <template id="cpn">
    <div>
        我是子组件
    </div>
</template>
```

```javascript
const app = new Vue({
        el: '#app',
        data: {
            message: "你好啊"
        },
        methods: {
            btnClick() {
                console.log(this.$children);
                for (let c of this.$children) {
                    console.log(c.name);
                    c.showMessage();
                }

            }
        },
        components: {
            cpn: {
                template: '#cpn',
                data() {
                    return {
                        name: '我是子组件的name'
                    }
                },
                methods: {
                    showMessage() {
                        console.log('showMessage');
                    }
                },
            }
        }
    })
```

- $refs来访问子组件的属性和方法(常用)

  - ref去设置属性
  - this.$refs来访问

  ```html
  <!-- 父组件模板 -->
      <div id="app">
          <cpn></cpn>
          <cpn ref="aaa"></cpn>
          <cpn></cpn>
          <button @click="btnClick">按钮</button>
      </div>
      <!-- 子组件模板 -->
      <template id="cpn">
      <div>
          我是子组件
      </div>
  </template>
  ```

  ```javascriptja
  const app = new Vue({
          el: '#app',
          data: {
              message: "你好啊"
          },
          methods: {
              btnClick() {
                  // console.log(this.$children);
                  // for (let c of this.$children) {
                  //     console.log(c.name);
                  //     c.showMessage();
                  // }
                  console.log(this.$refs.aaa.name);
              }
          },
          components: {
              cpn: {
                  template: '#cpn',
                  data() {
                      return {
                          name: '我是子组件的name'
                      }
                  },
                  methods: {
                      showMessage() {
                          console.log('showMessage');
                      }
                  },
              }
          }
      })
  ```



## 子访问父组件

通过子组件$emit自定义点击事件，父组件也点击事件相对于的名字

$parent/$root

$parent

```html
<!-- 父组件模板 -->
    <div id="app">
        <cpn></cpn>
    </div>
    <!-- 子组件模板 -->
    <template id="cpn">
    <div>
        <h2>我是cpn组件</h2>
        <ccpn></ccpn>
    </div>
</template>
    <template id="ccpn">
    <div>
        <h2>我是子组件</h2>
        <button @click="btnClick">按钮</button>
    </div>
</template>
```

```javascript
const app = new Vue({
        el: '#app',
        data: {
            message: "你好啊"
        },
        components: {
            cpn: {
                template: '#cpn',
                data() {
                    return {
                        name: '我是cpn组件的name'
                    }
                },
                components: {
                    ccpn: {
                        template: '#ccpn',
                        methods: {
                            btnClick() {
                                console.log(this.$parent);
                                console.log(this.$parent.name);
                                //访问根组件
                                console.log(this.$root.message);
                            }
                        },
                    }
                }
            }
        }
    })
```

![image-20201128104229679](images/image-20201128104229679.png)

带参数的传递，通过$event接收

![image-20201128104714415](images/image-20201128104714415.png)



## 组件插槽

![image-20201128130441674](images/image-20201128130441674.png)

![image-20201128130602026](images/image-20201128130602026.png)

![image-20201128131638983](images/image-20201128131638983.png)



## el

挂载点

- 可以挂载任何标签，最好是div
- 可以用class，id标签选择器挂载，最好用id
- 挂载的元素，子元素可以使用data中的数据



## MVVM准则

MVVM是前端视图层的分层开发思想，主要把每个页面分成了M、V和VM，其中VM是MVVM思想的核心，因为VM是M和V直接的调度者。

- View(HTML+CSS)界面，指页面中的元素和样式，一般指HTML+CSS
- Model(data中的数据)模型，指程序中创建的或从服务端获取的数据，一般用JS中的一个对象来保持。数据内容会显示到界面View中，即data
- 
  VueModel(Vue实例)试图模型，替代之前手写的DOM/JQUERY操作，把模型中的数据和界面中的HTML元素“绑定在一起”，数据双向绑定



## Vue的使用

创建vue实例
el对象（id）#名字
data数据
methods方法



## method/function

method类中的方法，function为函数



## this

this表当前对象，可以获取到data中的数据



## v-text

- v-text替换全部文字,并会覆盖标签内的文字(abc)
- 解决网速慢插值表达式闪烁的问题

```html
<h2 v-text="message">abc</h2>
```



## v-html

v-html包括标签，有标签时用

```html
<h2 v-html="url"></h2>
```



## v-on

- v-on:click=“do” 


- 语法糖：简写v-on:click等于@click
- @input输入即触发
- @change输入框失去焦点时


- 事件修饰符，可多个修饰符并用@click.prevent.once


  - @keyup.enter点击回车键
  - @click.stop阻止事件冒泡

  ![image-20200808151023322](images/image-20200808151023322.png)

  - @click.prevent阻止默认事件，阻止表单自动跳转

  ![image-20200811163835906](images/image-20200811163835906.png)

  - @keyup.enter监听键盘的按下事件
  - keyup.delete监听键盘删除事件
  - @click.once只触发一次回调
  - @click.prevent阻止默认事件
  - @click.capture实现捕获触发机制
  - @click.self只有点击当前元素的时候，才会触发事件处理函数
- 事件：@play播放，@pause暂停
- 事件监听，函数没有参数，则可以省略括号
- 函数需要参数，但是没有传，那么函数的形参为undefined
- 在事件定义时，写函数时省略了小括号，但方法本身是需要一个参数的，这个时候，vue会默认将浏览器生产的event事件对象作为参数传入到方法
- 调用方法时，如何手动的获取到浏览器参数的event对象：$event

![image-20200807150957469](images/image-20200807150957469.png)



## v-show,v-if

v-show显示，本质是切换元素的display，设置函数对属性进行取反，用于平凡切换，有较高的初始渲染消耗
v-if是否切换显示的状态，本质是移除dom元素，有较高的切换性能消耗

![image-20200807141038485](images/image-20200807141038485.png)



## v-bind

设置元素的属性（src，title，class）

1. 动态的给属性绑定值
2. v-bind:src=“imgSrc”设置属性
3. 语法糖：简写:src=“”
4. 动态绑定class对象语法
5. 动态绑定的true和false为boolean值

```css
.active{
	color: red;
}
```

```html
<div id="app-8">
    <h2 :class="{active:isActive,line:isActive}">{{message}}</h2>
    <button @click="btn">按钮</button>
</div>
```

```javascript
var app = new Vue({
    el:'#app-8',
    data:{
            message:'你好啊',
            isActive:true,
            isLine:true
        },
    methods:{
        btn:function(){
            this.isActive=!this.isActive
        }
    }
 })
```

对象的方式：class=“｛active：isActive｝”，下面的更简单

![image-20200807142609207](images/image-20200807142609207.png)



## v-for

key的作用主要是为了高效的更新虚拟DOM

v-for="item in arr"

v-for循环遍历
v-for=“（it，index）in arr”
@keyup.enter事件绑定修饰符
v-model双向绑定 ，需于表单元素一起使用

![image-20200807144644650](images/image-20200807144644650.png)

遍历对象时

![image-20200811114420144](images/image-20200811114420144.png)

![image-20200811114516655](images/image-20200811114516655.png)

key提高性能，只能使用number和string类型，使用时必须v-bind绑定

![image-20200811115701823](images/image-20200811115701823.png)

遍历数字，循环10次，count值从1开始

![image-20200915100735709](images/image-20200915100735709.png)



## v-once

- 控制台改变值时，它不会改变，不做响应式
- 只做一次响应

```html
<h2>{{message}}</h2>

<h2 v-once>{{message}}</h2>
```



## v-pre

把标签内的内容原封不动的显示出来，不解析，显示{{message}}

```html
<h2>{{message}}</h2>
<h2 v-pre>{{message}}</h2>
```



## v-cloak

特殊情况下的js代码解析不出来时，html会原封不动的解析到浏览器,加入v-cloak属性则显示页面空白效果

原理：一开始属性为none，当解析到vue时，vue会删除掉v-cloak属性

```html
<div id="app-8" v-cloak>
        <h2>{{message}}</h2>
</div>
```

```css
[v-cloak]{
            display: none;
}
```



## v-model

- 表单域修饰符
  - number转化为数值，v-model.number
  - trim去掉开始和结尾的空格
  - lazy将input事件切换为change事件

获取和设置input表单元素的值（双向数据绑定），textare也可以双向绑定

![image-20200807151942685](images/image-20200807151942685.png)

双向绑定原理v-model相当于v-bind:value和v-on:input的结合，表单里面的value都是字符串类型

![image-20200811163256782](images/image-20200811163256782.png)

与radio结合使用，默认选中

![image-20200811165126792](images/image-20200811165126792.png)

checkbox单选框，同意协议，下一步

![image-20200811165647601](images/image-20200811165647601.png)

checkbox多选框

![image-20200811170001429](images/image-20200811170001429.png)

select列表，选择一个

![](images/image-20200811170506344.png)

select列表，选择多个，select加mutiple属性

![image-20200811170622791](images/image-20200811170622791.png)

值绑定，动态的绑定用v-bind

![image-20200811171243118](images/image-20200811171243118.png)

修饰符

- v-model.lazy(失去焦点，敲回车时更新)

![image-20200811171853275](images/image-20200811171853275.png)

v-model.number数字类型，因为v-model默认的是String类型

![image-20200811172312636](images/image-20200811172312636.png)

v-model.trim去除空格

![image-20200811172500962](images/image-20200811172500962.png)



## 自定义指令

- 自动默认的焦点事件，`v-focus`

```javascript
Vue.directive('focus',{
	inserted:function(el){
		el.focus();
	}
})
```

- 带参数的自定义指令（改变元素背景色）

```javascript
Vue.directive('color',{
	inserted:function(el){
		el.style.backgroundColor = binding.value.color
	}
})
```

- 指令用法

```html
<input type="text" v-color="{color:'orange'}"/>
```



## splice删除

第一个参数为索引，第二个参数为删除几个

![image-20200807153121375](images/image-20200807153121375.png)



## event

![image-20201122204238033](images/image-20201122204238033.png)

- 如果事件直接绑定函数名称，则默认会传递事件对象作为事件函数的第一个参数
- 如果事件绑定函数调用（加了括号），那么事件对象必须作为最后一个参数显示传递，并且事件对象名称必须是$event



## axios网络封装

Ajax库，先导入后使用

get请求

![image-20200807154528321](images/image-20200807154528321.png)

![image-20200807161040210](images/image-20200807161040210.png)

post请求

![image-20200807154650114](images/image-20200807154650114.png)

![image-20200807161110471](images/image-20200807161110471.png)



## computed

- 是个对象

- 基于data中的数据进行处理，表达式的计算逻辑比较复杂时使用，需要return
- 使用时直接{{函数名}}
- 使模板内容更加简洁

- 计算属性，只调用一次，调用时不用加括号，性能比methods内的方法高，有缓存效果


- 本质是实现函数的get方法,直接使用fullName(){}


![image-20200808112236824](images/image-20200808112236824.png)



## 计算属性与方法的区别

- 计算属性是基于它们的依赖进行缓存的，提高性能，使用时不用加括号
- 方法不存在缓存，方法中的函数使用时要加括号



## 侦听器

- 数据变化执行异步（Ajax、定时器）或开销较大（耗时）的操作
- val为改变的数据
- data属性名与watch中函数名要相同

![image-20201123162823246](images/image-20201123162823246.png)

模拟用户注册

![image-20201123163922052](images/image-20201123163922052.png)



## 过滤器

- 格式化数据,需要return
- 参数val，为输入的数据
- 参数arg为第二个参数,接收format的参数格式

![image-20201123165237387](images/image-20201123165237387.png)

![image-20201123231410911](images/image-20201123231410911.png)

时间过滤器

```javascript
<div id="app">
    {{data | format('yyyy-MM-dd')}}
  </div>
  <script>
    Vue.filter('format',function(value,arg){
      if( arg === 'yyyy-MM-dd' ){
        var ret = ''
        ret += value.getFullYear() + '-' + ( value.getMonth() + 1 ) + '-' + value.getDate()
        return ret
      }
      return value
    })
    var vue = new Vue({
      el: '#app',
      data: {
        data:new Date()
      },
      methods: {}
    });
  </script>
```



## Vue.set

动态处理响应式

![image-20201123235952805](images/image-20201123235952805.png)

![image-20201123235911342](images/image-20201123235911342.png)



## input复用问题

vue内部存在虚拟Dom，key解决复用问题

![image-20200811112753075](images/image-20200811112753075.png)



## filter

过滤

```javascript
this.books.filter(function(item){
 return item.id == id 
})
```



## push

数组中最后面添加一个元素



## split

- 'abc'.split('')则变为['a','b','c']
- 'abc'.split(',')则变成数组['abc']



## slice

val='abcd'

val.slice(1)从第二个元素到后面'bcd'



## charAt

a.charAt(0)拿到一个字符串的第一个字母



## pop

删除数组中的最后一个元素



## shift

删除数组中的第一个元素



## unshift

在数组的最前面添加元素



## splice

- 索引从0开始，不删除第二个参数写0，可写两个参数

- 第二个参数为0即为插入

- splice（第几个元素开始，删除几个元素，替换插入加的元素）
- 删除自己

![image-20200811151821191](images/image-20200811151821191.png)



## sort

排序



## reverse

反序



## set

![image-20200811142042720](images/image-20200811142042720.png)



## 点击变色事件

![image-20200811142816654](images/image-20200811142816654.png)

![image-20200811142750238](images/image-20200811142750238.png)



## toFixed

保留小数点，toFixed（2）保留两位小数

![image-20200811150528484](images/image-20200811150528484.png)



## filters过滤器

![image-20200811150842761](images/image-20200811150842761.png)

![image-20200811150823646](images/image-20200811150823646.png)



## button

disabled属性，按钮不能点击



## slot

组件的插槽

- 组件的插槽也是为了让我们封装的组件更加具有扩展性
- 让使用者可以决定组件内容的一些内容到底展示什么

```html
<div id="app">
        <cpn>
            <button>按钮</button>
        </cpn>
        <cpn><span>嘿嘿嘿</span></cpn>
        <cpn><i>呵呵呵</i></cpn>
        <cpn><button>按钮</button></cpn>
    </div>
    <template id="cpn">
        <div>
            <h2>我是组件</h2>
            <p>我是组件哈哈哈</p>
            <slot></slot>
        </div>
    </template>
```

```javascript
const app = new Vue({
        el: '#app',
        components: {
            cpn: {
                template: '#cpn'
            }
        }
    })
```

- 插槽的基本使用
  - <slot></slot>
  - 插槽的默认值<slot>button</slot>
  - 如果有多个值，同时放入到组件进行替换时，一起作为替换元素

![image-20200813094147479](images/image-20200813094147479.png)

- 具名插槽slot,通过指定name，使用时slot来确定name

```html
<div id="app">
        <cpn><span slot="center">标题</span></cpn>
    </div>
    <template id="cpn">
        <div>
            <slot name="left"><span>左边</span></slot>
            <slot name="center"><span>中间</span></slot>
            <slot name="right"><span>右边</span></slot>
        </div>
    </template>
</body>
<script src="vue-2.6.11/dist/vue.min.js"></script>
<script>
    const app = new Vue({
        el: '#app',
        components: {
            cpn: {
                template: '#cpn'
            }
        }
    })
</script>
```



## 编译的作用域

- 模板的变量的作用域为vue实例里面的，v-show和v-for在vue的data中的

![image-20200813100334429](images/image-20200813100334429.png)

- 组件的作用域为组件里面的，v-show在组件data返回函数中的

![image-20200813100402213](images/image-20200813100402213.png)

- 子组件的插槽传递数据给父组件
  - 子组件插槽设置:data="变量名",data是随便取的
  - 传递给父组件，父组件添加一个template模板，通过slot-scope="slot"接收数据
  - 父组件使用数据，用item in slot.data直接渲染

  ```html
  <div id="app">
          <cpn></cpn>
          <cpn>
              <template slot-scope="slot">
                  <span v-for="item in slot.data">{{item}}</span>
              </template>
          </cpn>
      </div>
      <template id="cpn">
          <div>
              <slot :data="arr">
                  <ul>
                      <li v-for="item in arr">{{item}}</li>
                  </ul>
              </slot>
          </div>
      </template>
  </body>
  <script src="vue-2.6.11/dist/vue.min.js"></script>
  <script>
      const app = new Vue({
          el: '#app',
          components: {
              cpn: {
                  template: '#cpn',
                  data() {
                      return {
                          arr: ['C#', 'JavaScript', 'Java']
                      }
                  },
              }
          }
      })
  ```




# Vue Router

## 基本使用

![image-20201206195751996](images/image-20201206195751996.png)

![image-20201206195804456](images/image-20201206195804456.png)

![image-20201206195901169](images/image-20201206195901169.png)

![image-20201206195838274](images/image-20201206195838274.png)

![image-20201206200244937](images/image-20201206200244937.png)

## 动态匹配路由

![image-20201206203518574](images/image-20201206203518574.png)

## 路由传参

![image-20201206203728041](images/image-20201206203728041.png)

![image-20201206204222163](images/image-20201206204222163.png)

route接受动态的参数

![image-20201206204504314](images/image-20201206204504314.png)

## 命名路由

![image-20201206205157341](images/image-20201206205157341.png)

## 编程式导航

- 声明式导航：点击链接实现导航的方式。比如网页中的a链接
- 编程式导航：通过调用JavaScript形式的API实现导航的方式，比如网页中的location.href
  - push跳转页面
  - go为1前进一个页面，-1为后退

![image-20201206210424585](images/image-20201206210424585.png)

![image-20201206211213518](images/image-20201206211213518.png)



# 模块化

![image-20201206221938126](images/image-20201206221938126.png)

## 浏览器端模块化规范

- AMD    `Require.js`
- CMD   `Sea.js`

## 服务器端模块化规范

- CommonJS（node.js）
  - 模块分为单文件模块与包
  - 模块成员导出：module.exports和exports
  - 模块成员导入：require('模块标识符')

## ES6模块化

ES6语法规范中，在语言层面上定义了ES6模块化规范，是浏览器于服务器端通用的模块化开发规范

- AMD和CMD适用于浏览器端的Javascript模块化
- CommonJS适用于服务器端的Javascript模块化
- 每个js文件都是一个独立的模块
- 导入模块成员适用import关键字
- 暴露模块成员适用export关键字

#### 默认导出与默认导入

注意：每个模块中，只允许使用唯一的一次export defaul，否则会报错

![image-20201207140748953](images/image-20201207140748953.png)

### 按需导出与按需导入

- 按需导出和默认导出可以同时存在
- 按需导入时名称必须与导出的名称一致
- 注意：在每个模块可以使用多个按需导出

![image-20201207143145523](images/image-20201207143145523.png)

### 直接导入并执行模块代码

![image-20201207143816529](images/image-20201207143816529.png)



# babel

## 在Node.js中通过babel体验ES6模块化

- cnpm install --save-dev @babel/core @babel/cli @babel/preset-env @babel/node
- cnpm i -S @babel/polyfill

```javascript
const presets = [
  [
    "@babel/env", {
      targets: {
        edge: "17",
        firefox: "60",
        chrome: "67",
        safari: "11.1"
      }
    }
  ]
]
module.exports = {presets}
```

![image-20201206223339120](images/image-20201206223339120.png)



# Webpack

- 文件依赖关系错综复杂
- 静态资源请求效率低
- 模块化支持不友好
- 浏览器对高级Javascript特性兼容性程度较低

## 概述

- webpack是一个流行的前端项目构建工具（打包工具），可以解决当前web开发中所面临的困境
- 提供了友好的模块化支持，以及代码压缩混淆、处理js兼容问题、性能优化等强大的功能，从而让程序员把工作的重心放到工具的功能实现上，提高了开发效率和项目的可维护性

## 基本使用

解决JavaScript的高级语法（es6）浏览器的兼容问题

![image-20201207144955356](images/image-20201207144955356.png)

- cnpm i webpack webpack-cli -D
- webpack.config.js配置

```javascript
module.exports = {
  //编译模式
  mode:'development' //development  production
}
```

![image-20201207145914212](images/image-20201207145914212.png)



### 配置打包的入口与出口

![image-20201207151229414](images/image-20201207151229414.png)

### 配置自动打包功能

- webpack-cli为3.x版本
- bundle.js被存在根目录中隐藏的，被存在了内存中
- 不修改，则打包出口为则为/main.js
- 配置dev ：webpack-dev-server --open --port xxx  --host  xxx

![image-20201207152749777](images/image-20201207152749777.png)

### 配置html-webpack-plugin插件

- 生成预览页面
- 这个插件会自己在index.html的添加script的js文件，即不用引入js
- cnpm i html-webpack-plugin -D

![image-20201208142115087](images/image-20201208142115087.png)

### 配置自动打包相关参数

![image-20201208143137818](images/image-20201208143137818.png)

## loader加载器

打包非js模块，webpack默认只能打包处理以.js后缀名的模块，其他非.js后缀名结尾的模块，webpack默认处理不了，需要动用loader加载器才可以正常打包，否则会报错！

![image-20201208143813966](images/image-20201208143813966.png)

### 打包处理css文件

- cnpm i style-loader css-loader -D
- use数组指定的loader顺序是固定的
- 多个loader的调用顺序是：从后往前调用

![image-20201208144323999](images/image-20201208144323999.png)

### 打包处理less文件

- cnpmi less-loader less -D

![image-20201208145223202](images/image-20201208145223202.png)

### 打包处理scss文件

- cnpm i sass-loader node-sass -D

![image-20201208145914225](images/image-20201208145914225.png)

### 配置postCSS自动添加css的兼容前缀

- cnpm i postcss-loader autoprefixer -D

![image-20201208145832345](images/image-20201208145832345.png)

### 打包样式表中的图片和字体文件

-  cnpm i url-loader file-loader -D
- base64使图片在加载期间速度更快
  - 当图片的大小大于limit的值时，会变成图片地址
  - 当小于等于limit时会变成base64地址

![image-20201208152535130](images/image-20201208152535130.png)

### 打包处理js文件中的高级语法

- 如class
- cnpm i babel-loader @babel/core @babel/runtime -D
- cnpm i @babel/preset-env @babel/plugin-transform-runtime @babel/plugin-proposal-class-properties -D

![image-20201208153813902](images/image-20201208153813902.png)

### 配置vue组件的加载器

- cnpm i vue-loader vue-template-compiler -D

![image-20201208160036183](images/image-20201208160036183.png)

### 在webpack项目中使用vue

![image-20201208160715424](images/image-20201208160715424.png)



# Vue单文件组件

## 基本用法

scoped防止各组件样式冲突

![image-20201208155530608](images/image-20201208155530608.png)



# Vue-cli脚手架

![image-20201208165925127](images/image-20201208165925127.png)

- vue的可视化工具安装

- 下载国内的cnpm安装
  
- npm install -g cnpm --registry=https://registry.npm.taobao.org
  
- 安装vue-cli

  - cnpm install -g @vue/cli 

- 运行vue ui打开可视化工具

- 配置文件

  - 插件

  ![image-20200811153102028](images/image-20200811153102028.png)

  - 依赖

  ![image-20200811153122120](images/image-20200811153122120.png)

  ![image-20200811153134901](images/image-20200811153134901.png)

  - 点击任务serve->运行->启动app



## Element-ui

![image-20201208170801497](images/image-20201208170801497.png)

![image-20201208171211710](images/image-20201208171211710.png)



## Runtime-Compiler和Runtime-only

![image-20200822093114803](images/image-20200822093114803.png)

render替换html里面挂载的app，$mount相当于el挂载点

![image-20200822100546012](images/image-20200822100546012.png)



## 父子传递

子组件,创建子组件文件夹和子组件为exchange
- html

```html
<view class="tabs">
  <view class="tabs_title">
    <view 
    class="title_item"
    wx:for="{{exchange}}"
    wx:key="id"
    bindtap="handleItemComponents"
    data-index="{{index}}"
    >
      <text class="{{item.isActive?'active':'noactive'}}">{{item.value}}</text>
    </view>
  </view>
  <view class="tabs_content">
    <slot></slot>
  </view>
</view>
```

- css

```css
.tabs_title{
  display: flex;
  background-color: #F5F5F5;
  height: 66rpx;
}
.title_item{
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15rpx 0;
}
.active{
  color:rgba(53,53,53,1);
  font-size: 28rpx;
  font-family:Source Han Sans CN;
  font-weight:400;
  border-bottom: 3rpx solid #2A2A2A;
  padding: 10rpx 0;
}
.noactive{
  font-size:28rpx;
  font-family:Source Han Sans CN;
  font-weight:400;
  color:#787878;
  padding: 10rpx 0;
}
```

- js

```javascript
// components/Tabs/Tabs.js
Component({
  /**
   * 组件的属性列表
   */
  properties: {
    exchange:{
      type:Array,
      value:[]
    }
  },
  /**
   * 组件的初始数据
   */
  data: {

  },

  /**
   * 组件的方法列表
   */
  methods: {
    handleItemComponents(e){
      const {index} = e.currentTarget.dataset;
      this.triggerEvent("tabsItemChange",{index});
    }
  },
})

```

父组件，引入

- json

```json
"usingComponents": {
    "exchange":"../../components/exchange/exchange"
  },
```

- html

```html
<exchange exchange="{{exchange}}" bindtabsItemChange="handleTabsItemChange"
<view wx:if="{{exchange[0].isActive}}">
</view>
<view class="wait_recived_wrap" wx:if="{{exchange[1].isActive}}">
</view>
<view class="completed_wrap" wx:if="{{exchange[2].isActive}}">
</view>
</exchange>
```

- js

```javascript
// pages/exchange_list/index.js
Page({

  /**
   * 页面的初始数据
   */
  data: {
    exchange: [
      {
        id: 0,
        value: "待发货",
        isActive: true
      },
      {
        id: 1,
        value: "待收货",
        isActive: false
      },
      {
        id: 2,
        value: "已完成",
        isActive: false
      }
    ],
  },
  // 导航栏
  handleTabsItemChange(e) {
    console.log(e)
    const { index } = e.detail;
    let { exchange } = this.data;
    exchange.forEach((v, i) => i === index ? v.isActive = true : v.isActive = false);
    this.setData({
      exchange
    })
  },
  
})
```



## URL的hash

- url的hash也就是锚点（#），本质上改变window.location的href属性
- 我们可以通过直接赋值location.hash来改变href（但是页面不发生刷新）

- h5中history也能改变url

  - history.pushState((),'','home')，网页不会刷新，有返回效果
  - history.replaceState((),'','home')

  ![image-20200824094232185](images/image-20200824094232185.png)



## router-view

渲染内容的位置



## router-link

- 相当于a标签，tag更改标签为span标签，to为要跳转的路由页面

![image-20200917225454888](images/image-20200917225454888.png)

- 路由传参
  - 直接在to里面传参，不用修改path，this.$route可以查参数

![image-20200917232511913](images/image-20200917232511913.png)

![image-20200917232418720](images/image-20200917232418720.png)

- 使用params传参，$route.params获取数据

![image-20200917233410710](images/image-20200917233410710.png)

![image-20200917233440844](images/image-20200917233440844.png)

![image-20200918102538665](images/image-20200918102538665.png)



## vue-router的使用

![image-20200824100906312](images/image-20200824100906312.png)

![image-20200824100933042](images/image-20200824100933042.png)

- path为路径，component为要传入渲染的组件，出现path路径下的内容则显示component组件
- redirect重定向，更改hash值，一打开直接显示/login组件，path也可不填，即默认值,path:''

![image-20200824101326538](images/image-20200824101326538.png)

- mode:'history'改变路由的地址，把#去掉，将hash改为history模式

![image-20200824102529147](images/image-20200824102529147.png)

- to跳转的路径，tag改变标签，默认为a标签，replace把history模式改为replace模式，即没有返回链接 
- 使用tag后，button自带class名字
- active-class改变自带class的名字，也可通过router下的index.js进行修改class的名字
- this.$router.push('/home')，指定跳转home页面，每个组件都有$router，vue-router自带的，有返回
- this.$router.repalce('/home')，这个没有返回

![](images/image-20200824104330703.png)

![](images/image-20200824104515020.png)

![image-20200824103937191](images/image-20200824103937191.png)



## 完善eslint语法

![image-20200824103241006](images/image-20200824103241006.png)



## 路由的懒加载

- 打包成一个个js，用到时在加载

- 使用

![image-20200825094236419](images/image-20200825094236419.png)



## 路由的嵌套

- 创建对应的子组件，并且在路由由映射中匹配对应的子路由
- 在组件内部使用router-view标签

![image-20200825102614484](images/image-20200825102614484.png)



## $route和$router

- router是路由栈，route是当前活跃的路由
- vue的原型加属性，调用时this.name即可调用

![image-20200826102746562](images/image-20200826102746562.png)

- $router为VueRouter实例，想要导航到不同URL，则使用$router.push方法
- $route为当前router跳转对象里面可以获取name，path，query，params等



## 路由导航守卫

- 监听每次组件点击后的要发生的事件
- 参数to，from，next
- 必须调next（）
- 修改各组件的标题,加meta增加一个title属性

```javascript
const routes = [
    { path: '/', redirect: '/login' },
    { 
      path: '/login', 
      component: Login,
      meta:{
        title:'登录'
      }
    },
    { 
      path: '/home', 
      component: Home ,
      redirect: '/welcome',
      children:[
        {
          path:'/welcome',
          component:Welcome,
          meta:{
            title:'欢迎'
          }
        },
        {
          path:'/users',
          component:User,
          meta:{
            title:'用户列表'
          }
        }
      ]
    }
]
```

```javascript
// 路由权限守卫，前置守卫（guard）
router.beforeEach((to, from, next) => {
  // document.title=to.matched[0].meta.title;
    //修改标题
  document.title=to.meta.title;
  // console.log(to);
    //获取缓存中的token
  if (to.path === '/login') return next();
  const tokenStr = window.sessionStorage.getItem('token');
  if (!tokenStr) return next('/login');
  next();
})
//后置守卫,跳转页面后执行after
router.afterEach((to, from) => {
    
}) 
```



## keep-alive

- 提高性能，执行一次created函数，使得切换点击链接时不会刷新
- exclude="路由名" , 排除哪个路由

```html
<keep-alive>
      <router-view></router-view>
</keep-alive>
```

![image-20200826163958755](images/image-20200826163958755.png)



# Vuex

- Vuex是一个专为Vue.js应用程序开发的状态管理模式

- Vuex官方调试工具devtools

- 用户的登录状态、用户名称、头像、地理位置信息

- 商品的收藏、购物车中的物品

- 浏览器安装插件vue.js.devtools

- 多界面状态管理使用

  - main.js,通过$store去调用store

  ```javascript
  import Vue from 'vue'
  import App from './App'
  //挂载
  import store from './store'
  Vue.config.productionTip = false
  Vue.prototype.$store = store
  new Vue({
  	el:'#app',
  	store,
  	render:h=>h(App)
  })
  ```
  
  -  src下创建文件夹为store，文件名为index.js文件
```javascript
  import Vue from 'vue'
  import Vuex from 'vuex'
  //安装插件
  Vue.use(Vuex)
  //创建对象
  const store = new Vuex.Store({
      //保存状态
  	state:{
  		counter:1000
  	},
      //用于改state中的状态
  	mutations:{
          //方法
          increment(state){
          	state.counter++    
          }
          decrement(state){
      		state..counter--
  		}
      },
      //异步操作(发送网络请求)                         
  	actions:{},
      //计算属性
  	getters:{},
      //划分模块
	modules:{}
  })
  //导出store
export default store
```

  ![image-20200905093142426](images/image-20200905093142426.png)
- 页面中调用，通过mutations来定义
  
  ![image-20200905102208547](images/image-20200905102208547.png)
  
  ![](images/image-20200905101902526.png)
  
- mutations,参数为count，称为payload,默认参数state，必须填，用于改store里面的state

第一种提交

![image-20200905150518467](images/image-20200905150518467.png)

![image-20200905150442571](images/image-20200905150442571.png)

![image-20200905150606497](images/image-20200905150606497.png)

第二种提交

![image-20200910105726670](images/image-20200910105726670.png)

![image-20200910105749928](images/image-20200910105749928.png)

- getters相当于计算属性

![image-20200909215310862](images/image-20200909215310862.png)

![image-20200909215415289](images/image-20200909215415289.png)

- state中初始化定义的数据是响应式的，特殊方法定义的响应式Vue.set，Vue.delete

![](images/image-20200910204600370.png)

- actions，用于异步操作

![image-20200911094827584](images/image-20200911094827584.png)

![image-20200911094836683](images/image-20200911094836683.png)



## 父子组件传递

![image-20200905161136992](images/image-20200905161136992.png)

![image-20200905161154409](images/image-20200905161154409.png)



## 跑马灯

substring，第一个参数为索引，从0开始，第二个参数为第几个字符

![](images/image-20200915135727744.png)

![image-20200915140343901](images/image-20200915140343901.png)

![image-20200915135757278](images/image-20200915135757278.png)

停止定时器

![](images/image-20200915140540014.png)



## Vue样式

- 外联样式：data中的数据不用加引号，可用三元表达式，也可用对象

```html
<h1 :class="['thin','red']">123</h1>
```

![image-20200915231730225](images/image-20200915231730225.png)

![image-20200915231826639](images/image-20200915231826639.png)

![image-20200915232210651](images/image-20200915232210651.png)

- 内联样式：可放数组，data绑定放对象。有-的要加引号

```html
<h1 :style="{color:'red','font-weight':200}">123</h1>
<h1 :style="[styleObj1,styleObj2]">123</h1>
```



## Vue动画

vue提供的transition标签

![image-20200916214206744](images/image-20200916214206744.png)

![image-20200916214632648](images/image-20200916214632648.png)

![image-20200916214538705](images/image-20200916214538705.png)



## 路由高亮

.router-link-active设置样式



## 路由动画

mode为动画的效果，添加动画样式

**![image-20200917231525007](images/image-20200917231525007.png)**



## 路由经典布局

![image-20200920163439828](images/image-20200920163439828.png)

![image-20200920163325931](images/image-20200920163325931.png)

![image-20200920163352237](images/image-20200920163352237.png)



## 父子组件传值

- 父传子

![image-20200920165108427](images/image-20200920165108427.png)

![image-20200920165050354](images/image-20200920165050354.png)

- 子传父

![image-20200920165409172](images/image-20200920165409172.png)

![image-20200920165441430](images/image-20200920165441430.png)

![image-20200920165242019](images/image-20200920165242019.png)



## watch

- 可以监视data中的数据

![image-20200923231823861](images/image-20200923231823861.png)

![image-20200923231752880](images/image-20200923231752880.png)

- 监视路由地址的改变

![image-20200923232420279](images/image-20200923232420279.png)



## computed

![image-20200925102836646](images/image-20200925102836646.png)

![image-20200925102943560](images/image-20200925102943560.png)



## 修改Webpack

![image-20200927210242990](images/image-20200927210242990.png)



## 修改打包入口

一个是发布模式的，用于生产，一个是的开发模式，用于开发

```javascript
module.exports = {
  chainWebpack: config => {
    //发布模式
    config.when(process.env.NODE_ENV === 'production',config => {
      config.entry('app').clear().add('./src/main-prod.js')

      config.set('externals',{
        vue:'Vue',
        'vue-router':'VueRouter',
        axios:'axios',
        lodash:'_',
        echarts:'echarts',
        nprogress:'NProgress',
        'vue-quill-editor':'vueQuillEditor'
      })
    })
    //开发模式
    config.when(process.env.NODE_ENV === 'development',config => {
      config.entry('app').clear().add('./src/main-dev.js')
    })
  }
}
```

![image-20200927210544690](images/image-20200927210544690.png)

![image-20200927210716761](images/image-20200927210716761.png)

1.通过externals加载外部CDN资源，优化import导入的资源，在vue.config.js开发模式中加入

```javascript
config.set('externals',{
        vue:'Vue',
        'vue-router':'VueRouter',
        axios:'axios',
        lodash:'_',
        echarts:'echarts',
        nprogress:'NProgress',
        'vue-quill-editor':'vueQuillEditor'
      })
```

2.在main-prod.js中删除富文本和NProgress样式和element-ui.js，然后public在index.html中加入

```html
    <% if(htmlWebpackPlugin.options.isProd){ %>
      <!-- nprogress 的样式表文件 -->
      <link rel="stylesheet" href="https://cdn.staticfile.org/nprogress/0.2.0/nprogress.min.css" />
      <!-- 富文本编辑器 的样式表文件 -->
      <link rel="stylesheet" href="https://cdn.staticfile.org/quill/1.3.4/quill.core.min.css" />
      <link rel="stylesheet" href="https://cdn.staticfile.org/quill/1.3.4/quill.snow.min.css" />
      <link rel="stylesheet" href="https://cdn.staticfile.org/quill/1.3.4/quill.bubble.min.css" />
      <!-- element-ui 的样式表文件 -->
      <link rel="stylesheet" href="https://cdn.staticfile.org/element-ui/2.8.2/theme-chalk/index.css" />
  
      <script src="https://cdn.staticfile.org/vue/2.5.22/vue.min.js"></script>
      <script src="https://cdn.staticfile.org/vue-router/3.0.1/vue-router.min.js"></script>
      <script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js"></script>
      <script src="https://cdn.staticfile.org/lodash.js/4.17.11/lodash.min.js"></script>
      <script src="https://cdn.staticfile.org/echarts/4.1.0/echarts.min.js"></script>
      <script src="https://cdn.staticfile.org/nprogress/0.2.0/nprogress.min.js"></script>
      <!-- 富文本编辑器的 js 文件 -->
      <script src="https://cdn.staticfile.org/quill/1.3.4/quill.min.js"></script>
      <script src="https://cdn.jsdelivr.net/npm/vue-quill-editor@3.0.4/dist/vue-quill-editor.js"></script>
  
      <!-- element-ui 的 js 文件 -->
      <script src="https://cdn.staticfile.org/element-ui/2.8.2/index.js"></script>
  
      <% } %>
```



## 路由的懒加载

- 运行依赖中安装@babel/plugin-syntax-dynamic-import

- 在babel.config.js中配置文件

```javascript
module.exports = {
  "presets": [
    "@vue/cli-plugin-babel/preset"
  ],
  "plugins": [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ],
    // 配置路由的懒加载
    '@babel/plugin-syntax-dynamic-import'
  ]
}
```

- webpackChunkName打包的分组，打包到各自拥有的js文件中

![image-20200928095943097](images/image-20200928095943097.png)

## 项目上线

- 通过node创建web服务器
- cls清楚命令
- npm init -y初始化包管理文件
- npm i express -S安装包管理工具
- app.js中配置文件
- node .\app.js运行

```javascript
const express = require('express')
const app = express()

app.use(express.static('./dist'))

app.listen(80,()=>{
  console.log('server running at http://127.0.0.1')
})
```

![image-20200928102138613](images/image-20200928102138613.png)

- Gzip网络传输压缩
  - npm i compression -S
  - 一定要把app.use(compression())写在静态资源托管之前

![image-20200928165009495](images/image-20200928165009495.png)

- 配置https服务

![image-20200928165936243](images/image-20200928165936243.png)

- 使用pm2管理项目

![image-20200928170802155](images/image-20200928170802155.png)

