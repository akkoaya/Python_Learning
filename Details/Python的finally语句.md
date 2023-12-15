## finally语句
>`finally` 语句块中的代码，无论是否出现异常，均会执行  

> 有一些情况需要捆绑两个行为：  
> 1.文件的打开和关闭(不关闭的话会导致后台占用，其他软件无法进行编辑)  
> 2.数据库的连接和断开  
> 3.锁的获取和释放

    def read_file(filename):
        try:
            with open(filename) as f:
            #with的意思是在try之前就执行完毕这段代码，也就是默认`打开文件`这个操作是没有异常的
                return f.read()
        except FileNotFoundError:
            return "File not found"
        finally:
            f.close()
>无论上述的代码执行过程中是否出现异常，都会执行`finally`语句块中的代码

思考：`finally`是在`return`之前执行还是在`return`之后执行？
    
    def test():
        f = [1, 2]
        sum = 0
        try:
            for i in f:
                sum += int(i)
                return sum
        except Exception:
            pass
        finally:
            sum = 10
            print(sum)
    print(test())
    #输出：10
          1   #这里sum没有变为10是因为int类型是值传递，不会改变原有的sum
>`finally`是在`return`之前执行的  
> 实际上在`return sum`之前,`sum`会被赋值给一个临时变量，`return`的其实是那个临时变量，由于`sum`是值传递所以`sum`的值没有被`finally`影响

    def test():
        f = [1, 2]
        s = sum(f)
        try:
    
            return f
        except Exception:
            pass
        finally:
            f.append(3)
            print(s)
    print(test())
    #输出：3
          [1,2,3]   #f里面添加了一个元素3,并且return回的是执行finally之后的f，因为列表list是引用传递

但是`finally`会出现问题:以锁为例

    import threading
    
    def test():
        try:
            #此处进行打开文件等一系列操作
            lock = threading.Lock()
            lock.acquire()  #获取锁
            #此处是多行处理逻辑
        except Exception:
            pass
        finally:
            lock.release()
            pass
>如果，在打开文件等一系列操作的时候就已经异常了，那`try`里面声明的`lock`变量就会不存在，进而导致`finally`里的`lock.release()`报错

可以解决：但会很丑陋

    import threading
    
    def test():
        try:
            #此处进行打开文件等一系列操作
            lock = threading.Lock()
            lock.acquire()  #获取锁
            try:
                #此处是多行处理逻辑
            expect Expection:
                pass
            finally:
                lock.release()
        except Exception:
            pass
        finally:
            pass

## with语句
>用with可以替代`try expect finally`

    with open(filename) as f:
        #此处是多行处理逻辑

>`with`语句的好处就是可以不用管`close`的操作，因为`with`语句会自动帮你关闭
    
