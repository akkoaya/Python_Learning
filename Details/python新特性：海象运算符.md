
## 海象运算符`:=` (注意区别于Go里面的赋值符号)
### 不使用海象运算符：
    course_list = ["python", "golang", "java"]
    course_size = len(course_list)
    powers = [course_size, course_size*2, course_size*3]
    print(powers)
    # 要写三行
### 用海象运算符：
    course_list = ["python", "golang", "java"]
    powers = [course_size := len(course_list), course_size*2, course_size*3]
    print(powers)
    # 只要两行

>海象运算符就是可以把定义变量为函数的步骤放入原来不能放定义变量的地方
> 
>注意不能直接 a := 10 ;这是Golang里面的写法，python中不能写这个
