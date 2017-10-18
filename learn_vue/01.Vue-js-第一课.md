01. Vue.JS 第一课 02:51 
Vue 1.0.21 Sublime Text
#1 、vue:
  读音:	v-u-e
  view

#2、vue到底是什么?
  一个mvvm框架(库)、和angular类似
  比较容易上手、小巧
  mvc:
    mvp
    mvvm
    mv*
    mvx

#3、官网:http://cn.vuejs.org/	
手册： http://cn.vuejs.org/api/

#4、vue和angular区别?
 共同点: 不兼容低版本IE
  ##vue——简单、易学
    指令以 v-xxx
    一片html代码配合上json，再new出来vue实例
    个人维护项目

    适合: 移动端项目,小巧

    vue的发展势头很猛，github上start数量已经超越angular
  ##angular——上手难
    指令以 ng-xxx
    所有属性和方法都挂到$scope身上
    angular由google维护
		
    合适: pc端项目

-------------------------------------------
#5、vue基本雏形:
  ##angular展示一条基本数据:

    var app=angular.module('app',[]);

    app.controller('xxx',function($scope){  //C
        $scope.msg='welcome'
    })

    html:
    div ng-controller="xxx"
      {{msg}}
  ##vue:
    html:
    <div id="box">
        {{msg}}
    </div>

    var c=new Vue({
      el:'#box',	//选择器  class tagName
      data:{
        msg:'welcome vue'
      }
    });

#6、常用指令:
指令: 扩展html标签功能,属性
  ##angular: 
    ng-model   ng-controller
    ng-repeat
    ng-click
    ng-show  

    $scope.show=function(){}
    
##Vue:
1) v-model 一般表单元素 双向数据绑定
    实例：v_model01.html v_model02.html v_model03.html
2) new Vue data:里面的数据可以是string,number,boolean,array,json
    示例：5 vue_data数据类型
3) 循环： v-for
    a-循环数组：

          <ul>
              <li v-for="value in arr">
                   {{$index}} {{value}}  <!-- 索引值 值 -->
               </li>
          </ul>

b--循环json
```
    <ul>
      <li v-for="items in json">
        {{$index}}--{{$key}}: {{items}}  <!-- 索引、键、值 -->
      </li>
   </ul>
```
    <ul>
      <li v-for="(k,v) in json2">
        {{$index}}--{{k}}: {{v}}  <!-- 索引、键、值 -->
      </li>
    </ul>

4）事件：配合methods使用

      v-on:click= ""
      v-on:mouseover= ""
      v-on:mouseout=""
      v-on:mousedown=""
      v-on:dblclick=""
实例见 8 vue_基础事件02.html
```
  <input type="button" value="按钮" v-on:click="add()" />
  <script type="text/javascript">
    var vm = new Vue({  //{}--json形式；c可取任意名字
      el: '#box', //标签选择器亦可
      data: {
        arr: ['apple','orange','banana'],  //数组
      },
      methods: {  //务必记得加s
        add: function(){
          // alert(this.arr);
          this.arr.push('pear'); //this指vm
        }
      }
    });
  </script>
```
5）显示隐藏:

      v-show=“true/false”

bootstrap+vue简易留言板(todolist):
	
	bootstrap: css框架	跟jqueryMobile一样
		只需要给标签 赋予class，角色
		依赖jquery

	确认删除？和确认删除全部么?

-----------------------------------------

#7、事件:
v-on:click/mouseover......
简写的:
@click=""		推荐
例子：
```
<input type="button" value="按钮" v-on:click="show()" />
<input type="button" value="按钮" @click="show()" />
```
#8、事件对象:
@click="show($event)"
示例：
```
<div id="box">
  <input type="button" value="按钮" @click="show($event)" />
</div>
<script type="text/javascript" src="vue.js"></script>
<script type="text/javascript">
  window.onload = function() {
    var c = new Vue({
      el: "#box",
      data: {

      },
      methods: {
        show: function(ev){
          alert(ev.clientX);
        }
      }
    });
  }
</script>
```
#9、事件冒泡:
实例：点击按钮 会以此弹出1和2，说明出现了事件冒泡
```
<div id="box">
  <div @click="show2()">
    <input type="button" value="按钮" @click="show()" />
  </div>
</div>
```
```
<script type="text/javascript" src="vue.js"></script>
<script type="text/javascript">
window.onload = function() {
  var c = new Vue({
    el: "#box",
    data: {

    },
    methods: {
      show: function(){
        alert(1);
      },
      show2: function(){
        alert(2);
      }
    }
  });
}
</script>
```
##阻止冒泡:  
###a). ev.cancelBubble=true; [原生JS]
```
  <div @click="show2()">
     <input type="button" value="按钮" @click="show($event)" />
  </div>
  var c = new Vue({
    el: "#box",
    data: {

    },
    methods: {
      show: function(ev){
        alert(1);
        ev.cancelBubble=true;

      },
      show2: function(){
        alert(2);
      }
    }
  });
```
###b). @click.stop	推荐
```
<div id="box">
  <div @click="show2()">
    <input type="button" value="按钮" @click.stop="show($event)" />
  </div>
</div>
```
```
var c = new Vue({
    el: "#box",
    data: {

    },
    methods: {
      show: function(ev){
        alert(1);
      },
      show2: function(){
        alert(2);
      }
    }
  });
```
#10、默认行为(默认事件):
阻止默认行为:
##a). ev.preventDefault();  [原生JS]
```
  <div id="box">
    <input type="button" value="按钮" @contextmenu="show($event)" />
  </div>

```
```
  var c = new Vue({
    el: "#box",
    data: {

    },
    methods: {
      show: function(ev){
        alert(1);
        ev.preventDefault();
      }
    }
  });
}
</script>
```
##b). @contextmenu.prevent	推荐
```
  <div id="box">
    <input type="button" value="按钮" @contextmenu.prevent="show()" />
  </div>
```

