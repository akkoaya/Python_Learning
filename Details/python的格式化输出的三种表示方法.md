## 格式化输入
    name = 'python'
    age = 18

### 方法一
用`%s，%d`占位符

    print('%s is %d years old.' % (name, age))
### 方法二
用`format函数`，{ }作为占位符

    print('{} is {} years old.'.format(name, age))
### 方法三
用`f-string`方法，可以看到这种方式十分方便；实际上上面两行蓝色波浪线也在提醒用这种方法

    print(f'{name} is {age} years old.')
