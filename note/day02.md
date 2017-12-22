## 自定义指令
> module/controller/directive/serivce/filter

1. 指令的创建方法
```
var app = angular.module();
app.directive('指令名称',func);
```

2. 指令的使用方法
> 涉及到命名规则，指令本身命名要符合驼峰式写法，一般有两部分构成，前缀（项目或者模块的简写）和后缀（指令的作用），在使用时 要遵循烤串式

```
app.directive('tsHello',func)
ts-hello
```

3. 指令的其他特征
    1. restrict:'EC'
        - E:element
        - A:attribute
        - C:class
        - M:commit
    2. replace:true/false
        - 将调用指令的元素替换
        - div ts-hello
    3. template:指定模板内容
    4. scope:储存数据

```html
<!DOCTYPE html>
<html ng-app="myApp">
<head lang="en">
    <meta charset="UTF-8">
    <script src="js/angular.js"></script>
    <title></title>
</head>
<body>

<div ng-controller="myCtrl">
    <p>{{num}}</p>
    <!-- element-->
    <ts-hello></ts-hello>
    <!-- attribute-->
    <div ts-hello></div>
    <!-- class-->
    <div class="ts-hello"></div>
    <!-- comment-->
    <!-- directive:ts-hello -->
</div>
<script>
    var app = angular.module('myApp', ['ng']);
    //自定义指令
    app.directive('tsHello', function () {
        return {
            template: '<h1> Hello Custom Directive</h1>',
            restrict: 'EACM',
            replace: true
        }
    });
    app.controller('myCtrl', function ($scope) {
        $scope.num = 10;
    });
</script>
</body>
</html>
```
4. 自定义指令参数
> 指令内部设置允许接受参数

scope:对象
```
app.directive('tsHello', function () {
	return {
		template: "<h1>{{testName}}</h1>",
		scope:{
			// @ 指定要从调用指令的属性中 找到test-name对应的值 赋值给testName
			testName:'@'
				}
			}
  })
```
> 调用指令时,把参数发送过去

```
<ts-hello test-name="参数"></ts-hello>
```
```html
<!DOCTYPE html>
<html ng-app="myApp">
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <script src="js/angular.js"></script>
</head>
<body>
<div ng-controller="myCtrl">
  <!--在调用指令时 传递参数-->
  <ts-hello test-name="参数"></ts-hello>
</div>
<script>
  var app = angular.module('myApp', ['ng']);

  app.directive('tsHello', function () {
    return {
      template: "<h1>{{testName}}</h1>",
      scope:{
        // @ 指定要从调用指令的属性中 找到test-name对应的值 赋值给testName
        testName:'@'
      }
    }
  });
  app.controller('myCtrl', function () {})
</script>
</body>
</html>
```

5. 练习
> 创建自定义指令tsDirective,传入ts-age='20';
并通过调用指令显示在p标签

```html
<!DOCTYPE html>
<html ng-app="myApp">
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <script src="js/angular.js"></script>
</head>
<body>
<div ng-controller="myCtrl"></div>
<div ts-directive ts-age="20"></div>
<script>
  var app = angular.module('myApp', ['ng']);
  //实现自定义指令并传参显示出来
  app.directive('tsDirective', function () {
    return {
      //指令相关的设置
      template:'<p>{{tsAge}}</p>',
      scope:{
        tsAge:'@'
      }
    }
  });
  app.controller('myCtrl', function () {});
</script>
</body>
</html>
```

## 双向数据绑定
方向1: 将数据绑定视图

绑定方法：
1. ng中常用指令 ngRepeat
2. 双花括号
          
方向2：将视图中的用户操作的结果绑定数据
1. 绑定方式：ngModel
    
2. 



## 过滤器、函数
1. 过滤器的概述
> Selects a subset of items from array and returns it as a new array.

> 对数据进行筛选、过滤、格式化

> 过滤器使用语法：支持多重过滤

