
>`函数式编程`：是一种思维  
现在更常用的还是`面向对象`的思维  
`闭包`是函数式编程的一种体现

## 闭包(与变量的作用域有关)

python一切皆对象：python中只有类和对象  

而在其他语言中还有很多，比如C 中有结构体，枚举，函数，过程等等，这还是对C++的一种简化，C++更加复杂

    def curve_pre():
        def curve():     #函数里面可以嵌套函数
            print('This is a function')
        return curve     
        #函数可以作为返回的结果,因为函数也是一个对象，注意这里是curve_pre()返回curve函数
    C = curve_pre()  
    #可以把函数赋值给一个变量，注意区分实例化的过程，现在这个C就是返回的curve函数
    C()     #因为现在C就是一个函数，可以用函数的调用方式
    #输出：This is a function
####
    def curve_pre():
        a= 10
        def curve(x):
          return a*x*x
        return curve
    C = curve_pre()
    print(C(2))
    #输出：40
    #在curve()里如果找不到a，就会去上一层去找，直到找到为止

但如果在外部更改a的值，输出的C(2)会不会改变呢

    def curve_pre():
        a= 10
        def curve(x):
            return a*x*x
        return curve
    a = 20
    C = curve_pre()
    print(C(2))
    #还是输出40 -------a的值还是从curve_pre()中取的，这就是闭包的现象

`闭包`：是由函数以及它在定义时候的环境变量所构成的一个整体

    print(f.__closure__)     #环境变量a被储存在了f.__closure__这个内置的变量中
    print(f.__closure__[0].sell_contents)     #以取出变量a

举例：

     def f1():
         a = 10
         def f2():
             a = 20    #此时的a是局部变量
             print(a)    #注意这里f2()并没有被执行，所以不会输出内容
         print(a)        #这里输出了10，因为f2()还没有被执行，所以输出还是原来的10
         f2()            #这里执行了f2()，所以a的值变化了
         print(a)        #关键是这里，重新打印a，a的值还是变为原来的10，因为f2中的是局部变量a,不能影响到外部的变量
    f1()
    #输出：10   20   10
思考：此时是闭包吗
>不是的，可以操作看看

     def f1():
         a = 10
         def f2():
             a = 20     #仅仅是比上面的curve多了这一步
                        #是不是闭包和有没有返回结果没有关系，可以加上return a试试
         return f2
    f = f1()
    print(f.__closure__)
    #输出：None

    #把f2()里的a = 20去除后发现，print(f.__closure__)有值了
    #在f2()里的a被认为是局部变量，那就和外面的a没有关系了，并没有引用外部的环境变量a，所以不是一个闭包
    #所以千万不要认为在f2中出现了a就是一个闭包，必须要是引用了外部环境变量a才是一个闭包，比如 c = a + 2

要求以闭包的形式保存走的步数，并可以累加  

以非闭包的方式：

    x = 0
    def step(a):
        global x
        m = x + a
        x = m
        return x
    print(step(10))   #注意这里不能把函数赋给S
    print(step(10))
    #输出：10  20
    print(x)
    #输出：20   -------全局变量也跟着变了


以闭包的方式： 函数式编程，不太常用；还是面向对象的思维更符合

    x = 0
    def step(x):
        def step_up(a):
            nonlocal x     #nonlocal可以定义一个变量为非局部变量，注意这里用global会改变全局变量
            m = x + a
            x = m
            return m
        return step_up
    S = step(x)
    print(S(10))
    print(S(20))
    #输出：10  30
    #闭包的好处是什么:
    print(x)
    ?输出：0 ---- nonlocal没有改变全局变量
>但也有不好的地方：环境变量是常驻于内存空间里的，所以很容易造成内存泄露，如果是JavaScript里，很容易造成浏览器的卡顿


## 匿名函数
>顾名思义，在定义函数的时候，不需要给函数命名
> 
>格式：`lambda 参数列表: 表达式`(不能是代码语句，如a = x + y)

例：

    lambda x,y: x + y
对比：

    def f(x,y):
        return x + y   #匿名函数不需要return

### 调用匿名函数

    f = lambda x,y: x + y    #把匿名函数赋值给变量
    print(f(1,2))
    #不过这个举例毫无意义，下面看实际应用

x>y时输出x,否则输出y  
**三元表达式：**
>其他大部分语言用三元表达式会这么表示：`x > y ? x : y`  
Python用三元表达式会这么表示：`x if x > y else y`  
格式：`输出值A if 条件 else 输出值B`


