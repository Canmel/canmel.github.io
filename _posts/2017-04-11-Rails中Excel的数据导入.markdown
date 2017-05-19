---
layout: post
title: "Rails中Excel的数据导入"
subtitle: ""
date: 2016-05-20 21:11:27
author: baily
---

#### Rails中Excel的数据导入
* 从界面开始
	* 首先创建一个表单

	* ```ruby
      <%= bootstrap_form_tag url: '/imports' , :html => {:id => 'import_car_form', :multipart => true} do |f| %>
	      <input type="text" size="50" id="upfile" name="upfile" readonly>
	      <%= f.file_field :file, class: "hidden", hide_label: true, :onchange => "upfile.value=this.value", :required => 'true'%>
	      <a href="javascript:_file.click();" class="btn btn-sm btn-success"><span class="glyphicon glyphicon-plus">选择文件</span></a>
	      <%= f.submit "上传", class: "btn btn-sm btn-primary" %>
      <% end %>
	  ```
* Gemfile
	* ```ruby
       gem 'roo ~>2.7.1'
	  ```
	* ```ruby
	   bundle install
	  ```

* routes

	* ```ruby
	   resouces: imports
	  ```
* controller
	* controller中建立一个create方法
	* 在参数中获取临时文件，参数 `file`

	* ```ruby
        # rails默认的路由之一
        # 请求方式post
        def create
            tempfile = params[:file]
            require 'util/excel_util'
            if ExcelUtil.file_present? tempfile
                format = {name: "名称", age: "年龄", phone: "手机"}
                results = ExcelUtil.analysis upload_file, format
                # 在这里就获取到了结果集
                # 可以对数据进行持久化操作等
                # 但是得注意多数据操作的事务添加
            end
        end
	  ```

* ExcelUtil

	* ```ruby
		class ExcelUtil
			# 针对单行表头的excel表导入
			# 默认是使用excel的第二行开始计算有效数据，第一行为表头信息
			# upfile: 上传的临时文件
			# format: 定义的表头信息
			#   格式为： {property1: "msg1", property2: "msg2"}
			# 	eg: format = {name: "名称, price: "价格", num: "数量"}
			# 返回值： json数组
			#   格式为： [{name: "土豆"， price: "3", num: "4"}，{name: "里脊肉"， price: "12", num: "1"}]
			# ==========================================================
			# 调用之前请先确认文件存在与正确性可调用 file_present? == true
			def analysis upfile, format
			   require 'roo'
  			   xlsx = Roo::Spreadsheet.open(file, extension: :xlsx)
  			   result = []
  			   xlsx.each_with_pagename do |name, sheet|
		          sheet.each do |hash, index|
		             result << hash if index != 0
           		  end
		       end
	           result
			end
			# 判断文件是否存在或者符合要求
			# 必须是以xlsx或xls结尾的文件
			def file_present? file
		      return false if file.nil?
		      if file.instance_of? ActionDispatch::Http::UploadedFile
		        original_name = file.original_filename
		      else
		        original_name = file.path
		      end
		      suffix = original_name.split('.').last
		      return suffix === "xls" || suffix === "xlsx" ? true : false
		    end
			# TODO 比较复杂的excel的导入（多行表头）
		end
	  ```