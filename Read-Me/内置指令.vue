事件修饰符：
prevent 阻止默认事件（常用）
stop  阻止时间冒泡
once  事件只触发一次（常用）
capture  使用事件的捕获方式
self  只有event, target是当前操作的元素时才触发事件
passive  事件的默认行为立即执行，无需等待事件回调执行完成


内置指定：
v-bind  单向绑定解析表达式，可简写为：xxx
v-model 双向数据绑定
v-for  遍历数组/对象/字符串
v-on  绑定时间监听，可简写为@
v-if  条件渲染（动态控制节点是否存在）
v-else  条件渲染（条件控制节点是否存在）
v-show  条件渲染（动态控制节点是否展示）

v-text  向其所在的标签插入文本  不支持结构解析
v-html 向指定节点中渲染包含HTML结构的内容 且 v-html 会替换节点中的所有内容 但是会导致XSS攻击
v-cloak  本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删除掉 v-cloak 属性 
        使用css配合 v-cloak 可以解决网速慢时页面展示出 {{xxx}} 的问题

v-once  所在节点在初次动态渲染后，就视为静态内容了
        以后数据的改变不会引起 v-once 所在结构的更新，可以用于优化性能        
v-pre   可以让其所在节点跳过编译的过程，
        可利用它跳过，没有使用指令语法，没有使用插值语法的节点，会加快编译

自定义指令：
总结：
    1）局部指令：
    new Vue({                                   new Vue({
        directives:{ 指令名:配置队形 }   或者           directives:{ 指令名:函数 }
    })                                          })

    2）全局指令: Vue.directive('自定义指令名', 配置对象)  或者  Vue.directive('自定义指令名', 回调函数)
    
    3）配置对象中常用的三个回调函：
      bind 指令与元素成功绑定时                   
      inserted  指令所在元素被插入页面时              
      update 指令所在的模板被重新解析时
    4）指令名定义时不加 v-, 但使用时要加 v-  
    5）指令名如果是多个单词，要使用kebab-case命名方式，不要使用camelCase命名

1.自定义指令 v-bind  定义时用 bind  使用时用 v-bind
new Vue({
    el:"#root",
    data:{
        num:1
    },
    directives:{
        // 此自定义的函数何时才会被调用 1.指定于元素成功绑定时  2.指令所在的模板被重新解析时
        big(element, binding){
            element.innerText = binding.value * 10 
        }
    }
})

2.自定义指令 v-fbind
new Vue({
    el:"#root",
    data:{
        num:1
    },
    // 局部指令
    directives:{
        // 此自定义的函数何时才会被调用 1.指定于元素成功绑定时  2.指令所在的模板被重新解析时
        fbind:{  // 钩子？？？？
            //指令与元素成功绑定时（一上来）
            bind(element, binding){
                element.value = binding.value;
            },
            //指令所在元素被插入页面时
            inserted(element, binding){
                // 自动获取焦点
                element.focus();
            },
            //指令所在的模板被重新解析时
            update(element, binding){
                element.value = binding.value;
            }
        }
    }
})

3.自定义指令易错点
  3.1 自定义指令命名不支持驼峰命名法  错误使用 v-bigNumber  正确使用 写成函数式的 'bing-number':function(element,binding){}
  3.2 自定义指令 里面如果使用到 this 关键字 则此 this 为window而不是 vue
  3.3 定义全局指令 跟过滤器一样，需要在一开始的时候就声明好
      Vue.directive('自定义指令名', {
          //指令与元素成功绑定时（一上来）
            bind(element, binding){
                element.value = binding.value;
            },
            //指令所在元素被插入页面时
            inserted(element, binding){
                // 自动获取焦点
                element.focus();
            },
            // 指令所在的模板被重新解析时
            update(element, binding){
                element.value = binding.value;
            }
      })


// template 与 v-if配合使用来实现 展示和隐藏
<template v-if="1 == 1"><span>哈哈哈哈哈</span></template>

注意：如果vue实例里面包含了模板 template 并且模板中的代码值需要换行展示，对应的属性值需要使用es6的写法 `<h2>当前的n值为{{n}}</h2>` 否则会报错
      template:`<h2>当前的n值为{{n}}</h2>`

生命周期
    mounted 跟data平级
    //Vue完成模板的解析并把初始的真实DOM元素放入页面后（挂载完毕）调用 mounted
    mounted(){ 
        do something……
    }


    声明周期---挂载流程
    init  Events & Lifecycle  初始化：生命周期、事件，但数据代理还未开始
        beforeCreate  此时：无法通过vm访问到data中的数据、methods中的方法
    init  injections & reactivity  初始化：数据监测、数据代理
        created  此时：可以通过vm访问到data中的数据、methods中配置的方法
   【解析模板阶段】此阶段开始解析模板，生成虚拟DOM(内存中)，页面还不能显示解析好的内容
    beforeMount  此时：1.页面呈现的是 “未经编译” 的DOM结构   2.所有对DOM的操作，最终都不奏效
    Create vm.$el and replace "el" with it  将内存中的虚拟DOM转为真实DOM插入页面
    mounted  此时： 1.页面呈现的是 “经过vue”编译后的DOM，
                    2.对DOM的操作均有效（尽可能的避免）.至此，初始化过程结束，
                    一般在此进行：开启定时器、发送网络请求、订阅消息、绑定自定义事件、等初始化操作
    Mounted（声明周期--》更新流程） 注意大小写 
    

    声明周期---更新流程（Mounted）
    beforeUpdate 此时：数据是最新的，但页面时旧的，即：页面尚未和数据保持同步
    根据新数据，生成新的虚拟DOM,随后与旧的虚拟DOM进行比较，最终完成页面刷新，即：完成了Model ->View的更新
    updated 此时：数据是新的，页面也是新的，即：数据和页面保持同步

    声明周期---销毁流程（Mounted）
    beforeDestroy 此时：vm中所有的：data,methods,指令等等，都处于可用的状态，马上要执行销毁过程，
                        一般在此阶段：关闭定时器、取消订阅消息、“解绑自定义事件” 等收尾工作
    destroyed


    生命周期总结：
        生命周期钩子：beforeCreate、created、beforeMount、mounted、beforeUpdate、updated、beforeDestroy、destroyed
        常用的生命周期钩子：1.mounted 发送异步请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】
                          2.beforeDestroy：清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】
        关于销毁Vue实例：
            1.销毁后借助VUE开发者工具看不到任何信息
            2.销毁后自定义的事件会失效，但原生DOM事件仍然有效
            3.一般不会在 beforeDestroy 操作数据，因为即便操作数据，也不会再触发更新流程
        

    