## map类
>格式：`map(函数，列表)`  
>
> 输出要转为`list`格式
> 
>`map类`可以让列表里的每个元素都执行一次输入的函数

例：要对列表中的每一个元素都执行某个函数，并返回一个新列表

    list_x = [1,2,3,4,5,6,7,8]
    def square(x):
        return x ** 2

方法一：用for循环

    squared_list = [square(x) for x in list_x]  
    print(squared_list) 

方法二：用map类

    r = map(square,list_x)  
    print(r)
    #输出：<map object at 0x000001E974411270>
    print(list(r))
    #输出：[1, 4, 9, 16, 25, 36, 49, 64]

## map与lambda
例1：

    list_x = [1,2,3,4,5,6,7,8]
    r = map(lambda x: x ** 2, list_x)
    print(list(r))
    #输出:[1, 4, 9, 16, 25, 36, 49, 64]
例2：

    list_x = [1,2,3,4,5,6,7,8]
    list_y = [1,2,3,4,5,6,7,8]
    r = map(lambda x,y: x ** 2 + y, list_x, list_y)    
    #传入列表的个数必须和传入参数的个数相同
    print(list(r))
    #输出:[2, 6, 12, 20, 30, 42, 56, 72]
例3：如果列表不是对称的呢

    list_x = [1,2,3,4,5,6]     
    list_y = [1,2,3,4,5,6,7,8]
    r = map(lambda x,y: x ** 2 + y, list_x, list_y)
    print(list(r))
    #输出:[2, 6, 12, 20, 30, 42]
    #不会报错，但输出的列表的元素个数取决于传入的列表里元素较少的那个
- lambda并不会提升代码运行效率，只会让代码更简洁

## reduce函数
>归约，用于并行计算

例:

    from functools import reduce     
    #reduce函数在functools模块里，不是系统函数，不能直接用

    list_x = [1,2,3,4,5,6,7,8]
    r = reduce(lambda x,y: x+y,list_x)     #reduce下的lambda必须传入两个参数
    print(r)
    #输出：36    
明明没有输入`y`，是怎么进行运算的？
>不难看出其实结果`36`是`list_x`里每个元素相加的结果

>`reduce函数`会连续计算，也就是连续调用`lambda`，第一次把第一个元素`1`放入，发现`lambda`缺少一个参数不能运行，然后把第二个元素`2`放入，就能运行了，得到`3`，再将第三个元素放入，与得到的结果`3`计算，得到`6`，再将第四个元素`4`放入，与得到的结果`6`计算，得到`10`......  
> 
>也就是：`((((((1+2)+3)+4)+5)+6)+7)+8`  
> 
>特点是会把运算的结果作为参数继续参与运算

### reduce的第三个参数
>将第三个参数作为初始值，也就是把`10`作为第一个元素参与运算  
> 默认初始值为`0`

    r = reduce(lambda x,y: x+y,list_x,10)
    #这里的10相当于把10放入list_x作为第一个元素，再放入1，2.....
    print(r)
    #输出：46


## filter类 
>过滤，用于筛选  
> 表达式里要返回布尔值

例:三元表达式

    list_x = [1,2,3,4,5,6,7,8]    #列表里元素个数要大于2
    r = filter(lambda x: True if x % 2 == 0 else False, list_x)    
    #lambda里的表达式要返回布尔值(或者能表示真假的值)，这里用三元表达式

    print(list(r))  #和map一样，输出格式为一个集合，记得转换为列表
    #输出：[2, 4, 6, 8]
简化：

    list_x = [1,2,3,4,5,6,7,8]
    r = filter(lambda x: x % 2 == 0, list_x)    #如果返回的是False，将会从列表中剔除
    print(list(r))
    #输出：[2, 4, 6, 8]

## 命令式编程vs函数式编程

>函数式编程:`filter`，`reduce`，`map`，`lambda`...
>   
>命令式编程:无论怎样的业务，都能通过`def,if else,for`这个流程实现

- 函数式编程的鼻祖：`lisp`


## 装饰器
>原则：对修改是封闭的，对扩展是开放的

    def f1():
        print("f1")
    def f2():
        print("f2")
现在新增要求，对原有的函数增加显示`unix时间戳`的功能