```
{{表达式 | 过滤器:参数 | 过滤器}}
```
```html
<!DOCTYPE html>
<html ng-app="myApp">
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="js/angular.js"></script>
</head>
<body>
<div ng-controller="myCtrl">
    <p>{{price}}</p>
    <!-- 基本用法-->
    <p>{{price | currency}}</p>
    <!-- 接收参数-->
    <p>{{price | currency:'￥'}}</p>
    <!-- 日期过滤器-->
    <p>{{nowDate}}</p>
    <p>{{nowDate | date:'yy-MM-dd'}}</p>
    <!-- 排序过滤器-->
    <ul>
        <li ng-repeat="stu in stuList | orderBy:'age':true">
            {{"age is "+stu.age+" name is "+stu.name}}
        </li>
    </ul>
    <!-- 使用uppercase lowercase 字符串大小写的转换-->
    <p>{{name}}</p>
    <p>{{name | uppercase}}</p>
    <p>{{name | lowercase}}</p>
    <!-- limitTo 限定集合中显示的元素的个数-->
    <ul>
        <li ng-repeat="tmp in list | limitTo:2 ">
            {{tmp}}
        </li>
    </ul>
</div>
<script>
    var app = angular.module('myApp', ['ng']);

    app.controller('myCtrl', function ($scope) {
        $scope.price = 1234;
        $scope.nowDate = new Date();
        $scope.name = 'Michael';
        $scope.stuList = [
            {age: 18, name: 'zhangsan'},
            {age: 20, name: 'zhangda'},
            {age: 19, name: 'zhanger'}
        ];
        $scope.list = [100, 200, 300];
    });
</script>
</body>
</html>
```
2. 常用过滤器
- currency 货币样式
- date 日期
- orderBy 排序（降序或者升序）
- limitTo 限定显示的元素的个数（数组）
- uppercase/lowercase 大小写的转换

> 练习：实现一个学生数组（5条），每一个学生都有:name age sex score
	处理：按成绩前三名显示在table中。(排序：limitTo)

```
<!DOCTYPE html>
<html ng-app="myApp">
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="js/angular.js"></script>
</head>
<body>
<div ng-controller="myCtrl">
    <table>
        <thead>
        <tr>
            <th>名称</th>
            <th>年龄</th>
            <th>性别</th>
            <th>成绩</th>
        </tr>
        </thead>
        <tbody>
        <tr ng-repeat="stu in stuList | orderBy:'score':true | limitTo:3">
            <td>{{stu.name}}</td>
            <td>{{stu.age}}</td>
            <td>{{stu.sex}}</td>
            <td>{{stu.score}}</td>
        </tr>
        </tbody>
    </table>
</div>
<script>
    var app = angular.module('myApp', ['ng']);
    app.controller('myCtrl', function ($scope) {
        $scope.stuList = [
            {name: 'lily', age: 20, sex: 1, score: 80},
            {name: 'michael', age: 22, sex: 2, score: 88},
            {name: 'finch', age: 24, sex: 1, score: 83},
            {name: 'hanmeimei', age: 28, sex: 1, score: 82},
            {name: 'lucy', age: 30, sex: 1, score: 81}
        ];
    });
</script>
</body>
</html>
```
3. 自定义过滤器
> 自定义过滤器的本质 就是定义一个方法（有参数和右返回值）

```
var app = angular.module()
app.filter('过滤器名称'，function(){
	return function(inputValue,arg1。。){
		return '处理后的数据'
	}
});
```
```html
<!DOCTYPE html>
<html ng-app="myApp">
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="js/angular.js"></script>
</head>
<body>
<div ng-controller="myCtrl">
    <p>{{100 | myFilter:'￥'}}</p>
</div>
<script>
    var app = angular.module('myApp', ['ng']);
    //自定义过滤器
    app.filter('myFilter', function () {
        //过滤器的本质就是方法
        return function (inputValue, arg1) {
            return arg1 + inputValue
        }
    });
    app.controller('myCtrl', function () {
        console.log('ok');
    });
</script>
</body>
</html>
```

4. 常用函数

angular.toJson/fromJson:json格式的序列化（将一个对象或者数组序列化为json格式的字符串）、反序列化（..）

angular.forEach(集合名称，function(value,key){})

angular.uppercase/lowercase

```html
<!DOCTYPE html>
<html ng-app="myApp">
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="js/angular.js"></script>
</head>
<body>
<div ng-controller="myCtrl"></div>
<script>
    var app = angular.module('myApp', ['ng']);
    app.controller('myCtrl', function ($scope) {
        //json格式的反序列化
        var jsonStr = '{"name":"zhangsan","age":20}';
        var student = angular.fromJson(jsonStr);
        console.log(student);
        //将对象或者数组 序列化 json格式的字符串
        var obj = {price: 20, count: 100};
        var productStr = angular.toJson(obj);
        console.log(productStr);
        //大小写转换
        var name = 'Michael';
        console.log(angular.uppercase(name));
        //集合的操作 forEach
        var list = [100, 200, 300, 400];
        angular.forEach(list, function (value, key) {
            console.log(
                'key is ' + key + " value is " + value);
        });
    });
</script>
</body>
</html>
```

## 服务
> 服务的本质在ng中 就是一个对象，可以提供数据和方法。<br>
服务有很多:$location/$http/$scope/$rootScope/$window/$interval/$timeout...

