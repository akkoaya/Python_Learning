
## 函数
>`print()`,`input()`,`round()`,`abs()`,`min()`,`max()`,`len()`,`range()`,`sum()`...这些都是函数

### round函数
保留小数点后几位,同时四舍五入

    a = 1.23456
    result = round(a,3)  #保留3位小数
    print(result)
    #输出：1.235---------四舍五入的结果

### 如何快速了解python内置的函数的作用
>在cmd中输入`python`，进入python交互模式  
输入`help(round)`即可查看round函数的用法

### 如何自定义函数
>语法：`def FuncName(parameter_list):`-----def 函数名(参数列表):  
 	        `pass`------函数体  
  	        `return value`------返回结果  
            #到这里整个函数的定义就结束了，  后面再写内容也不会识别

举例1:

    def add(x,y):
	    return x+y

举例2:

    def print(code):
        print(code)
    #报错，超过了最高递归深度
>原因：上面的函数在反复自己调用自己，python解释器默认的递归深度是1000，如果超过这个深度，就会报错

    sys.setrecursionlimit(2000)
    #设置递归深度,要先import sys(深度上限受计算机性能影响,我的笔记本是996次为上限)

>解决办法，换一个函数名字，不能和内部的函数名一样  
>并且该函数没有return，会返回None

举例3：

    def add(x,y):
    	return x+y
    def print_code(code):
        print(code)
    a = add(1,2)
    b = print_code('Apple')
    print(a,b)
    #输出:Apple
         3 None
>原因：1.`print(code)`在上面定义函数的时候就已经输出了，所以第一行会输出`Apple`  
      2.`print(a,b)`会把a和b在同一行输出，`print_code`函数没有返回值，所以b返回`None`

举例4：

    def price(number1,number2):
          price1 = number1*10
          price2 = number2*5
          return price1,price2 ------这里不需要加括号，python会自动认为输出的是元组类型的(price1,price2)，这也是动态语言的特性
>已知上面返回的是元组，那确实可以用`total_price=price(1,2)`来接收  
> 但是这样写的话，下面要输出就要写成元组的索引方式：`print(total_price[0],total_price[1])`,时间久了就会忘记元组里的具体元素

>建议的做法是：用有意义的变量名称来接受函数的结果

    number1_price,number2_price = price(1,2)
    #这样就把元组里的元素赋值给number1_price和number2_price了
这样的操作被称作是：`序列解包`

### 序列解包

    a=1
    b=2
    c=3
这样要写三行，有没有更简洁的方法呢
    
    a,b,c = 1,2,3
序列解包的概念

    d=1,2,3
    type(d)=tuple
    #误区：以为没有加括号就不是tuple了
    a,b,c = d
    #这就是序列解包，把序列d里的元素赋值给a,b,c，要一一对应才行，数量要一致

### 参数种类
>1.必须参数  
2.关键字参数  
3.默认参数

**1.必须参数**

    def add(x,y):
        return x+y
    #调用add(1,2)
    #输出:3

    #调用add(1)
    #报错:TypeError: add() missing 1 required positional argument: 'y'
必须参数：调用函数的时候必须传入的参数，否则会报错

**2.关键字参数**

    def add(x,y):
        return x+y
    print(add(y=2,x=1))
    #输出:3

    #调用add(y=2)
    #报错:TypeError: add() missing 1 required keyword-only argument: 'x'
关键字参数：调用函数的时候，可以不用按照定义的顺序来传入参数，但是要加上参数名

**3.默认参数**

    def add(x,y=1,z=2,v=3):
        return x+y+z+v
    print(add(1))
    #输出:7
默认参数：调用函数的时候，可以不用传入已经定义的参数，默认使用定义的值，只需要输入必须参数

>如果要输出改变默认参数，调用的时候可以改写默认参数，`print(add(1,1,4))`  
>输出:9(修改了z的默认参数为4，但不能跳过y的默认参数，可以不输入v的参数)    
>输出时修改默认参数，必须按照参数顺序一个一个修改，不能跳着修改；但可以用关键字参数直接修改print(add(2,z=4))  
> 
>但是默认参数要遵循：必须参数必须设置在默认参数之前，否则会报错：add(x=1,y)---------报错
