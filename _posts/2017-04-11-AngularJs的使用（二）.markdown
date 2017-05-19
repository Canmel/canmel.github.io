---
layout: post
title: "angularJs的使用（二）"
subtitle: ""
date: 2016-05-20 21:11:27
author: baily
---

#### 使用angularJs （一）
### 环境
- 后台支持： Java , springMVC, mybatis, mysql
- 前端框架： Jquery , BootStrap , AngularJs

### 使用
* 一般AngularJs 应用在一个单页面的web项目中，一般结构为
	* head
		* js和css文件
	* body
		* header
		* sidebar(菜单部分)
		* main-content
		* footer
* 一般header和footer都是固定的,点击菜单（sidebar）中的菜单，页面（main-content）部分做出相应的改变，本文仅围绕这个模式书写
* 在`main-content`上设置属性`ui-view`
	* `ui-view`是告诉angularJs,今后更改页面，刷新页面，就在这个属性所在的标签下，在你还没有告诉angular要去哪个页面的时候，angular会去路由中找到默认路由去渲染
* 左侧菜单设置设置相应的`controller`
	* 新建`sidebarController.js`
	* 建立go方法
		* ```javascript
         meedesidy.controller('sidebar', [ '$scope', '$http', '$state', '$stateParams', function($scope, $http, $state, stateParams) {
                 //调用go方法跳往指定页面，在ui-view下显示
                 $scope.go = function($event, url) {
                     $state.go(url);
                 }
         }]);

          ```

* 在菜单中设置ng-click点击事件
	* ```html
        <a ng-click="go($event, '/users')">用户菜单</a>

      ```

	* 这样在页面中点击了`用户菜单`在`ui-view`中就可以看到路由中定义的templateUrl，即`pages/user/index.jsp`

	* 相应的设置其他路由，即可显示其他界面

* angularJs中的子路由
	* 点击每一个菜单会相应的改变`ui-view`中的jsp,在jsp中会有属于各页面的路由，如：user.jsp中会有`新建用户`的链接，这就需要一个新的子路由，下面开始创建子路由
	* 子路由的创建也很简单，之前配置路由的时候会在每个路由下设置一个`controller`的属性.
	* ```javascript
		.state('user', {
			url: '/user',
			templateUrl: 'pages/user/index.jsp',
			controller: 'user'
	  ```
	* 该属性表明：在当前路由下的页面（现在就是jsp）,使用另外一个controller,这个controller的名字就叫user
	* 每一次在`xxxController.js`中设置controller的时候，都会设置它的名字，第一个参数就是
	* ```javascript
		meedesidy.controller('user', ['$scope', '$http', '$state', '$stateParams', function($scope, $http, $state, $stateParams) {
	  ```
	* 这样在就将页面和新建的controller连在一起了，可以在controller中操作页面中的内容

	* 在`userController.js`中创建子路由
	* ```javascript
		meedesidy.config(['$stateProvider', '$urlRouterProvider', function($stateProvider, $urlRouterProvider) {
		//	配置基本路由
			$stateProvider.state('newUser',{
				url: '/user/new',
				templateUrl: 'pages/user/new.jsp',
				controller: 'user'
			}).state('editUser',{
				url: '/user/edit/{id}',
				templateUrl: 'pages/user/new.jsp',
				controller: 'user'
			});
		}]);
	  ```
	* 这样在user.jsp中就多了几个子路由：`newUser`和`editUser`

* 子路由的使用
	* 子路由的使用和基本路由一样，不论是超链接还是使用go方法跳转，都一样使用
	* 你可以这样使用：

	* ```html
		<a ui-sref="newUser">新建用户</a>
	  ```
	* 也可以这样用：
	* ```
		<a ng-click="go(newUser)">新建</a>
	  ```
	* 然后建立相应的go方法

### 到这里angularJs的基本使用就差不多了，剩下的就是一些使用了，但关键就是一个数据的双向绑定
