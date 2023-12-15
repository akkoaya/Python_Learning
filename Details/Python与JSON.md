
>JSON是一种轻量级的数据交换格式,是web数据交换的主流数据结构,相应的还有XML，与XML相比，JSON是相当的轻量化，而且更加简洁，JSON的语法也是经过精心设计的，非常简单，易于理解，现在XML不太常用了

>全名JavaScript Object Notation,JavaScript对象标记，但JSON并不是JavaScript的一个附属品，只不过数据格式很像

>JSON是一种`数据格式`，`字符串`是JSON的表现形式，符合JSON格式的字符串就叫`JSON字符串`

>JSON并不是哪一种语言特有的，可以把某一种语言的功能做成一个服务，利用服务的特性，使用JSON来传递数据，实现跨语言的交换数据

>所以JSON就相当于是A语言和B语言数据类型转换的`中间数据格式`

>现在的小程序，基本都是使用的JSON，Html只适用于web的开发，小程序后端提供的接口，都是JSON字符串，前端拿到JSON字符串，解析成JS对象，然后渲染到页面上，而在JSON后端，可以使用各种不同语言的API服务


## 看一看代码里面python是如何和JSON进行数据交互的：
由JSON字符串转换到某一种语言的对象的解析过程，称为：`反序列化`

    import json
    json_str = '{"name": "jason", "age": 18, "gender":"male"}'
    #json中的字符串必须使用双引号表示，用单引号是无法解析成功的，而且不管key和value,只要是字符串，都要用双引号表示，最外层就要用单引号
其实这个格式是JavaScript里对象的格式，所以理论上应该命名为json_obj，这就是JSON字符串：JSON格式的字符串;而在Python里就是字典的格式，所以不同的语言会把JSON的数据类型认为是不同的类型

    student = json.loads(json_str)  
    #用loads函数把JSON数据类型解析为Python数据类型
    print(student)
    print(type(student))
    print(student['name'])
    print(student['age'])

    输出:{'name': 'jason', 'age': 18, 'gender': 'male'}
         <class 'dict'>
         #JSON的那串格式是字符串，但在Python里是字典格式



## JSON数组array如何用JSON字符串表示
`数组array`就类似于Python中的`列表list`和`元组tuple`，只不过Python把集合分的比较细,数组其实就是一个集合

    import json
    json_str = '[{"name": "jason", "age": 18},{"name": "tank", "age": 20}]'
    student = json.loads(json_str)
    print(student)
    print(type(student))

    输出:[{'name': 'jason', 'age': 18}, {'name': 'tank', 'age': 20}]
       <class 'list'>


## JSON和Python的数据类型对应关系

    JSON数据类型         Python数据类型
      {}object              dict
      []array               list
      ""string              str
      number                int或float
      true                  True
      false                 False
      null                  None

## 序列化的过程
Python数据类型->JSON和Python的数据类型

    import json
    student = [{"name": "jason", "age": 18},{"name": "tank", "age": 20}]
    json_str = json.dumps(student)
    print(json_str)
    print(type(json_str))

    输出：[{"name": "jason", "age": 18},{"name": "tank", "age": 20}]
         <class 'str'>
         #list格式被转换成了str格式
序列化可以把一种语言的对象转换为字符串格式，便于传输，存入mysql数据库，但是别直接传入对象，要把对象分解成一个`二维表结构`，也就是把对象拆成一个个属性
