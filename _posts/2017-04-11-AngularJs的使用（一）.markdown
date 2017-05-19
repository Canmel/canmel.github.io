---
layout: post
title: "angularJs的使用（一）"
subtitle: ""
date: 2016-05-20 21:11:27
author: baily
---



#### 使用angularJs （一）

### 环境
- 后台支持： Java , springMVC, mybatis, mysql
- 前端框架： Jquery , BootStrap , AngularJs

### 使用
1. 创建app.js
* 创建ng-app
    * 在标签上添加`ng-app`属性
        * 如： `<body ng-app='meedesidy'></body>`这样就创建了一个名称为`meedesidy`的angular项目
* 设置路由
	* `app.js`中设置路由，一般都是在spa(single page application 单页面项目)中使用angularJs,这里配置的是在主页面下的子页面的路由
	* 代码：

	* ```javascript
		meedesidy.config(['$stateProvider', '$urlRouterProvider', function($stateProvider, $urlRouterProvider) {

			//	默认路由
			$urlRouterProvider.otherwise('index');

			//	配置基本路由
			$stateProvider.state('index', {
				url: '/index',
				templateUrl: 'pages/index.jsp'
			}).state('user', {
				url: '/user',
				templateUrl: 'pages/user/index.jsp',
				controller: 'user'
			})；
		}]);
	  ```
	* 当angular找不到指定的路由会去寻找指定的路由，这里设置的是 index
	* 当访问`localhost:8080/project/#/user`会去渲染加载`user/index.jsp`这个模板,并且会设置当前的控制器名称为`menu`
	* 设置了控制器名称，必须要加载相应的控制器,才能在相应的控制器中控制user/index.jsp中的内容
* 设置控制器
	* 在javascript文件夹下创建controller文件夹，并在controller文件夹下创建`userController.js`文件
	* 在controller文件中添加controller

	* ```javascript
		// 在app-name 下定义一个 name为 user 的controller
		meedesidy.controller('user', ['$scope', '$http', '$state', '$stateParams', function($scope, $http, $state, $stateParams) {


		}]);
	  ```

* HTTP请求
	* http请求需要注入`$http`服务
	* 请求：
	* ```jacascript
        $http({
            method: "post",	// 发送一个post请求
            url: "/xxx/user/pageQuery",
            data: $.param($scope.searchParams),	// 参数，可表达式
            headers : { 'Content-Type': 'application/x-www-form-urlencoded' } //header可选，头部信息
        }).then(function(resp){		//请求成功回调 参数是response
            $scope.datas = resp.data.result;
        }),(function(resp){			//请求失败的回调 参数是response
            console.info(resp);
        });


      ```

* 请求获取到了数据现在就可以在界面上显示了