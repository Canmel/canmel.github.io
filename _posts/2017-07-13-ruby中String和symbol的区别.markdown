---
layout: post
title: "ruby中String和symbol的区别"
author: "meedesidy"
meta: "Springfield"
---

## ruby中String和symbol的区别
### Symbol
- 定义：Ruby 是一个强大的面向对象脚本语言（本文所用 Ruby 版本为1.8.6），在 Ruby 中 Symbol 表示“名字”，比如字符串的名字，标识符的名字。
- 创建：创建一个 Symbol 对象的方法是在名字或者字符串前面加上冒号：
    ```irb
    irb(main):001:0> :foo
    => :foo
    irb(main):002:0> :test
    => :test
    irb(main):004:0> :"foo is string"
    => :"foo is string"
    ```
### String
* 定义：Ruby 中的 String 对象用于存储或操作一个或多个字节的序列。
* 创建：Ruby 字符串分为单引号字符串（'）和双引号字符串（"），区别在于双引号字符串能够支持更多的转义字符。
    ```irb
    irb(main):007:0> "这是一个字符串"
    => "这是一个字符串"
    irb(main):008:0> '这也是一个字符串'
    => "这也是一个字符串"

    ```
### 区别
- 因为在 Ruby 中字符串也是一种对象，即 String 对象。无论其结构还是操作和 Symbol 对象都是不同的。
- 在 Ruby 中每一个对象都有唯一的对象标识符（Object Identifier）,可以通过 object_id 方法来得到一个对象的标识符

    - Symbol

    ```irb
    irb(main):009:0> :foo.object_id
    => 1149148
    irb(main):010:0> :foo.object_id
    => 1149148
    irb(main):011:0> :"foo".object_id
    => 1149148

    ```

    - String

    ```irb
    irb(main):014:0* "foo".object_id
    => 47353958516800
    irb(main):015:0> "foo".object_id
    => 47353958510640
    irb(main):016:0> "foo".object_id
    => 47353958500960

    ```

   - 从上面可以看出，　前三句都是同一个Symble对象，后三个都是不同的String对象
   - 可见，每个 String 对象都是不同的，即便他们包含了相同的字符串内容；而对于 Symbol 对象，一个名字（字符串内容）**唯一确定** 一个 Symbol 对象。

   - *值得注意的是创建 Symbol 对象的字符串中不能含有’\0’字符，而 String 对象是可以的。*
    ```irb
    irb(main):001:0>  :"fo\0o"
    SyntaxError: compile error
    (irb):1: symbol cannot contain '\0'
            from (irb):1
    irb(main):002:0> :"foo\0"
    SyntaxError: compile error
    (irb):2: symbol cannot contain '\0'
            from (irb):2
    irb(main):003:0> puts "foo\0".object_id
    24305140
    => nil
    irb(main):004:0> puts "fo\0o".object_id
    24301000
    => nil
    irb(main):005:0>
    ```
   - 经过实际测试发现并不是这样(可能是因为ruby版本不一致，测试版本为 2.3.1)

   ```irb
   irb(main):002:0> :"fo\0o"
   => :"fo\x00o"
   irb(main):003:0> :"foo\0"
   => :"foo\x00"
   irb(main):004:0> puts "foo\0".object_id
   47315761608520
   ```

- 除了可以采用一般的字符串，还可以使用操作符（例如+, -, *, /），变量，常量，方法甚至类的名字来创建 Symbol 对象，例如:+就是一个合法的 Symbol 。实际上，在 Ruby 内部操作符、变量等名字本身就是作为 Symbol 处理的，例如当你定义一个实例变量时， Ruby 会自动创建一个 Symbol 对象，例如 @test 对应为 :@test 。
    ```ruby
    # 实例变量的 Symbol

    class Test
     attr_accessor :test
    end
    ```
- 这个类定义了一个具有读写方法的实例变量 @test 。实际上 Ruby 创建了两个 Symbol ，一个是实例变量的 symbol :@test ，另一个是 :test 。那如果使用字符串对象 ”test” 作为参数呢？也可以，仍然会创建两个 symbol ，:test 和 :@test ，为什么还会创建 :test 呢？这是和Ruby的实现相关的（至少Ruby1.8.6里是这样）。