#11、键盘:
@keydown	$event	ev.keyCode
@keyup
###示例1：
```
  <div id="box">
    <input type="button" value="按钮" @keydown="show()" />
  </div>
```
###示例2：
```
<div id="box">
    <input type="text" @keydown="show($event)" />
  </div>

  methods: {
      show: function(ev){
        alert(ev.keyCode);
      }
  }
```
##常用键:
###回车(ev.keyCode == 13)
a). @keyup.13 
``` 
<input type="text" @keydown.13="show()" />
 ```
b). @keyup.enter
``` 
<input type="text" @keydown.enter="show()" />
 ```
c). 上、下、左、右
- @keyup/keydown.left
- @keyup/keydown.right
- @keyup/keydown.up
- @keyup/keydown.down
			.....
-----------------------------------------
#12、属性:
	v-bind:src=""
		width/height/title....
	
	简写:
	:src=""	推荐

	![]({{url}})	效果能出来，但是会报一个404错误
	![](url)	效果可以出来，不会发404请求

![vue_属性01.jpg](http://upload-images.jianshu.io/upload_images/4329360-760a4857fd87ecae.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
        
-----------------------------------------
##特殊属性：class和style:
### 1、class
- :class=""	
  v-bind:class=""
  :style=""	        
  v-bind:style=""

- :class="{red:true, blue:true}"  	
```
  <div id="box">
    <strong :class="{red: true,big:true}">class属性</strong>
  </div>

  <style type="text/css">
    .red{
      color: red;
    }
    .big{
      font-size: 30px;
    }
  </style>

```
效果图：

![vue_class属性01.jpg](http://upload-images.jianshu.io/upload_images/4329360-97eee79368d3e74f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

:class="{red:a, blue:false}"  
```
  <div id="box">
    <strong :class="{red:a,blue:false}">class属性</strong>
  </div>

  <style type="text/css">
    .red{color: red;}
    .blue{background-color: blue;}
  </style>

    var vm=new Vue({
      el: '#box',
      data: {
        a: true
      }
    });
```
- :class="[red]"	         //red是data里面的数据

  :class="[red,b,c,d]"   //多个class的情况

![vue_class属性02.jpg](http://upload-images.jianshu.io/upload_images/4329360-ef5c8afb24f5789e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
-------------------------------------------------------------------------------------------------

- :class="json"
```		
new Vue({
    el:'#box',
    data:{
        json:{
            red:true,
            blue:true
        }
    }
});
<div id="box">
        <strong :class="json">文字...</strong>
</div>
```
-------------------------------------------------------------------------------------------------
###2、style:
```
<strong :style="{color:'red'}">style属性</strong>
<!-- 注意:red加引号 -->
```
- :style="[c]"

![vue_style01.jpg](http://upload-images.jianshu.io/upload_images/4329360-f16acd059636d438.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- :style="[c,d]"
注意:  复合样式，采用驼峰命名法

![vue_style02.jpg](http://upload-images.jianshu.io/upload_images/4329360-19f387dab36df5a6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- :style="json"  --官方推荐


![vue_style03.jpg](http://upload-images.jianshu.io/upload_images/4329360-d80016d3068f91e7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


--------------------------------------------------------------
#模板:
- {{msg}}		数据更新模板变化
- {{*msg}}	数据只绑定一次


![vue_msg01.jpg](http://upload-images.jianshu.io/upload_images/4329360-565198a6386836d0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
- {{{msg}}}	HTML转义输出

![vue_转义输出.jpg](http://upload-images.jianshu.io/upload_images/4329360-ad25b2b17bb00686.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

-----------------------------------------
#过滤器:-> 过滤模板数据
系统提供一些过滤器:
	
{{msg| filterA}}
{{msg| filterA | filterB}}

uppercase	eg:	{{'welcome'| uppercase}}
lowercase
capitalize --首字母大写
currency	钱
{{msg| filterA 参数}}
	....
实例如下：

![过滤器01.jpg](http://upload-images.jianshu.io/upload_images/4329360-1a32302e1b0fdaf6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

-----------------------------------------
#交互:
	Angular:$http	（ajax对象）

	如果vue想做交互

	引入: vue-resouce (库)

	get:
		获取一个普通文本数据:
		this.$http.get('aa.txt').then(function(res){
		    alert(res.data);
		},function(res){
		    alert(res.status);
		});
		给服务发送数据:√
		this.$http.get('get.php',{
		    a:1,
		    b:2
		}).then(function(res){
		    alert(res.data);
		},function(res){
		    alert(res.status);
		});
PHP我不会也~~呜呜
	post:
		this.$http.post('post.php',{
		    a:1,
		    b:20
		},{
		    emulateJSON:true
		}).then(function(res){
		    alert(res.data);
		},function(res){
		    alert(res.status);
		});

	jsonp:
		https://sug.so.360.cn/suggest?callback=suggest_so&word=a
		https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd=a&cb=jshow
		this.$http.jsonp('https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su',{
		    wd:'a'
		},{
		    jsonp:'cb'	//callback名字，默认名字就是"callback"
		}).then(function(res){
		    alert(res.data.s);
		},function(res){
		    alert(res.status);
		});
		
https://www.baidu.com/s?wd=s

	作业:
		1. 简易留言-> 确认删除? 确认删除全部
		2. 用vue get 写个例子	weibo
