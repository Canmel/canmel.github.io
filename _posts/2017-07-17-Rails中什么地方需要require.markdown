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

