
## python3.4添加了枚举类型：
    from enum import Enum
    class VIP(Enum):      所有的枚举类型都是Enum类的子类
        YELLOW = 1     枚举常量最好用大写表示
        GREEN = 2
        BLACK = 3
        RED = 4
    print(VIP.YELLOW)
     输出：VIP.YELLOW
     换成普通的类的话会输出1，因为1本身只负责存储，名字才是有意义的，这就是枚举的意义所在

### 与字典类型和普通的类相比枚举类的优点：
字典

    {'yellow': 1, 'green': 2, 'black': 3, 'red': 4}
普通的类

    class TypeVIP():
        YELLOW = 1
        GREEN = 2
>字典和普通的类，它们的值都是可变的，而枚举强行改值的话会报错  
字典和普通的类，value可以重复，并且读取的时候没有问题  
枚举里不能同时存在两个'YELLOW',但是后面的值可以重复，但读取的时候会有问题


枚举中值重复会怎么样  

    from enum import Enum
    class VIP(Enum):
        YELLOW = 1
        GREEN = 1
        BLACK = 3
        RED = 4
    print(VIP.GREEN)
    #输出：VIP.YELLOW  -----打印的是GREEN，输出的却是YELLOW
>这时候其实`GREEN`应该叫做`YELLOW`的别名，`GREEN`可以改名为`YELLOW_ALIAS`
  ,而且值相等第二个是不会被遍历出来的 

如果要遍历出别名，在`for in 循环`中加上`__members__`属性,再调用`__members__`属性里的`items`方法

    for a in VIP.__members__.items():   
    #不调用items方法也可以，会直接输出常量名
    print(a)    
    #输出：('YELLOW', <VIP.YELLOW: 1>)
          ('GREEN', <VIP.YELLOW: 1>)
          ('BLACK', <VIP.BLACK: 3>)
          ('RED', <VIP.RED: 4>)


**获取枚举常量的值**

    from enum import Enum
    class VIP(Enum):
        YELLOW = 1
        GREEN = 2
        BLACK = 3
        RED = 4
    print(VIP.YELLOW.value) 
    #方法就是在常量名称后面加一个'.value'
    #输出：1

**获取枚举常量的名字**

    from enum import Enum
    class VIP(Enum):
        YELLOW = 1
        GREEN = 2
        BLACK = 3
        RED = 4
    print(VIP.YELLOW.name)
    方法就是在常量名称后面加一个'.name' ，注意不是key
    #输出：YELLOW
    print(type(VIP.YELLOW.name))  ------<class 'str'> 是字符串类型
    print(type(VIP.YELLOW))       ------<enum 'VIP> 是一个枚举类型


**枚举类型是可以遍历的**

    from enum import Enum
    class VIP(Enum):
        YELLOW = 1
        GREEN = 2
        BLACK = 3
        RED = 4
    for a in VIP:
        print(a)
    #输出：VIP.YELLOW  
          VIP.GREEN
          VIP.BLACK
          VIP.RED


**枚举类型的常量可以进行等值比较**

    from enum import Enum
    class VIP(Enum):
        YELLOW = 1
        GREEN = 2
        BLACK = 3
        RED = 4
    print(VIP.YELLOW == VIP.GREEN)   
    #不能进行等值以外的比较，(<，>，>=，<=，!=)，但是身份比较is可以
    #输出：False


### 枚举数据类型转换
在数据库中都是以数字类型存储变量或者常量的，比较节省空间  

场景：在数据库中取到了数字1，如何把数字1转换成枚举类型

    from enum import Enum
    class VIP(Enum):
        YELLOW = 1
        GREEN = 2
        BLACK = 3
        RED = 4
    print(VIP(1))   
    #把数字1转换成枚举类型
    #输出：VIP.YELLOW


如果要限制输入的数据类型只能是int，且不能重复

    from enum import IntEnum，unique    
    #IntEnum是限制为int类型，unique是限制枚举类型不能重复
    @unique  #语法糖
    class VIP(IntEnum):
        YELLOW = 1
        GREEN = 'str' --------报错，不能输入字符串
        BLACK = 1     --------报错，不能输入相同数据
        RED = 4


23种设计模式-----高级进阶，现阶段可以不去了解
