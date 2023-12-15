## 错误和异常
错误和异常如何区分
>错误`Error`是程序中可以预知的情况，所以会故意设置一个错误，用于输出设置好的错误的情况  
>异常`Exception`是程序中无法预知的情况，所以会抛出一个异常

以除法函数为例：  
1.异常

    def div(a,b):
        if b == 0:
            raise Exception("被除数不能为0")  #这是异常
        return a/b
    
    def cal():
        a = int(input())
        b = int(input())
        div(a,b)
        #此处为其他代码逻辑
    cal()
        #如果这样，终端里输入被除数0，程序会直接抛异常，就不会执行下面的代码逻辑了

解决：捕获异常
    
    def div(a,b):
        if b == 0:
            raise Exception("被除数不能为0")  #这是异常
        return a/b
    
    def cal():
        a = int(input())
        b = int(input())
        try:
            div(a,b)
        except Exception as e:
            print(e)
        #此处为其他代码逻辑
    cal()
    #这样即使输入被除数0，程序也会继续执行后面的代码逻辑，只是输出一个异常的提示字符串

可以把下面的代码放入`while`循环，输入错误也能重新输入
    
    def div(a,b):
        if b == 0:
            raise Exception("被除数不能为0")  #这是异常
        return a/b
    
    def cal():
        while 1:
            a = int(input())
            b = int(input())
            try:
                div(a,b)
            except Exception as e:
                print(e)
            #此处为其他代码逻辑
    cal()

2.错误
    
    def div(a,b):
        if b == 0:
            return None, "被除数不能为0"  #这就是错误

        return a/b, None
    
    def cal():
        while 1:
            a = int(input())
            b = int(input())
            v,err = div(a,b)   #v和err用于接受div函数return的两个结果
            if err is not None:  
            #由于None是全局唯一的对象，所以当把None赋值给某个值的时候要用`is`,而不是`==`
            #如果用`=`,会报错; 如果用`==`,只会返回'None'这个字符串
                print(err)
            else:
                print(v)
    cal()

### 但有些情况会把异常当作错误处理，在需要抛异常的时候却没抛异常，让程序继续执行了

    import re
    desc = "age:18"
    a = re.match("ages:(.*)",desc)  #ages在这里并不存在
    if a is not None:
        print(a.group(1))
    #但是程序不会抛异常，因为正则表达式默认是会存在匹配不到的情况，默认这不是异常
