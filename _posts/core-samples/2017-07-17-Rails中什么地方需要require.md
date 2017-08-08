---
layout: post
title: "Rails中什么时候需要require"
author: "meedesidy"
meta: "Springfield"
---

## Rails中什么时候需要require

**`Scene`**  在现在的ROR开发中，比如在用户的控制器中使用下面一段代码：
```ruby
class UsersController < ApplicationController
  def test
    p User.name
  end
end

```
**`Qustion`** 然而在`Controller`中并没有任何的`require`或者`import`或者`include`等动作，那`User`类是怎么加载到`controller`中的呢？

### 整理这个问题的时候,我顺便找到了关于 [Ruby 内核类加载机制 ](http://www.baidu.com)

#### $LOAD_PATH

- 在`Rails`项目中有很多第三方库，如果要对第三方库和业务代码进行管理，即`$LOAD_PATH`
- `Ruby`并不要求统一管理类，理论上我们的类文件可以遍布整个系统各个角落，意味着我们需要为每个文件制定一个绝对路径活着相对路径
- `$LOAD_PATH`是一个字符串数组，元素是一堆路径
    ```irb
    irb(main):001:0> $:
    => ["/home/baily/.rbenv/plugins/rbenv-gem-rehash", "/home/baily/.rbenv/rbenv.d/exec/gem-rehash", "/home/baily/.rbenv/versions/2.3.1/lib/ruby/gems/2.3.0/gems/did_you_mean-1.0.0/lib", "/home/baily/.rbenv/versions/2.3.1/lib/ruby/site_ruby/2.3.0", "/home/baily/.rbenv/versions/2.3.1/lib/ruby/site_ruby/2.3.0/x86_64-linux", "/home/baily/.rbenv/versions/2.3.1/lib/ruby/site_ruby", "/home/baily/.rbenv/versions/2.3.1/lib/ruby/vendor_ruby/2.3.0", "/home/baily/.rbenv/versions/2.3.1/lib/ruby/vendor_ruby/2.3.0/x86_64-linux", "/home/baily/.rbenv/versions/2.3.1/lib/ruby/vendor_ruby", "/home/baily/.rbenv/versions/2.3.1/lib/ruby/2.3.0", "/home/baily/.rbenv/versions/2.3.1/lib/ruby/2.3.0/x86_64-linux"]
    irb(main):002:0>
    ```
- 这样就可以很方便对类库进行管理，和新类库的添加
    > $:.unshift File.dirname(__FILE__) //添加新路径
      
#### Ruby Kernel 中的类加载

**Ruby 内核提供了 4 个类加载命令，分别是 load, autoload, require, require_relative, 分别对应了不同的使用场景**
      
- load(filename, wrap=false):
 
    > 每次重新加载整个文件，成功返回true,失败抛出异常，wrap为true时，被加载文件会在一个匿名模块中执行，避免污染；
- autoload(module, filename):

    > 将filename与module关联，当第一次使用module时，使用require加载该文件;成功返回nil,失败抛出异常

- require(name):
    > 函数执行时，如果name是绝对路径，则会去查找该文件；通常name是相对路径，需要$:.unshift添加搜索路径；,Ruby会在$:中的目录中搜索该文件,第一次加载返回true，已经加载返回false，找不到文件会抛出LoadError

- require_relative(filename):
    > 直接取相对路径。此时与$LOAD_PATH($:)无关，是文件本身路径的相对地址    

### ActiveSupport 对内核类加载的扩展

**Active Support 是 Rails 裡的工具函式庫，它也擴充了一些 Ruby 標準函式庫。除了被用在 Rails 核心程式中，你也可以在你的程式中使用**

#### autoload 扩展
**除了`ActiveSupport`对gem的扩展，在`Rails`中，维护了一个类似`$LOAD_PATH`的变量`autoload_paths`**

- Rails 3 中默认会将 `app` 下的子目录以及 lib 目录全部加入 `autoload_paths`, Rails 4 中去掉了 lib. 
- 以下是一个刚生成的 Rails 3 项目的 `autoload` 路径:

    ```
    $ bin/rails r 'puts ActiveSupport::Dependencies.autoload_paths'
    .../app/assets
    .../app/controllers
    .../app/helpers
    .../app/mailers
    .../app/models
    .../app/controllers/concerns
    .../app/models/concerns
    .../test/mailers/previews
    .../lib
    ```

- 在 Rails 中还可以添加自定义的 autoload 路径:
    
    ```ruby
    # config/application.rb
    config.autoload_paths << "#{Rails.root}/something"
    ```
    
**`autoload_paths` 实际上是在 `ActiveSupport::Dependencies.autoload_paths` 中定义的, 这是一个 `String Array`, 可见 `Rails` 的 `autoload` 本质上是 `ActiveSupport` 的 `autoload` 机制**

- 以下代码将当前目录加入`autoload_paths`, 这样在`Ruby`找不到某个常量定义的时候，`ActiveSupport`就会尝试找到常量定义文件并自动加载.
    ```
    ActiveSupport::Dependencies.autoload_paths << '.'
    ```

- **到这里`rails`项目中就可以在业务代码中互相访问了, 在`autoload_path`中没有包含的路径，则需要添加**

- 查询`autoload_path`
    > bin/rails r 'puts ActiveSupport::Dependencies.autoload_paths' # 查询autoload_path

- 添加`autoload_path`
    > config.autoload_paths << "#{Rails.root}/something" # 添加autoload_path

- 也可以在代码中添加`require`
    > require '../test.rb' # 如果name是绝对路径，则会去查找该文件；通常name是相对路径，需要$:.unshift添加搜索路径；
    
**`Answer`：**

- `User`加载到`UsersController`是通过`Rails`的扩展库`ActiveSupport`维护的一个类似`$LOAD_PATH`的一个变量`autoload_path`管理文件的`load`之类的行为,他默认路径包含`app/`之下的几个文件夹(受rails版本，可能不相同)，`/controller`和`model`在其中，所以可以在`controller`中访问`model`信息

- 也就是说在`rails3`以下路径下都是不用`include`, `require`, `import`这类行为，就可以使用
    ```
    .../app/assets
    .../app/controllers
    .../app/helpers
    .../app/mailers
    .../app/models
    .../app/controllers/concerns
    .../app/models/concerns
    .../test/mailers/previews
    .../lib
    ```
- 而在`autoload_path`中不包含的路径就需要手动`require`,像这样：
    ```ruby
    ## Rails 5
    class UsersController < ApplicationController::Base
      # rails的autoload没有lib的路径
      # $LOAD_PATH中 $: 中有lib的路径，所以不需要require全路径
      require 'util/myutil'
    end
    ```
    