- 注意，**类变量 @@test 和实例变量 @test 对应的 Symbol 显然是不同的。记住：名字相同，则Symbol 相同**。 名字相同， Symbol 相同

    ```ruby
    class Test
      puts :Test.object_id
      Test = 10
      puts :Test.object_id

      def Test
        puts :Test.object_id
      end
    end

    Test.new.Test
    ```
    运行结果

    ```ruby
    224298
    224298
    224298
    ```

- 名字不同， Symbol 不同
    ```ruby
    class Test
      puts :Test.object_id

      @@test = 10
      puts :@@test.object_id
      def test
        puts :test.object_id
        @test = 10
        puts :@test.object_id

      end
    end

    t =Test.new
    t.test
    ```
运行结果
```ruby
224298
288068
79858
288108
```

- **Symbol 表示一个名字，仅此而已**
- **Symbol 对象一旦定义将一直存在，直到程序执行退出**

    > 可以通过类方法 Symbol.all_symbols 得到当前 Ruby 程序中定义的所有 Symbol 对象，该方法返回一个 Symbol 对象数组

### String 和　Symbol
- String 类有很多方法，Symbol类只有all_symbols()的类方法和７个实例方法
- 虽然 Symbol 和 String 是不同的对象，但它们之间关系很密切。 Ruby 提供了方法在 Symbol和 String 之间转换。

#### Symbol 转化为 String
- Symbol 到 String
    ```irb
    irb(main):001:0> :test.id2name
    => "test"
    irb(main):002:0> :test.to_s
    => "test"
    irb(main):003:0> :"I am a boy".to_s
    => "I am a boy"
    ```
- **注意，每个 String 对象都是唯一的，因此对一个 Symbol 调用多次将产生多个 String 对象。**

#### String 转化为 Symbol
如果该 Symbol 已经存在，则直接返回。
- String 到 Symbol
```irb
irb(main):001:0> var1 = "test".to_sym
=> :test
irb(main):002:0> var2 = "test".intern
=> :test
irb(main):003:0> var1 == var2
=> true
```

### 使用 Symbol
- Symbol本质上是 Ruby 符号表中的东西, 使用Symbol可以降低ruby内存消耗，提高执行速度
- 通常来讲，当你面临 String 还是 Symbol 的选择时，可以参考以下标准：

  > - 如果使用字符串的内容，这个内容可能会变化，使用 String
  > - 如果使用固定的名字或者说是标识符，使用 Symbol,比如枚举值、关键字（哈希表关键字、方法的参数）
#### Hash中使用
- 使用 Symbol 作为 key
    ```ruby
    hosts = {
        :beijing => 'machine1',
        :shanghai => 'machine2',
        :guangzhou => 'machine3',
        :tianjin' => 'machine4',
        :shenzhen => 'machine5'
        }
    ```
- 使用　String 作为 key
    ```ruby
     hosts = {
        "beijing" => 'machine1',
        "shanghai" => 'machine2',
        :guangzhou" => 'machine3',
        "tianjin" => 'machine4',
        "shenzhen" => 'machine5'
        }
    ```
- **每次获取hosts值的时候使用hosts[:beijing],不会创建新的对象。**
- **每次获取hosts值的时候使用hosts["beijing"],每次都会创建新的String对象，造成多余的内存和性能开销**

### 哈希参数
- 通常我们定义的函数的参数的个数和顺序是写死的，调用函数的时候要确保参数的个数、顺序匹配，有时候这样很不方便，使用哈希参数可以解决这个问题。
- ROR 中就大量地运用这种方式，也许你已经看到了，到处都是 Symbol 和哈希

使用哈希参数的方法调用
    ```ruby
    link_to 'Show', :action => 'show', :id => product
    add_column :products, :price, :decimal, :precision => 8, :scale => 2, :default => 0
    ```