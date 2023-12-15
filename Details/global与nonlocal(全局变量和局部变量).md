## 变量的作用域
### nonlocal和global的区别
```python
def scope_test():
    spam = "spam"

    def do_local():
        # 此函数定义了另外的一个spam字符串变量，并且生命周期只在此函数内。此处的spam和外层的spam是两个变量，如果写出spam = spam + “local spam” 会报错
        spam = "local spam"
        print(spam)

    def do_nonlocal():
        nonlocal spam  # 使用外层的spam变量
        spam = "nonlocal spam"

    def do_global():
        global spam
        spam = "global spam"

    do_local()
    print("After local assignmane:", spam)
    do_nonlocal()
    print("After nonlocal assignment:", spam)
    do_global()
    print("After global assignment:", spam)
    # 这里还是输出nonlocal spam是因为函数内部只会使用函数内部的变量，不会使用全局变量

scope_test()
print("In global scope:", spam)
```
### 输出
    After local assignmane: spam
    After nonlocal assignment: nonlocal spam
    After global assignment: nonlocal spam
    In global scope: global spam



## 误区
```python
courses = ["a", "b", "c"]
def change_list(a):
    a[0] = "d"
    print(a)

change_list(courses)
print(courses)
```
- 改变列表的元素会把全局变量一起改变
##
```python
courses = ["a", "b", "c"]
def change_list2(a):
    a = ["d", "e", "f"]
    print(a)

change_list2(courses)
print(courses)
```
- 但是直接赋值，不会改变全局变量