**服务的用法**：只需要在对应的ng对象中注入对应的服务，可以在ng对象中调用服务中封装好的方法和数据了
```
app.controller('myCtrl',function($location,$scope){
		//调用服务中封装好的方法、数据
	})
```
```html
<!DOCTYPE html>
<html ng-app="myApp">
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="js/angular.js"></script>
</head>
<body>
<!--点击按钮，获取到当前的端口号 显示在视图中-->
<div ng-controller="myCtrl">
    <h1>{{nowPort}}</h1>
    <button ng-click="handleClick()">
        clickMe
    </button>
</div>
<script>
    var app = angular.module('myApp', ['ng']);
    app.controller(
        'myCtrl',
        function ($scope, $location) {
            $scope.handleClick = function () {
                //获取当前的端口号
                $scope.nowPort = $location.port();
            }
        });
</script>
</body>
</html>
```

> #### 面试题：$scope和$rootScope的关系
```
1.每一个控制器对应的有一个$scope,每一个$scope都是相互隔离的
2.$scope的id是根据控制器的加载顺序是递增的，有共同的父元素$rootScope
：总的说就是父与子的关系
```

> #### 控制器之间数据如何传输?

1. 可以把要共享的数据或者方法 放在$rootScope.(在子对象中 可以 读取 父对象中的数据或者方法).
2. 借助于控制器之间的嵌套
```
div	ng-controller='myCtrl02'
    div	ng-controller='myCtrl03'
```
```html
<!DOCTYPE html>
<html ng-app="myApp">
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="js/angular.js"></script>
</head>
<body>
<!--调用第一个控制器-->
<div ng-controller="myCtrl01">
    <p>{{num}}</p>
    <p>{{"myCtrl01 name is "+name}}</p>
</div>
<!--调用第二个控制器-->
<div ng-controller="myCtrl02">
    <p>{{num}}</p>
    <p>{{"myCtrl02 name is "+name}}</p>
    <!-- 将myCtrl03嵌套在了myCtrl02中-->
    <div ng-controller="myCtrl03">
        <p>{{"myCtrl03 age is "+age}}</p>
    </div>
</div>
<script>
    var app = angular.module('myApp', ['ng']);
    //控制器 myCtrl01
    app.controller(
        'myCtrl01',
        function ($scope, $rootScope) {
            $scope.num = 10;
            console.log($scope);
            console.log($rootScope);
            $rootScope.name = 'Tarena';
        });
    //控制器 myCtrl02
    app.controller('myCtrl02', function ($scope) {
        console.log($scope);
        $scope.age = 20;
    });
    //控制器 myCtrl03
    app.controller('myCtrl03', function ($scope) {
    })
</script>
</body>
</html>
```

3. 借助于事件
    - 事件的绑定：$scope.$on('eventName',function(event,data){})
    - 事件的触发：
        - 由父向子 触发：$scope.$broadcast('eventName',data);
        - 由子向父 触发：$scope.$emit('eventName',data);

```html
<!DOCTYPE html>
<html ng-app="myApp">
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script src="js/angular.js"></script>
</head>
<body>
<div ng-controller="myCtrl01">
    <button ng-click="sendToChild()">
        向子控制器传值
    </button>
    <p>{{"儿子的回话："+msgFromSon}}</p>
    <hr/>
    <div ng-controller="myCtrl02">
        <h1>{{"接收到的数据为"+msgFromFather}}</h1>
        <button ng-click="sendToFather()">
            向父控制器传值
        </button>
    </div>
</div>
<script>
    var app = angular.module('myApp', ['ng']);
    app.controller('myCtrl01', function ($scope) {
        //向子对象传值
        $scope.sendToChild = function () {
            $scope.$broadcast(
                'fromFather',
                '小子 你在干嘛'
            );
        };
        //绑定一个事件，用来接收子对象带来的参数
        $scope.$on('fromSon', function (event, data) {
            console.log(data);
            $scope.msgFromSon = data;
        });
    });
    app.controller('myCtrl02', function ($scope) {
        //绑定一个事件，是用来接收父级对象传值的
        $scope.$on(
            'fromFather',
            function (event, data) {
                console.log(data);
                $scope.msgFromFather = data;
            });
        //定义方法，用来向父控制器传值
        $scope.sendToFather = function () {
            //触发事件
            $scope.$emit('fromSon', '我们正在刻苦的学习');
        }
    });
</script>
</body>
</html>
```

### 练习
> 全选或者取消全选

> 使用ng的双向数据绑定

1. 构造对象数组
2. 显示出来
3. 实现部分选中

> 逻辑与运算

上边两个复选框选中，全选就被选中

上边两个复选框只要有一个没被选中，全选就被取消

4. 实现全选复选框

点击全选，上边两个复选框被选中

取消全选，上边两个复选框取消选中
