<div>
    <div id="root"></div>
    当前时间为：{}
    // 计算属性
    <h3>{{ftmTime}}</h3>
    // methods
    <h3>{{getFtmTime()}}</h3>
    // 过滤器实现
    <h3>{{time | timeFormater }}</h3>
    // 过滤器传参
    <h3>{{time | timeFormater('YYYY_MM_DD') }}</h3>
    // 过滤器传参串联
    <h3>{{time | timeFormater('YYYY_MM_DD') | mySclice }}</h3>
    // v-bind 使用过滤器  :x="msg
    <h3 :x="msg | mySclice">hello,word</h3>

</div>

<script type="text/javascript">
    // 过滤器适用于一些简单的逻辑处理 过于复杂的尽量使用 计算属性  或者 方法
    Vue.config.productionTip = false;
    // 全局过滤器 多个 vue实例都可以使用 注意：必须在创建之前提前声明好
    Vue.filter('mySclice',function(value) {
         return value.splice(0,4);
    });

    new Vue({
        el:"#root",
        data:{
            time: 1635509371000 // 时间戳
            x
        },
        // 计算属性
        computed:{

        },
        // 函数方法
        methods:{

        },
         // 局部过滤器
        filters:{
            // es6语法，如果 str为空则 格式为 YYYY年MM月DD日 HH:mm:ss
            timeFormater(value,str = 'YYYY年MM月DD日 HH:mm:ss'){
                return 格式化后的时间
            }
        }

    })

</script>