不用装饰器实现:

    import time
    def current_time(func):
        print(time.time())     #time.time()是获取当前unix时间戳
        func()
    current_time(f1)     #利用函数式编程，函数也可作为参数被导入
    current_time(f2)     #输出的时候要函数套函数，可以接受定义时候的复杂，但是调用的时候一定要简洁
    #输出：1700828854.1110294
          f1
          1700828854.1119823
          f2
>缺点是，打印时间这个功能应该属于原有的函数的，并不是新增加的函数的，而且两者没有体现关联性

用装饰器实现:

    import time
    def current_time(func):    
    #利用函数式编程，函数也可作为参数被导入；定义了一个名为current_time的装饰器，该装饰器接受一个名为func的函数作为参数;
    #这个func其实是个抽象的函数，下面引用装饰器的函数会替换这个抽象函数放入运算
        def wrapper():
            print(time.time())
            func()
        return wrapper
上面这个代码块就是一个装饰器，命名为`current_time`

引用装饰器：`@语法糖`

    @current_time
    def my_function():
        print("Hello, world!")
    my_function()              
    #好处就是可以直接调用被装饰的函数运行，不用函数套函数了，十分简洁
    #输出：1700830956.5398917  #注意输出的顺序
          Hello, world!
如果把print(time.time())和func()换个顺序：

    import time
    def current_time(func):
        def wrapper():
            func()
            print(time.time())
        return wrapper

    @current_time
    def my_function():
        print("Hello, world!")
    my_function()
    #输出：Hello, world!  #顺序发生变化
          1700831115.9833217

- 装饰器体现了AOP的编程思想，值得一学

### 如果被装饰的函数有参数，那么装饰器函数的参数也要有
例：

    import time
    def current_time(func):
        def wrapper(name):     #需要引用装饰器的函数有参数，那装饰器里也要填入相应参数
            func(name)
            print(time.time())
        return wrapper

    @current_time
    def my_function(name):   #被装饰函数里有个name参数
        print("Hello, " + name)
    my_function('小红')

### 如果下面还有其他函数有两个甚至更多的参数需要引用该装饰器呢

>首先要明白一个理念，装饰器要考虑的是通用性，不能把它和某个函数绑定，这样装饰器就没有意义了

>那有没有参数可以表示任意的多个参数呢，也就是这个参数是可变的
>
>`*args` ------`args`是很多语言的位置参数，这里只是一个代指，没有什么具体的意义，就是一个通用的叫法

    import time
    def current_time(func):
        def wrapper(*args):     #(*args)表示任意数量的任意参数
            func(*args)
            print(time.time())
        return wrapper

    @current_time
    def my_function(name1,name2):   #被装饰函数里有两个参数
        print("Hello, " + name1 +"my name is " + name2 )
    my_function('小红','小蓝')
### 还有一种类型的参数:关键字参数
>关键字参数`**kwargs` -----可以把传入的参数作为字典输出出去   
>可以简化为`**kw`


    import time
    def current_time(func):
        def wrapper(*args,**kw):     #(**kw)表示关键字参数
            func(*args,**kw)
            print(time.time())
        return wrapper

    @current_time
    def my_function(name1,name2,**kw):   #(**kw)关键字参数,输入的参数要是一个等式，满足字典的格式
        print("Hello, " + name1 +"my name is " + name2 +"I like" + str(kw))   
        #注意这里引用**kw要把**去掉，而且要对应下面输入的参数类型

    my_function('小红','小蓝',a='读书', b='听音乐')   #注意**kw要以字典的格式输入
    #输出：Hello, 小红my name is 小蓝I like{'a': '读书', 'b': '听音乐'}
          1700834549.3968458

- 注意这里的args和kw都可以替换为别的词，但基本都统一使用args和kw，辨识性高

>`*args`和`**kw`一般都组合使用，当不知道后面会传入什么参数的时候，就使用`*args`和`**kw`万金油

装饰器用于给已经封装好的完整函数修改或添加新的功能,一个函数可以添加多个装饰器

## flask开发web

    @api.route('/get',methods=['GET'])   #加了这个装饰器之后下面的函数就是一个控制器
    def test_javascript_http():
        p = request.args.get('name')
        return p,200
上面这个接口就是所有人可以访问的

    @api.route('/psw',methods=['GET'])
    @auth.login_required
    def get_gsw():
        p = request.args.get('psw')
        r = generate_password_hash(p)
        return 'aaaaaa',200
上面就是一个需要验证身份的接口
