overload 与 override

1. python 没有类型, python 支持可变参数. 故没有overload



### 12 python提升-1

#### 12.1 GIL 全局解释器锁

1. GIL = global interpret lock
2. Python语言和GIL没有关系
   1.  历史原因`Cpython虚拟机`(解释器)，难以移除GIL
3. GIL原理： 线程 在执行时要先获取GIL, 保证**同一时刻**只有**一个线程**可以执行代码
4. 线程释放GIL锁的情况
   1.  IO操作 能引起阻塞的系统调用 前,可 暂时释放GIL,但执行完后, 要重新获取GIL
   2. Python 3.x 使用 计时器（执行时间达到阈值后，当前线程释放GIL）
   3. Python 2.x 使用`tickets`计数达到100
5. Python多进程 可用多核CPU资源
6. 爬取时, 多线程比单线程性能高: 因为遇到`IO阻塞`会自动释放`GIL锁`



#### 12.2 深拷贝、浅拷贝

1.  `=`  等号

   1. 是赋值 不是拷贝,  指向同一个对象的 `引用`

   2. list `=`   是赋值, 不是浅拷贝

      ```
      b = [11, 22]
      a = b
      id(a)  # 56454044 
      id(b)  # 56454044,  a与b指向同一个对象的引用
      ```

   3. dict  `=`  是赋值, 不是 浅拷贝

      ```
      c = {'name':"wang"}
      d = c
      c["passwd2"]="234"
      c  # 输出 {'name': 'wang','passwd2': '234'}
      d  # 输出 {'name': 'wang','passwd2': '234'}
      id(c) # 56454016
      id(d) # 56454016 , c与d指向同一个对象的引用
      ```

2. 浅拷贝

   1. 一个对象的`顶层拷贝`

   2. 俗的理解：拷贝了 引用，并没有拷贝 内容

      

   3. copy.copy()

      1. list,  copy.copy()

         ```
         a = [11, 22]  # a指向[11, 22]de引用
         b = [33, 44]  # b指向[33, 44]的引用
         c = [a, b]    # c指向[a,b]的引用,a,b分别指向[11,22],[33,44]引用
         id(a)  # 56500160
         id(b)  # 56494272
         id(c)  # 56493056
         
         import copy
         d = copy.copy(c)  
         d  # [[11, 22], [33, 44]]
         id(d)  # 56453952 
         id(c)  # 56493056, d与c指向的不同的对象
         id(d[0]) # 56500160, 指向[11, 22]的地址
         id(d[1]) # 56494272, 指向[33, 44]的地址
         
         a.append(11)
         c  # [[11, 22, 11], [33, 44]]
         d  # [[11, 22, 11], [33, 44]]
         ```

         1. 浅copy, 只复制 最顶层的那个列表list

3. 深拷贝

   1. 对象 所有层次的拷贝(递归),  或 递归拷贝

   2. ```
      import copy
      a = [11, 22]
      b = copy.deepcopy(a)
      a  # [11, 22]
      b  # [11, 22]
      id(a)  # 56604736
      id(b)  # 52703616 a与b指向的不同的对象
      ```

      1. deepcopy copy了 a列表中所有的数据的引用,  不只 copy 了a指向的列表的引用

      ```
      a.append(33)
      a  # [11, 22, 33]
      b  # [11, 22]
      ```

   3. 进一步理解 deepcopy

      ```
      import copy
      a = [11, 22]
      b = [33, 44]
      c = [a, b]
      d = copy.deepcopy(c)  
      id(a)  # 56608640
      id(b)  # 56624832
      id(c)  # 56624640
      id(d)  # 56619648  # d,c指向不同引用
      id(c[0])  # 56608640  c[0]与a指向相同引用
      id(c[1])  # 56624832
      id(d[0])  # 56598784, d[0]与c[0]指向不同引用
      id(d[1])  # 56623040
      ```

3. 拷贝的 其他方式

   1. list 切片拷贝: 浅拷贝, 深拷贝两种

      1. 切片表达式可以赋值一个序列,  list全体的切片的是浅拷贝

      2. ```
         a = [11, 22]
         b = [33, 44]
         c = [a, b]
         d = c[:]  # list切片/分片
         id(c)  # 56645504
         id(d)  # 56644224,  [:]功能等同于浅拷贝
         id(c[0])  # 56638272
         id(d[0])  # 56638272, d[0]与c[0]指向相同 引用
         a.append(33)
         c  # [[11, 22, 33], [33, 44]]
         d  # [[11, 22, 33], [33, 44]]
         ```
         
         1. d=c[:] 与 d=copy.copy(c) 一样,  浅拷贝

      3.  全体切片`[:]`是浅拷贝, 部分切片 如[1:2] 深拷贝. 拷贝原理不同

         1. ```
   a = [11, 22, 33]
            b = a[1:2] # [22] 创建一个新对象,b指向新对象的引用
         b.append(2222)
            b  # [22, 2222]
            a  # [11, 22, 33] , a与b指向不同的引用
            ```
      
   2. dict, 
   
      1. 字典,  copy() 方法可以拷贝一个字典, 浅拷贝

      2.  ```
   d = dict(name='as', age=27, children_ages=[11,22])
         d_copy = d.copy()
         d['children_ages'].append(9)
         
         d # {'name': 'as', 'age': 27, 'children_ages': [11, 22, 9]}
         d_copy # {'name': 'as', 'age': 27, 'children_ages': [11, 22, 9]}
         
         id(d) # 56679424
         id(d_copy) # 56624384 顶层地址 不同
         
         id(d['children_ages']) # 56565824
         id(d_copy['children_ages']) # 56565824, 底层地址 相同
         
         id(d['name'])  #36792176
         id(d_copy['name']) # 36792176, 底层地址 相同
         ```
   
5. 注意点

   1. copy.copy() 

      1. 对 可变类型， 浅拷贝. 对 不可变类型，不拷贝， 仅 指向

      2. 可变类型list，copy.copy(list可变类型)  是浅拷贝, 

         1. ```
            # list, 一层, 浅拷贝
            a = [11, 22]
            b = copy.copy(a)
            id(a)  # 56078656
            id(b)  # 55576768, 浅拷贝一阶地址不同
            a.append(44)
            a  # [11, 22, 44]
            b  # [11, 22]
            ```

         2. ```
            # list中一阶list地址不同,二阶list地址相同, 是浅拷贝
            a = [11,22]
            b = [33, 44]
            c = [a, b]
            d = copy.copy(c)
            
            id(c)  # 52777344
            id(d)  # 61364736, d与c指向不同的引用
            
            id(c[0])  # 52705664
            id(d[0])  # 52705664, 底层指向相同的引用
            a.append(55)
            a  # [11, 22, 55]
            c  # [[11, 22, 55], [33, 44]]
      d  # [[11, 22, 55], [33, 44]]
            ```
            
         3. ```
            // 两层, 顶层list, 底层元组couple, 浅拷贝
            a = (11, 22)
            b = (33, 44)
            c = [a, b]
            d = copy.copy(c)  #list类型, 可变
            id(c)  # 55928192
            id(d)  # 52704064, d与c指向不同引用
            id(c[0])  # 55787584
         id(d[0])  # 55787584, 底层指向相同引用
            ```

            

      3. tuple元组, 不可变类型 ，copy.copy() 仅 指向, 不拷贝
   
         1. ```
            // tuple 元组, 一阶, 不拷贝, 指向同一个引用
            a = (11, 22, 33)
            b = copy.copy(a)
            id(a)  # 63434240
         id(b)  # 63434240, b与a指向同一个引用
            ```
   
         2. ```
            //一阶元组, 底层是list, 指向同一个引用, 不拷贝
            a = [11, 22]
            b = [33, 44]
            c = (a, b)
            d = copy.copy(c)
            id(c)  # 65774016
            id(d)  # 65774016, d与c指向同一个引用
            id(c[0])  # 65770368
            id(d[0])  # 65770368, 底层指向同一个引用
            a.append(33)
            c  # ([11, 22, 33], [33, 44])
         d  # ([11, 22, 33], [33, 44])
            ```
   
         3. ```
            // 顶层元组, 底层元组, 不拷贝, 指向同一个引用
            a = (11, 22)
            b = (33, 44)
            c = (a, b)
            d = copy.copy(c)
            id(c)  # 55697344
            id(d)  # 55697344,  d与c指向同一个引用
            id(c[0])  # 63020160
         id(d[0])  # 63020160
            ```

      4. copy.copy总结 (引用 或 浅拷贝)
   
         1. tuple 不变类型
            1. 一层, 引用, 不拷贝
            2.  二层, 引用, 不拷贝
         2. list与tuple混合
            1. 顶层 tuple, 底层 list, 引用, 不拷贝
            2. 顶层list, 底层tuple, 浅拷贝
      3. lis可变类型
            1. list 一层, 浅拷贝
         2. list 二层, 引用
   
   2. copy.deepcopy, 
   
      1. 可变类型深拷贝, 不可变类型 引用 不拷贝
   
      2. list, 可变类型, deepcopy()  深拷贝
   
         1.  ```
             // list, 一层, 深拷贝, 
             a = [11, 22]
             b = copy.deepcopy(a)
             id(a)  # 54135488
             id(b)  # 59297280, a,b指向不同的引用
             a.append(55)
             a  # [11, 22, 55]
             b  # [11, 22]
             ```
   
         2.  ```
             // list, 顶层list, 底层list, 深拷贝
             a = [11, 22]
             b = [33, 44]
             c = [a, b]
             d = copy.deepcopy(c)
             id(c)  # 55295232
             id(d)  # 55314432, d与c指向不同的引用
          a.append(55)
             c  # [[11, 22, 55], [33, 44]]
             d  # [[11, 22], [33, 44]]
             ```
   
         3.  ```
             // 两层, 顶层list, 底层元组, 浅拷贝
             a = (11, 22)
             b = (33, 44)
             c = [a, b]
             d = copy.deepcopy(c)
             id(c)  # 56524608
         id(d)  # 56558400, d与c指向不同引用
             id(c[0])  # 52714368
      id(d[0])  # 52714368, 底层指向相同引用
             ```
   
             
   
      3. 元组, 不可变类型, deepcopy() 是指向, 不拷贝.
   
         1.   ```
             // 元组, 一层, deepcopy, 不拷贝, 指向同一个引用
             a = (11, 22, 33)
         b = copy.deepcopy(a)
             id(a)  # 59248768
             id(b)  # 59248768, b与a指向同一个引用
             ```
   
         2.  ```
             // 两层 顶层元组tuple, 底层list, 深拷贝
             a = [11, 22]
             b = [33, 44]
             c = (a, b)
             d = copy.deepcopy(c)
             id(c)  # 61299200
             id(d)  # 60835328, d与c指向不同的引用, 深拷贝
             a.append(55)
             c  # ([11, 22, 55], [33, 44])
          d  # ([11, 22], [33, 44])
             id(c[0])  # 58634560
             id(d[0])  # 52765632, 指向不同引用, 深拷贝
             ```
   
         3.   ```
              // 两层: 顶层元组list, 底层tuple, 浅拷贝
              import copy
              a = (11, 22)
              b = (33, 44)
              c = [a, b]
              d = copy.deepcopy(c)
                 
              id(c)  #77962056
             id(d)  #77306760, 地址不同
              
              id(c[0])  #77915336
              id(d[0])  #77915336, 地址相同, 浅拷贝
             ```
   
         4. ```
         // 两层 顶层元组,底层元组,不拷贝, 指向同一个引用
            a = (11, 22)
         b = (33, 44)
            c = (a, b)
         d = copy.deepcopy(c)
            id(c)  # 58924544
         id(d)  # 58924544, d与c指向同一个引用
            id(c[0])  # 52739520
      id(d[0])  # 52739520 , 指向同一个引用
            ```
   
      4. 总结 copy.deepcopy (引用, 浅拷贝, 深拷贝)
   
         1. tuple, 不变类型
            1. tuple 一层, (), 引用,  不拷贝(即不是浅拷贝也不是深拷贝)
            2. tuple 二层, ((), (), ()), 引用,  不拷贝(即不是浅拷贝也不是深拷贝)
         2. tuple与list混合
            1. 顶层tuple, 底层list,  ([], [], []), 深拷贝
            2. 顶层list, 底层tuple, [(), (), ()], 浅拷贝
         3. list
            1. list 一层, 深拷贝
            2. list 二层, 深拷贝
   
   3. 总结:
   
      1. 若c 是元组,
      1.  `copy.copy(c)`,  仅仅是元组的引用
         2. `copy.deepcopy(c)` , 引用, 不拷贝
      3. d = c  # 让d这个这个变量指向c指向的空间
         4. d = copy.copy(c)  #是指向引用, 不发生
      5. d=copy.deepcopy(c)  # 引用, 不拷贝

#### 12.3 私有化

1. `_`, `__`, 变量名中的 含义

   1. | 形式    | 含义                  | 意义                                                         |
      | ------- | --------------------- | ------------------------------------------------------------ |
      | `xx`    | 公有变量              |                                                              |
      | `_x`    | 前置单下划线          | <u>模块私有化</u>属性或方法, 不是 类私有<br>`from somemodule import *` 禁止导入,<BR>类对象和子类 可以访问 |
      | `__x`   | 前置双下划线          | <u>类私有</u>, <br>不被子类继承, 避免与子类中的属性命名冲突,<br>无法在 类或对象的外部 直接访问( 名字重整所以访问不到 ) |
      | `__x__` | <u>前后置双下划线</u> | <u>魔法</u>方法或属性<br> 如`__init__`, 不要自己发明这样的名字 |
      | `xx_`   | 后置单下划线          | 避免与Python关键词的<u>冲突</u>                              |

   2. private, protected, public

      1. | 类型          | 含义                                                         | 示例               | 继承关系                                                 |
         | ------------- | ------------------------------------------------------------ | ------------------ | -------------------------------------------------------- |
         | public类型    | python中数据和方法默认 是pubic类型的， 方法和变量名 没有下划线 | `xx` 没有以下划线  | public类型可在子类、类内以及类外被访问                   |
         | protected类型 |                                                              | `_xx` 单下划线开头 | 允许其本身与子类进行访问                                 |
         | private类型   | 类私有类型的变量或者方法,                                    | `__xx` 双下划线    | 只能允许类内进行访问. 类的实例中也没有, 类实例也不能调用 |
         | 特殊方法      |                                                              | `__xx__`           |                                                          |

      2. 示例

         1. `p1 = Person('jack', 25, 'football')`
            1. `p1.name`,  `p1._age`   #  正确<br>
            2. `p1.__taste`   # 错误, 类私有
            3. `Person.name, Person._age, Person.__taste`  #错误
         2. `s1 = Student('jack', 25, 'football')`   #Student是Person的子类
            1. `s1.name`, # public, 正确, 被子类继承, 子类实例可以调用
            2. `s1._age`  #protected, 正确, 被子类继承, 子类实例可以调用
            3. `s1.__taste`  #private, 错误, 父类私有, 子类不能继承, 子类实例不能调用

   3. 查看对象/类的 `属性/方法`

      1. `s1.__dict__; Stu.__dict__`  :  返回 字典类型, 对象/类 的所有`属性`和对应的值
      2. `dir(s1);  dir(Stu)`  :  返回 列表类型,  对象/类的 所有属性名和方法名
      3. `help(s1), help(Stu)`  : 返回字符串类型, 完整描述

   4. 示例

      1. ```
         #coding=utf-8
         
         class Person(object):
             def __init__(self, name, age, taste):
                 self.name = name
                 self._age = age 
                 self.__taste = taste
         
             def showperson(self):
                 print(self.name)
                 print(self._age)
                 print(self.__taste)
         
             def dowork(self):
                 self._work()
                 self.__away()
         
         
             def _work(self):
                 print('my _work')
         
             def __away(self):
                 print('my __away')
         
         class Student(Person):
             def construction(self, name, age, taste):
                 self.name = name
                 self._age = age 
                 self.__taste = taste
         
             def showstudent(self):
                 print(self.name)
                 print(self._age)
                 print(self.__taste)
         
             @staticmethod
             def testbug():
                 _Bug.showbug()
         
         # 模块内可以访问，当from  cur_module import *时，不导入
         class _Bug(object):
             @staticmethod
             def showbug():
                 print("showbug")
         
         s1 = Student('jack', 25, 'football')
         s1.showperson()
         print('*'*20)
         
         # 无法访问父类的__taste,同时子类没有__init__方法,因此子类没有__taste属性.导致报错
         # s1.showstudent()  # 子类的对象无法访问父类的__taste
         s1.construction('rose', 30, 'basketball') #方法在对象s1中创建一新字段`__taste`, 与父类中`__taste`重名,但子类Student没有`__taste`
         s1.showperson()
         print('*'*2)
         s1.showstudent()  # s1中已新建了一个字段`__taste`, 故不报错, 但类Student中没有`__taste`字段
         print('*'*3)
         
         s1.showstudent()
         print('*'*4)
         
         Student.testbug()
         
         s11 = Student('jack1', 251, 'football1')
         # 无法访问__taste,子类Student中没有__taste, 导致报错
         #s11.showstudent()
         ```

2. `name mangling` 名字重整机制

   1. 目的:  防止子类 重写 基类的方法或者属性.
   2.  `_Class__object`  可访问private

   

3. 总结:

   1. 父类中属性名为 `__名字` ，类私有,
      1.  子类不继承，子类不能访问, 
      2. 类对象不能访问, 如 `p1.__taste`报错
         1. 类的对象也没有
   2. 父类中有`__名字`, 子类中没有`__名字`,  子类的对象中向`__名字`赋值
      1. 子类的对象中会定义一个与父类中同名字的属性
      2. 但子类中仍没有`__名字`, 只是子类的对象中有
   3. `_名`的变量、函数、类, 模块私有. `from xxx import *` 不会被导入
   4. 子类没有`__init__(self,...)`时,
      1. 用父类的`__int__(self, ...)` 初始化或 创建子类对象
      2. 返回: 父类的对象. 

4. 补充

   ```
   s1 = Student('jack', 25, 'football')  //Student类没有__init__(),只能调用的父类__init__()方法, 返回父类对象转化成的子类对象, 该对象能用自己类和父类的public方法
   s1.showperson()
   ```

   输出:

   ```
   rose
   30
   football
   ```

   -----

   ````
   s1.construction('rose', 30, 'basketball', 'abc')  //调用父类__init__,并创建对象自己的属性, 但是子类中没有类自己的属性
   s1.showstudent()
   ````

   输出:

   ```
   rose
   30
   basketball
   ```






#### 12.4 import 导入模块

1. 查看模块路径(路径搜索)

   1. ```
      import sys
      sys.path
      ```

      输出 搜索路径 列表:

      1. ['D:\\PyCharm Community Edition 2020.1.1\\plugins\\python-ce\\helpers\\pydev', 'D:\\PyCharm Community Edition 2020.1.1\\plugins\\python-ce\\helpers\\third_party\\thriftpy', 'D:\\PyCharm Community Edition 2020.1.1\\plugins\\python-ce\\helpers\\pydev', 'D:\\Python3.8.3\\python38.zip', 'D:\\Python3.8.3\\DLLs', 'D:\\Python3.8.3\\lib', 'D:\\Python3.8.3', 'D:\\Python3.8.3\\lib\\site-packages', 'F:\\anconda project', 'F:\\flask', 'F:/anconda project']

   2. 路径搜索

      1. 目录列出依次要导入的`模块文件`
      2. `''` 当前路径
      3. 先后顺序 代表了`python解释器` 搜索模块时的 先后顺序

2. 模块路径中添加新模块的路径

   1. 方法: `.append()`, `.insert()`

   2. ```
      sys.path.append('/home/itcast/xxx')
      sys.path.insert(0, '/home/itcast/xxx')  # 可以确保先搜索这个路径
      ```

   3. ```
      sys.path.insert(0,"/home/python/xxxx")
      sys.path
      ```

   4. 重新导入模块

      1. 重新导入用`reload`, 不用 `import module`

         1. 初次导入用`import module`
         2. `import`只导入一次, 即是模块修改了, import也不重新导入

      2. ```
         from  imp import reload  #从imp模块中导入方法 reload()
         reload(reload_test)  # c重导入 reload_test.py模块
         ```

3. 多模块开发 注意点

   多个 py文件 开发

   1. 框图,  同一个包中

      ```
      main.py
      ┌───────────────┐
      │               │
      └───────────────┘
      
      
      recv_msg.py      ②            common.py
      ┌────────────────────┐        ┌──────────────────┐
      │ import common      │───────▲│ HANDLE_FLAG=False│
      │ common.HANDLE_FLAG │        └──────────────────┘
      └────────────────────┘               ▲HANDLE_FLAG变为True 
       此时 common.HANDLE_FLAG为True        │
                                           │
                                           │
      handle_msg.py    ①                   │
      ┌────────────────────┐               │
      │ import common      │───────────────┘
      │ common.HANDLE_FLAG =True│
      └────────────────────┘
      ```

   2. 代码 : 通用个包中

      ```
      #common.py 模块
      
      HANDLE_FLAG=False
      RECV_DATA_LIST=[]
      ```

      ```
      `# recv_msg.py模块
      
      from common import RECV_DATA_LIST   #从common模块导入RECV_DATA_LIST
      #from common import HANDLE_FLAG
      import common
      
      
      def recv_msg():
          """模拟接收到数据，然后添加到common模块中的列表中"""
          print("--->recv_msg")
          for i in range(5):
              RECV_DATA_LIST.append(i)
      
      
      def test_recv_data():
          """测试接收到的数据"""
          print("--->test_recv_data")
          print(RECV_DATA_LIST)
      
      
      def recv_msg_next():
          """已经处理完成后，再接收另外的其他数据"""
          print("--->recv_msg_next")
          # if HANDLE_FLAG:
          if common.HANDLE_FLAG:
              print("------发现之前的数据已经处理完成，这里进行接收其他的数据(模拟过程...)----")
          else:
              print("------发现之前的数据未处理完，等待中....------")
      ```

      ```
      `#handle_msg.py模块
      
      from common import RECV_DATA_LIST  #从common模块导入RECV_DATA_LIST
      # from common import HANDLE_FLAG
      import common
      
      def handle_data():
          """模拟处理recv_msg模块接收的数据"""
          print("--->handle_data")
          for i in RECV_DATA_LIST:
              print(i)
      
          # 既然处理完成了，那么将变量HANDLE_FLAG设置为True，意味着处理完成
          # global HANDLE_FLAG
          # HANDLE_FLAG = True
          common.HANDLE_FLAG = True
      
      def test_handle_data():
          """测试处理是否完成，变量是否设置为True"""
          print("--->test_handle_data")
          # if HANDLE_FLAG:
          if common.HANDLE_FLAG:
              print("=====已经处理完成====")
          else:
              print("=====未处理完成====")
      ```

      ```
      #main.py模块
      
      from recv_msg import *    #从recv_msg模块导入 
      from handle_msg import *  #从handle_msg模块导入
      
      
      def main():
          # 1. 接收数据
          recv_msg()
          # 2. 测试是否接收完毕
          test_recv_data()
          # 3. 判断如果处理完成，则接收其它数据
          recv_msg_next()
          # 4. 处理数据
          handle_data()
          # 5. 测试是否处理完毕
          test_handle_data()
          # 6. 判断如果处理完成，则接收其它数据
          recv_msg_next()
      
      
      if __name__ == "__main__":
          main()
      ```

#### 12.5  再议 封装、继承、多态

封装、继承、多态  面向对象的3大特性

2. 问什么要封装?

   1. 全局变量 , 传递方便

   2. 但, 并发程序, 对一个全局变量操作会出问题

   3. 模板, 实体
      1. 变量+函数, 模板(即 类), 提供给需求者
      2. 模板制作出 实体, 

   4. 好处

      1. 面向过程编程, 处理数据, 用 哪个模板中哪个函数

         面向对象编程, 数据存储在空间(对象),  对象通过`__class__`获得类(模板), 快速定位到 类中的 方法

      2. 全局变量 1份, 多个函数 多个备份，其它的变量储存；

         封装 储数据的变量 对象中的一个“全局”变量, 对象不一样 变量 再有1份，方便

      3. 代码划分 清晰

   5. 面向过程

      1. ```
         全局变量1
         全局变量2
         全局变量3
         ...
         
         def 函数1():
             pass
         
         def 函数2():
             pass
         
         def 函数3():
             pass
         
         def 函数4():
             pass
         
         def 函数5():
             pass
         ```

   6. 面向对象

      1. ```
         class 类(object):
             属性1
             属性2
         
             def 方法1(self):
                 pass
         
             def 方法2(self):
                 pass
         
         class 类2(object):
             属性3
             def 方法3(self):
                 pass
         
             def 方法4(self):
                 pass
         
             def 方法5(self):
                 pass
         ```

3. 为啥要继承

   1. 问题
      1.  类1 有很多代码
      2. 类2 对 类1扩充, 
         1. 方式1: 重新自己写一个`新版本`
         2. 方式2: 在类1基础上修改
   2. 好处
      1.  代码 重用率 
      2. 有效 代码的管理
         1. 当某个类有问题 修改这个类, 子类不需要 修改
   
4. 怎样理解多态

   1. ```
      class MiniOS(object):
          """MiniOS 操作系统类 """
          def __init__(self, name):
              self.name = name
              self.apps = []  # 已安装的应用程序名称列表
      
          def __str__(self):
              return "%s 安装的软件列表为 %s" % (self.name, str(self.apps))
      
          def install_app(self, app):
              # 判断是否已经安装了软件
              if app.name in self.apps:
                  print("已经安装了 %s，无需再次安装" % app.name)
              else:
                  app.install()
                  self.apps.append(app.name)
      
      
      class App(object):
          def __init__(self, name, version, desc):
              self.name = name
              self.version = version
              self.desc = desc
      
          def __str__(self):
              return "%s 的当前版本是 %s - %s" % (self.name, self.version, self.desc)
      
          def install(self):
              print("将 %s [%s] 的执行程序复制到程序目录..." % (self.name, self.version))
      
      
      class PyCharm(App):
          def install(self):
              print("正在解压缩安装程序...")
              super().install()  #调用 父类.ff()
      
      
      class Chrome(App):
          def install(self):
              print("正在解压缩安装程序...")
              super().install()  #调用 父类.ff()
      
      
      linux = MiniOS("Linux")
      print(linux)
      
      pycharm = PyCharm("PyCharm", "1.0", "python 开发的 IDE 环境")
      chrome = Chrome("Chrome", "2.0", "谷歌浏览器")
      
      linux.install_app(pycharm)
      linux.install_app(chrome)
      linux.install_app(chrome)
      
      print(linux)
      ```

      1. 输出

         ```
         Linux 安装的软件列表为 []
         将 PyCharm [1.0] 的执行程序复制到程序目录...
         正在解压缩安装程序...
         将 Chrome [2.0] 的执行程序复制到程序目录...
         已经安装了 Chrome，无需再次安装
         Linux 安装的软件列表为 ['PyCharm', 'Chrome']
         ```

         

   2. 魔法方法

      1. print输出对象 ，定义了`__str__(self)`，打印`__str__(self)`方法 return的数据
      2. 魔法方法 的触发函数
      3. 魔法方法 可子类继承

5. 重用、重写、重载

   1. 重用

      1. 定义

         1. 在对象的概念中，实现代码的重用性
         2. 继承角度, 子类继承父类, 使用父类属性方法, 即 代码重用
         3. 组合角度, 另一个类的对象作 类中一个数据属性，提高代码的重用性

      2. 重用的方式

         1. 继承

            1. 方式一：指名道姓的用另一个类中的函数（无关继承, 访问函数不存在自动传值）
               1. `CollegePeople.__init__(self, name, age, sex)`
            2. 方式二： 内置方法super()，返回特殊对象访问属性（严格依赖mro列表，存在自动传值）
               1. `super().__init__( name, age, sex)`    

         2. 组合

            1. 为一个对象添加属性，间接将两个类 关联 . 减少类与类代码冗余

               1. ```
                  class Foo:
                      xxx = 222
                  class Bar:
                      yyy = 111
                  obj = Foo()
                  b = Bar（）
                  
                  obj.attr = Bar()  #为对象obj添加attr属性, 值是Bar()对象
                  obj.a = b
                  
                  obj.xxxx #调用Foo内属性
                  obj.attr.yyyy #调用Bar内属性
                  ```

   2. 重写

      1. 重写定义

         1. 重写用于继承
         2. 子类继承父类同名函数(含同参数)，但函数体不同. 

      2. 示例

         1. ```
            class Parent:        # 定义父类
               def myMethod(self):
                  print '调用父类方法'
            
            class Child(Parent): # 定义子类
               def myMethod(self):
                  print '调用子类方法'
            
            c = Child()          # 子类实例
            c.myMethod()         # 子类调用重写方法
            ```

   3. 重载

      1. 定义
         1. 同一个类中, 函数或方法同名, 但参数列表不相同（类型不同，数量不同，位置不同）
         2. 同一个类中, 同名不同参数的函数或者方法









----

视频: 

#### 01- GIL锁

##### 01-GIL-1

1. GIL: 多线程的程序, GIL保证同一时刻只有一个线程在执行

   1. GIL是 `cpython解释器` 的历史遗留问题
   2. python3 是 python解释器, 
      1. C语言写的python解释器是 cpython, 有GIL
      2. java语言写的python解释器  jpython, 没有GIL

2. 并发: 用多进程. python线程的并发是假的

   

linux命令: 

1. ctr+shift+t  打开新vim标签
2. top ,  htop 显示 系统运行

##### 02-GIL-2

1. 多线程 比 单线程 快

   1. 单线程 阻塞 等待耗时
   2. 多线程 阻塞时执行其他线程
   3. python 多线程程序 是 单进程执行, 假的多线程执行, 但比纯单线程程序快

2.  gevent 协程

   1. 线程在某个`任务阻塞`等待时, 执行其他任务, 所以快, 效率高

3. python的多线程,协程 适合IO密集型. 多进程 适用 计算密集型

4. C语言

   1.  vim test.c  #编辑
   2.  gcc test.c   # 编译
      1.  a.out 
   3.  ./a.out  # 运行

5. python 调用 C

   1. `gcc xxx.c -shared -o libdead_loop.so`   # C语言文件编译成一个动态库的命令

   2. ```
      from ctypes import *
      from threading import Thread
      
      # 加载 动态库cdll
      lib = cdll.LoadLibrary("./libdead_loop.so")
      
      # 创建一子程序对象, 执行C语言的函数, 开启了两个线程
      t = Thread(target=lib.DeadLoop)
      t.start()
      
      # 主线程
      while True:
      	pass
      ```

   3. 如何解决GIL, 2方法

      1.  方法1: 更换 cpython解释器
      2.  方法2: 用 其他语言de函数 做python中线程的任务target的值

   4. python 可以执行C, C++, java, JavaScript

#### 02-深拷贝和浅拷贝

##### 01-深拷贝、浅拷贝-1

1. 引用(指向)

   1. ```
      a = [1, 2]  # a指向[1,2]
      b = [3, 4]  # b指向[3,4]
      c = [a, b]  # c指向[a,b],c没有创建新的[1,2],[3,4]
      ```

2. import copy, 拷贝

   1. copy.copy

      1. 浅拷贝, 拷贝的少, 只拷贝第一层, 只拷贝引用

      2. ```
         c = [a, b]
         d = copy.copy(c)  #只复制[a,b],d指向新地址, 没有拷贝a,b指向的内容,# id(c)不等于 id(d)
         ```

   2. copy.deepcopy()

      1. 深拷贝,拷贝的多, 拷贝到第二层, 拷贝值

##### 02-深拷贝, 深拷贝-2

1. ```
   a = [1, 2]
   b = [3, 4]
   c = [a, b]
   e = copy.deepcopy(c) # 内存生成新空间,新值是[a,b],新a指向新[1,2], 新b指向新[3,4]
   ```

   

##### 02-深拷贝、浅拷贝-2

##### 03-深拷贝、浅拷贝-3

1. ```
   a = [1, 2]
   b = [3, 4]
   c = [a, b]
   d = copy.copy(c)
   e = copy.deepcopy(c)
   c.append([5,6])
   ------
   输出:
   c : [[1, 2], [3, 4], [5, 6]]
   d: [[1, 2], [3, 4]]
   e: [[1, 2], [3, 4]]
   ```

2.  不可变类型, 

   1. deepcopy(), copy都是指向, 是引用
   2. 不产生新空间

3.  可变类型

   1. 深拷贝产生新空间,  浅拷贝产生新空间
   2. 深拷贝,拷贝的多, 浅拷贝拷贝的少

4. 不可变(可变, 可变)

   1. 浅拷贝, 不产生新空间, 发生指向
   2. 深拷贝, 产生新空间, 拷贝的多, 发生深拷贝, 值复制新空间

5. 可变[(不可变), (不可变)]

   1. 浅拷贝, 产生新空间,  深层不产生新空间, 发生指向
   2. 深拷贝,产生新空间, 深层不产生新空间, 发生指向

##### 04-深拷贝、浅拷贝-4

1. list切片, 是浅拷贝, 

   1. 二维和多维列表修改内部元素后原列表的结果可以看出，列表的切片是属于浅拷贝，等同于list.copy()和copy.copy()的效果

   2. ```
      #多维list
      list1 = [[1,2,3],[4,5,6]]
      list2 = list1[1:]
      list2
      Out[10]: [[4, 5, 6]]
      list2[0][1] = 100
      list2
      Out[12]: [[4, 100, 6]]
      list1
      Out[13]: [[1, 2, 3], [4, 100, 6]]
      原文链接：https://blog.csdn.net/m0_49397655/article/details/115351693
      ```

   3. ```
      #一维列表
      list1 = [1,2,3,4,5,6]
      list2 = list1[1:5]
      list2
      Out[4]: [2, 3, 4, 5]
      list2[0] = 100
      list2
      Out[6]: [100, 3, 4, 5]
      list1
      Out[7]: [1, 2, 3, 4, 5, 6]
      -------------
      原文链接：https://blog.csdn.net/m0_49397655/article/details/115351693
      ```

2. dict.copy(), 是浅拷贝, 深层是指向
   1. 键存于字典, 值存另外地方, 键指向值
   2. copy的是键, 不是值

-----

#### 03-私有化、import、封装继承多态

##### 01-私有化

1. 前两个下划线, `__a`, 类私有, 避免与子类属性命名冲突, 无法外部直接访问

   1. 类私有, 类内可以用. 类实例对象没有, 不能调用

2. 前后两个下划线, `__b__`,  魔法方法, 可继承

3. 前 1 个下划线, `_a`,  模块中私有化属性或函数(不是类的私有), from somemodule import * ,不能导入`_a`, 

   `_a`在类中, 可以被子类继承, protected

   '_a()` 在类中, 可被子类继承, protected

4. 后 1 个下划线, `a_`, 避免与Python关键字冲突

##### 02-import导入模块

1. 导入

   import  * 

   from aa import *

   from aa import A

   from aa as aa1

2. 查看`路径`

   import sys

   sys.path   # 返回list

   sys.path.insert(4, '/home/itcast/xxx')  # 下标位置4添加路径

   sys.path.appden("/home/itcast/xxx")  # 最后加入路径

3. 模块重载

   1. from imp import reload

      import aa

      reload(aa)  #重组模块名aa, 不加引号

      必须import 可以加载函数reload(),  

      from 不可以加载函数reload(),  from后接模块名

   2. import 防止模块重复加载

##### 03-多个模块import导入注意点

1. import 

   1. ```
      #handle_msg.py中
      import common  #作用: 本模块中创建本地变量common, 并让它指向common.py模块
      import common import RECV_DATA_LISt #在本模块中创建本地变量RECV_DATA_LISt指向common.py模块中的RECV_DATA_LISt变量的引用.
      ```

      1. 主要两者区别

##### 04-再议封装、继承、多态

1. 封装

   1.  类中函数 各实例共享

   2.  类中数据 各实例自有的

   3. ```
      t = T()
      # 属性
      t.__dict__
      t.
      ```

   4. ```
      class T():
      	name = 'aa'  # 类变量
      	def __init__(self, age)
      		self.age = age  #self.age 对象变量
      ```

      
      

2. 继承

   1.  重复代码
   2.  提高效率
   3.  python 参数传递的是引用

3. 多态

   1. 子类重写了父类的方法a, 
      1. 子类对象调用a, 执行的是子类的方法
      2. 若没有重新a, 执行的是继承自父类的方法

-----

### 13.python提升-2

#### 13.1 多继承以及MRO顺序

​	一个`子类`继承多个`父类`

1. ㊀ 单独调用父类的方法,  子类中 `父类名.__init__(self, 其他参数)` 语句

   1. ```
      print("******多继承使用类名.__init__ 发生的状态******")
      class Parent(object):
          def __init__(self, name):
              print('parent的init开始被调用')
              self.name = name
              print(self.name, 1)
              print('parent的init结束被调用')
      
      class Son1(Parent):
          def __init__(self, name, age):
              print('Son1的init开始被调用')
              self.age = age
              Parent.__init__(self, name)
              print('Son1的init结束被调用')
      
      class Son2(Parent):
          def __init__(self, name, gender):
              print('Son2的init开始被调用')
              self.gender = gender
              Parent.__init__(self, name)
              print('Son2的init结束被调用')
              
      class Son3(Parent):
          def __init__(self, name, age):
              print('Son1的init开始被调用')
              self.age = age
              self.name = name
              print('Son1的init结束被调用')
              
          def pri(self):
              Parent.__init__(self, self.name)  #这里也可以调用父类的一个函数
      
      class Grandson(Son1, Son2):
          def __init__(self, name, age, gender):
              print('Grandson的init开始被调用')
              Son1.__init__(self, name, age)  # 单独调用父类的初始化方法
              Son2.__init__(self, name, gender)
              print('Grandson的init结束被调用')

      gs = Grandson('grandson', 12, '男')
      print('姓名：', gs.name)
      print('年龄：', gs.age)
      print('性别：', gs.gender)
      
      print("******多继承使用类名.__init__ 发生的状态******\n\n")
      ```
      
      输出:
      
      ```
      ******多继承使用类名.__init__ 发生的状态******
      Grandson的init开始被调用
      Son1的init开始被调用
      parent的init开始被调用
      grandson 1
      parent的init结束被调用
      Son1的init结束被调用
      Son2的init开始被调用
   parent的init开始被调用
      grandson 1
      parent的init结束被调用
      Son2的init结束被调用
      Grandson的init结束被调用
      姓名： grandson
      年龄： 12
      性别： 男
      ******多继承使用类名.__init__ 发生的状态******
      ```
      
   2. 总结:

      1. 子类的对象中调用继承的父类的方法, 处理的是子类对象的数据

2. ㊁ 多个父类继承, super调用有所父类的被重写的方法, 子类中  `super.__init__(self, 其他参数)` 

   1. 父类1 `__init__(self, name, *args, **kwargs)` 不定长参数,避免多继承报错

      父类2 `__init__(self, name, age, *args, **kwargs)`  , `__init__`不定长参数

   2. ```
      print("******多继承使用super().__init__ 发生的状态******")
      class Parent(object):
          def __init__(self, name, *args, **kwargs):  # 为避免多继承报错，使用不定长参数，接受参数
              print('parent的init开始被调用')
              self.name = name
              print('parent的init结束被调用')
      
      class Son1(Parent):
          def __init__(self, name, age, *args, **kwargs):  # 为避免多继承报错，使用不定长参数，接受参数
              print('Son1的init开始被调用')
              self.age = age
              super().__init__(name, *args, **kwargs)  # 为避免多继承报错，使用不定长参数，接受参数
              print('Son1的init结束被调用')
      
      class Son2(Parent):
          def __init__(self, name, gender, *args, **kwargs):  # 为避免多继承报错，使用不定长参数，接受参数
              print('Son2的init开始被调用')
              self.gender = gender
              super().__init__(name, *args, **kwargs)  # 为避免多继承报错，使用不定长参数，接受参数
              print('Son2的init结束被调用')
      
      class Grandson(Son1, Son2):
          def __init__(self, name, age, gender):
              print('Grandson的init开始被调用')
              # 多继承时，类名.__init__方法，要把每个父类全部写一遍
              # super只用一句话，执行了全部父类的方法，这也是为何多继承需要全部传参的一个原因
              # super(Grandson, self).__init__(name, age, gender)
              super().__init__(name, age, gender)
              print('Grandson的init结束被调用')
      
      print(Grandson.__mro__)
      
      gs = Grandson('grandson', 12, '男')
      print('姓名：', gs.name)
      print('年龄：', gs.age)
      print('性别：', gs.gender)
      print("******多继承使用super().__init__ 发生的状态******\n\n")
      ```

      1. 输出

         ```
         Grandson的init开始被调用
         Son1的init开始被调用
         Son2的init开始被调用
         parent的init开始被调用
         parent的init结束被调用
         Son2的init结束被调用
         Son1的init结束被调用
         Grandson的init结束被调用
         ```

      2. 为什么要加super呢?

         1. 子类的`__init__`方法重写了父类的`__init__`方法, 即覆盖了父类的初始化方法. 
         2. 加`super().__init__`,  调用父类的`__init__`, 实现初始化.
      
   3. 总结:

      1. 子类中, `super().__init__(参数1, 参数2,...)`中的参数 对应父类的`__init__`方法中的参数, 一个也不能少

      2. super() 下面等价的

         ```
         super(Grandson, self).__init__(name, age, gender)   # ①
         super().__init__(name, age, gender)  # ②
         ```

         1. ① 中去掉 Grandson, 变成

            `super(self).__init__(name, age, gender)`

            报错: `TypeError: super() argument 1 must be type, not Grandson`

      3. `__mro__`,  类的魔法函数, `Grandson.__mro__` 正确,  `gs.__mro__`报错

   4. 注意

      1. ㊀ ㊁ 2个代码执行的结果不同
      2. 2个子类 继承了父类，子类中 `父类名.方法名(实参类别)`调用，Parent类 执行 2 次(son1,son2各调用了一次`Parent.__init__()`)
      3. 2个子类 继承了父类，子类中 `super().方法名(实参列表)`调用，Parent类被执行 1 次

3. 单继承中super,  一个`子类`只继承一次同一个`父类`

   1. ```
      print("******单继承使用super().__init__ 发生的状态******")
      class Parent(object):
          def __init__(self, name):
              print('parent的init开始被调用')
              self.name = name
              print('parent的init结束被调用')
      
      class Son1(Parent):
          def __init__(self, name, age):
              print('Son1的init开始被调用')
              self.age = age
              super().__init__(name)  # 单继承不能提供全部参数
              print('Son1的init结束被调用')
      
      class Grandson(Son1):
          def __init__(self, name, age, gender):
              print('Grandson的init开始被调用')
              super().__init__(name, age)  # 单继承不能提供全部参数
              print('Grandson的init结束被调用')
      
      gs = Grandson('grandson', 12, '男')
      print('姓名：', gs.name)
      print('年龄：', gs.age)
      #print('性别：', gs.gender)
      print("******单继承使用super().__init__ 发生的状态******\n\n")
      ```

4. `Grandson.__mro__`    # mro, method resolution order(方法解析顺序)

5. 总结`super().__init__`  `类名.__init__`

   1.  单继承, 用法 无差
   2. 多继承 有区别
      1. `super().方法` 父类的方法只 执行一次
      2. `类名.方法`  方法 执行多次
   3. 多继承,  super()方法，对父类的传参数， python中super算法导致的原因，要全部传递参数，否则 报错
   4. 单继承, 使用super方法，只传父类方法所需的参数，否则 报错
   5. 多继承时，`父类名.__init__`方法，父类全部写一遍.  super方法，只 写一句话 执行全部父类的方法.  多继承 全部传参的 原因

6. 小试牛刀(以下为面试题)

   1. ```
      class Parent(object):
          x = 1
      
      class Child1(Parent):
          pass
      
      class Child2(Parent):
          pass
      
      print(Parent.x, Child1.x, Child2.x)
      Child1.x = 2
      print(Parent.x, Child1.x, Child2.x)
      Parent.x = 3
      print(Parent.x, Child1.x, Child2.x)
      ```

      1. 输出

         ```
         1 1 1
         1 2 1
         3 2 3
         ```

   2. 分析

      1. 答案的关键是

         1. 在 Python 中，`类变量`作为字典处理的
         2. 一个变量的名字在当前类的字典中没有发现，将搜索祖先类（比如父类）直到被引用的变量名被找到
         3. 被引用的变量名既没有在`自己`所在的类又没有在`祖先类`中找到，引发一个 AttributeError 异常 

      2. 父类中设置 x = 1 使得类变量 x 在引用该类和其任何子类中的值为 1。这就是因为第一个 print 语句的输出是 1 1 1。

         随后，某个子类重写了该值（例如, Child1.x = 2）， 该值仅在子类中被改变。故第二个 print 语句的输出是 1 2 1。

         最后，该值在父类中被改变（例如，Parent.x = 3），这影响到未重写该值的子类的值（示例中影响子类 Child2）。故第三个 print 输出是 3 2 3。

#### 13.2 再论静态方法和类方法

1. 类属性、实例属性

   1. 概述

      1. 区别与联系
         1. 本质区别:内存中 位置不同
         2. 实例属性 属于 对象, 定义在`__init__()`中
         3. 类属性 属于 类,  定义在类开头
         4. 都用 字典{键:值, ....} 方式存储
      2. 查看属性 `dir(对象名)`,  `对象名.__dict__`
         1. dir返回list, 包含继承来的属性，
         2. `__dict__` 自己的属性
            1. `类名.__dict__`  类的属性,方法, 不包含实例的属性(self.属性)
            2. `实例.__dict__`  #实例的属性, 包含继承的实例的属性, 不包含类的属性

   2. 示例

      ```
      class Province(object):
          #类属性
          country = '中国'
      
          def __init__(self, name):
              # 实例属性
              self.name = name
      
      
      # 创建一个实例对象
      obj = Province('山东省')
      # 直接访问实例属性
      print(obj.name)
      # 直接访问类属性
      print(Province.country)   #类名.属性名 , 中国
      print(obj.country)    #实例名.属性名, 中国
      ```

      1. 分析
         1. 实例属性通过 实例名 访问
         2. 类属性通过 类名 访问
         3. 实例属性和类属性 归属 不同

   3. 存储方式

      1. ```
         类对象 Province                    实例对象1
         ┌─────────────────────────┐       obj1=Province("山东")
         │country="中国"            │\      ┌─────────────────────────┐
         │─────────────────────────│ | \   │name="山东"            │
         │def __init__(self,name): │ |  \  └─────────────────────────┘
             │self.name=name       │ |    \│默认的属性等               │
         │def other_func(self,...):│ |     │__class__指向类对象        │
         └─────────────────────────┘ |     └─────────────────────────┘
                                     |
                                     |     实例对象2
                                     \     obj1=Province("山西")
                                       \   ┌─────────────────────────┐
                                         \ │name="山西"            │
                                           └─────────────────────────┘
                                           │默认的属性等               │
                                           │__class__指向类对象        │
                                           └─────────────────────────┘
         ```

      2. 结论

         1. 类属性 内存 只一份
         2. 实例属性 每个对象中都 保存一份

      3. 应用场景

         1. 类创建实例对象, 如 每个对象 有相同名字的属性， 用类属性，一份既可

2. 实例方法, 静态方法, 类方法

   1. 3种方法在 内存   都属于类,  调用方式不同
      1. 实例方法

         1. 无 修饰符
2. 参数: 有 `self` 默认参数
         3. `对象.方法名(实参)`调用,   `对象名` 自动赋值给`self`；

     ​       

      2. 类方法

         1. `@classmethod` 修饰符
         2. 参数:  `cls` 默认参数
   3. `类名.方法(实参)`,  `对象.方法名(实参)`；执行时，`类名`自动赋值给`cls`；
   
3. 静态方法
   
         1. `@staticmethod`  修饰符
   2. 无 默认参数
      
   3. `类名.方法(实参)`,  `对象.方法名(实参)`；
      
   4. 示例
   
      1. ```
         class Foo(object):
             def __init__(self, name):
                 self.name = name
         
             def ord_func(self):
                 """ 定义实例方法，至少有一个self参数 """
                 # print(self.name)
                 print('实例方法')
         
             @classmethod
             def class_func(cls):
                 """ 定义类方法，至少有一个cls参数 """
                 print('类方法')
         
             @staticmethod
             def static_func():
                 """ 定义静态方法 ，无默认参数"""
                 print('静态方法')
         
         
         
         f = Foo("中国")
         # 调用实例方法
         f.ord_func()
         
         # 调用类方法
         Foo.class_func()
         
         # 调用静态方法
      Foo.static_func()
         ```
      ```
      
         1. 输出:
      
      ```
            实例方法
            类方法
         静态方法
         ```
      
         
         ```
   
   3. 总结--实例方法, 类方法, 静态方法
   
      1. 相同点, 所有的方法(静态方法, 类方法, 实例方法)  均属于类,  内存 只有一份
      2. 不同点, 调用者不同, 调用方法时 自动传入的参数不同 

--------

#### 13.3 property属性

1. 什么是property属性

   1. 用属性一样用`方法`. 像实例属性一样的特殊属性, 对应于某个方法

      1. 定义:  `@property` 修饰器,   有`self`参数, 没有其他参数

      2. 使用:   `对象名.方法名`,  方法名没有括号, 像 `实例名.属性名`

      3. 使用区别

         方法：   `foo_obj.func()`
         property属性：  `foo_obj.prop`

2. 分页技术

   1. 一页 `列表页面数据` : 向数据库请求从第m条到第n条的数据, 用于显示

   2.  分页的功能

      1. 根据用户请求的 `当前页`和 `每页默认条数` 计算出 m 和 n
      2. 根据m 和 n 去数据库中请求数据

   3. ```
      class Pager:
          def __init__(self, current_page):
              # 用户当前请求的页码（第一页、第二页...）
              self.current_page = current_page
              # 每页默认显示10条数据
              self.per_items = 10 
      
          @property
          def start(self):
              val = (self.current_page - 1) * self.per_items
              return val
      
          @property
          def end(self):
              val = self.current_page * self.per_items
              return val
      ```

      1. 调用

         ```
         p = Pager(1)
         p.start  #0, 就是起始值，即：m
         p.end  #10, 就是结束值，即：n
         ```

   4. property属性功能:  property属性内 进行逻辑计算，并将结果返回

      1. `property`属性不能  ` 对象名.property方法名()`  形式调用, 否则报错

   

3. property属性

   1. 创建两种方式 : 装饰器方式, 类属性方式

   2. `装饰器` 方式

      1. 装饰器 方式,  用`@property`装饰器 修饰方法

         1. 又两种方式: python2`经典类`和 python3`新式类

      2. 经典类，一种@property装饰器, 对应 python2

         1. ```
            class A():
            	@property
            	def property_func(self):
            		pass
            ```
            
         2. 示例: 

            ```
            `# ############### 定义 ###############    
            class Goods:
                @property
                def price(self):
                    return "laowang"
            `# ############### 调用 ###############
            obj = Goods()
            result = obj.price  # 自动执行 @property 修饰的 price 方法，并获取方法的返回值
            print(result)
            ```

            1. `自动`执行 @property 修饰的 `price 方法`，并获取方法的返回值

      3. 新类, 三种`@property`装饰器,  对应 python3, 

         1. 新类， `@property`、`@方法名.setter`、`@方法名.deleter` 修饰的三种方法 定义
            
            1. ```
               class A():
               	@property
               	def property_func():
               		pass
               		
               	@property_func.setter
               	def property_func(self, value):
               		pass
               	
               	@property_func.deleter
               	def property_func(self):
               		pass
               ```
            
         2. 示例

            ```
            #coding=utf-8
            # ############### 定义 ###############
            class Goods:
                """python3中默认继承object类
                    以python2、3执行此程序的结果不同，因为只有在python3中才有@xxx.setter  @xxx.deleter
                """
                @property
                def price(self):
                    print('@property')
            
                @price.setter
                def price(self, value):
                    print('@price.setter')
            
                @price.deleter
                def price(self):
                    print('@price.deleter')
            
            # ############### 调用 ###############
            obj = Goods()
            obj.price          # 自动执行 @property 修饰的 price 方法，并获取方法的返回值
            obj.price = 123    # 自动执行 @price.setter 修饰的 price 方法，并将  123 赋值给方法的参数
            del obj.price      # 自动执行 @price.deleter 修饰的 price 方法
            ```

         3. 三种方法 定义

            1. ```
               @property
               def 方法名(self):
               ```

            2. ```
               @方法名.setter
               def 方法名(self, 一个参数):
               ```

            3. ```
               @方法名.deleter
               def 方法名(self):
               ```
               
            4. 三个装饰器的方法名相同

         4. 新类, 三个装饰器的方法名相同：获取, 修改, 删除. (人为规定的). 

            ```
            class Goods(object):
            
                def __init__(self):
                    # 原价
                    self.original_price = 100
                    # 折扣
                    self.discount = 0.8
            
                @property
                def price(self):
                    # 实际价格 = 原价 * 折扣
                    new_price = self.original_price * self.discount
                    return new_price
            
                @price.setter
                def price(self, value):
                    self.original_price = value
            
                @price.deleter
                def price(self):
                    del self.original_price
            
            obj = Goods()
            obj.price         # 获取商品价格
            obj.price = 200   # 修改商品原价
            del obj.price     # 删除商品原价
            ```

         5. 三种方法  访问 (小结)

            1. `对象名.方法名`
            2. `对象名.方法名=xxx`
            3. `del 对象名.方法名`
            4. 附加函数与原始的特征属性相同的名称

   3. `类属性` 方式，创建 值为`property对象` 的类属性

      1. property() 类 解析

         1. ```
            class property(object):
            	property(fget=None, fset=None, fdel=None, doc=None)
            ```
            
            1. 类Property属性
            
               1. fget, function to be used for getting an attribute value
               2. fset, function to be used for setting an attribute value
               3. fdel, function to be used for del'ing an attribute
               4. doc, docstring
            
               
            
         2. 类内函数: `property(fget=None, fset=None, fdel=None, doc=None)`

            1. 参数
               1. fget, 获取属性值的函数， `对象.属性`  自动触发执行方法
               2. fset, 设置属性值的函数，`对象.属性 ＝ XXX` 自动触发执行方法
               3. fdel,  删除属性值的函数，`del 对象.属性` 自动触发执行方法
               4. doc, 为属性对象创建文档字符串，`对象.属性.__doc__`   该属性的描述信息
                  1. `对象.属性.__doc__`,  是属性, 没有`()`
            2. 返回值: property 属性
               1. 返回的特征属性对象 具有与构造器参数 对应的属性 fget, fset 和 fdel
               2. 用`对象名.__dict__` 查看 与构造器参数 对应的属性 fget, fset 和 fdel具体形式

         3. 类属性的三个方法 对 同一个属性的三个操作：获取、修改、删除

      2. 示例

         1. ```
            class C:
            	def __init__(self):
            		self._x = None
                def getx(self):
                    return self._x
            
                def setx(self, value):
                    self._x = value
            
                def delx(self):
                    del self._x
            
                x = property(getx, setx, delx, "I'm the 'x' property.")
            ```
            
      2.  c 是 C 的实例，c.x 将调用getter，c.x = value 将调用setter， del c.x 将调用deleter
         
         ```
            c = C
            c.x
            c.x = 5
            del c.x
            ```

   4. 两种方式对比, 示例

      1. ```
         #类属性，即在类中定义为值为Property对象的类属性
         class Person(object):
             def __init__(self):
                 self.__age=18    #定义私有的属性
             def get_age(self):   #访问私有实例属性
                 return self.__age
             def set_age(self,age):
                 if age<0:
                     print('年龄不能小于0')
                 else:
                     self.__age=age
             age=property(get_age,set_age)     #定义一个属性，当对这个age设置值时调用set_age,
         xiaoming=Person()                     #当获取值时调用get_age,顺序不能调换
         print(xiaoming.age)
         xiaoming.age=15                       #注：必须是以set_age或get_age开头的方法名，才能被调用
         print(xiaoming.age)
         
         
         print('-----------------------------------------------')
         #通过装饰器的方式实现
         class Person(object):
             def __init__(self):
                 self.__age=18    #定义私有的属性
             @property         #使用装饰器对age进行装饰，提供一个getter方法
             def age(self):   #访问私有实例属性
                 return self.__age       #直接返回函数的默认值
             @age.setter        #使用装饰器进行修饰，提供一个setter方法
             def age(self,age):     #函数被赋值的时候会调用本方法
                 if age<0:
                     print('年龄不能小于0')
                 else:
                     self.__age=age
         xiaoming=Person()                     #当获取值时调用get_age,顺序不能调换
         print(xiaoming.age)
         xiaoming.age=30                     #注：必须是以set_age或get_age开头的方法名，才能被调用
         print(xiaoming.age)
         
         ```

         

4. Django框架中应用了property属性（了解）

   WEB框架 Django 的视图中 request.POST  使用 `类属性`方式 创建的属性

   1. ```
      class WSGIRequest(http.HttpRequest):
          def __init__(self, environ):
              script_name = get_script_name(environ)
              path_info = get_path_info(environ)
              if not path_info:
                  # Sometimes PATH_INFO exists, but is empty (e.g. accessing
                  # the SCRIPT_NAME URL without a trailing slash). We really need to
                  # operate as if they'd requested '/'. Not amazingly nice to force
                  # the path like this, but should be harmless.
                  path_info = '/'
              self.environ = environ
              self.path_info = path_info
              self.path = '%s/%s' % (script_name.rstrip('/'), path_info.lstrip('/'))
              self.META = environ
              self.META['PATH_INFO'] = path_info
              self.META['SCRIPT_NAME'] = script_name
              self.method = environ['REQUEST_METHOD'].upper()
              _, content_params = cgi.parse_header(environ.get('CONTENT_TYPE', ''))
              if 'charset' in content_params:
                  try:
                      codecs.lookup(content_params['charset'])
                  except LookupError:
                      pass
                  else:
                      self.encoding = content_params['charset']
              self._post_parse_error = False
              try:
                  content_length = int(environ.get('CONTENT_LENGTH'))
              except (ValueError, TypeError):
                  content_length = 0
              self._stream = LimitedStream(self.environ['wsgi.input'], content_length)
              self._read_started = False
              self.resolver_match = None
      
          def _get_scheme(self):
              return self.environ.get('wsgi.url_scheme')
      
          def _get_request(self):
              warnings.warn('`request.REQUEST` is deprecated, use `request.GET` or '
                            '`request.POST` instead.', RemovedInDjango19Warning, 2)
              if not hasattr(self, '_request'):
                  self._request = datastructures.MergeDict(self.POST, self.GET)
              return self._request
      
          @cached_property
          def GET(self):
              # The WSGI spec says 'QUERY_STRING' may be absent.
              raw_query_string = get_bytes_from_wsgi(self.environ, 'QUERY_STRING', '')
              return http.QueryDict(raw_query_string, encoding=self._encoding)
      
          # ############### 看这里看这里  ###############
          def _get_post(self):
              if not hasattr(self, '_post'):
                  self._load_post_and_files()
              return self._post
      
          # ############### 看这里看这里  ###############
          def _set_post(self, post):
              self._post = post
      
          @cached_property
          def COOKIES(self):
              raw_cookie = get_str_from_wsgi(self.environ, 'HTTP_COOKIE', '')
              return http.parse_cookie(raw_cookie)
      
          def _get_files(self):
              if not hasattr(self, '_files'):
                  self._load_post_and_files()
              return self._files
      
          # ############### 看这里看这里  ###############
          POST = property(_get_post, _set_post)
      
          FILES = property(_get_files)
          REQUEST = property(_get_request)
      ```

5. 小结

   1. 定义property属性 `两种`方式
      1. `装饰器`, `类属性`, 
      2. `装饰器` 方式又分 经典类 新式类 两种不同方式。
   2. property属性，能 简化 获取数据的流程

#### 13.4  property属性-应用

1. 实现一个属性的设置和读取方法,可做边界判定

   1. ```
      class Money(object):
          def __init__(self):
              self.__money = 0
      
          # 使用装饰器对money进行装饰，那么会自动添加一个叫money的属性，当调用获取money的值时，调用装饰的方法
          @property
          def money(self):
              return self.__money
      
          # 使用装饰器对money进行装饰，当对money设置值时，调用装饰的方法
          @money.setter
          def money(self, value):
              if isinstance(value, int):
                  self.__money = value
              else:
                  print("error:不是整型数字")
      
      a = Money()
      a.money = 100
      print(a.money)
      ```

   2. `@property`,  `@money.setter`  装饰器 对 原来方法添加 property属性功能



#### 13.5 魔法属性

1. 类的魔法方法

   1. ```
      __doc
      __module__
      __class__
      __init__
      __del__
      __call__
      __dict__
      __str__
      ```

   2. 先有创建`__new__`，才有初始化`__init__`, 使用中实例化对象被调用`__call__`。即先`__new__`，而后`__init__`，最后`__call__`。

      1. 新建函数 `__new__`

         1 继承自object的新式类（元类）才有`__new__` .

         2 `__new__`要有参数cls，代表当前类， 参数cls在实例化时由Python解释器自动识别

      2. `__call__`

         1. `__call__`把实例当作函数一样的调用

            1. 同时不影响实例本身的生命周期(`__call__()`不影响一个实例的构造和析构）。
            2. `__call__`可以用来改变实例的内部成员的值

         2. 没有`__call__` 示例

            1. ```
               #没有__call__
               class Student():
                   #初始化类
                   def __init__(self, name, age):
                       self.name = name
                       self.age = age
                   
                   def __str__(self):
                       #格式化输出
                       return f'{self.name}的年龄是{self.age}'
                
               #实例化一个新的学生
               student_1 = Student('bill', 12)
                
               #打印学生信息
               #如果没有__str__打印的是地址，有__str__那就返回的是自定义的语句
               print(student_1) =>bill的年龄是12
                
                
               #打印学生名字
               print(student_1.name) #=> bill
                
               #打印学生年龄
               print(student_1.age) #=> 12
                
               #但是我想更改年龄怎么办？
               student_1.age = 34
                
               #打印学生信息
               print(student_1) #=>bill的年龄是34
                
               #student_1(34) #实例名(实参),像函数一样调用，报错的
               ```

         3. 有`__call__`

            1. ```
               #新建一个类
               class Student():
                       #进行初始化
                       def __init__(self, name, age, ):
                           self.name = name
                           self.age = age
                       
                       def __call__(self, age):
                           self.age = age
                       
                       def __str__(self):
                           return f'{self.name}的年龄是{self.age}'
                
                
               #实例化一个学生出来
               student1 = Student('bill', 12)
               print(student1)
                
               #现在进行年龄的更改，就可以象调用方法一样的调用了。
               student1(34)   ##实例名(实参),像函数一样调用. 正确
               print(student1)  #34
               ```

         4. 小结

            1. `__new__`： 实例的创建, 申请内存空间, 一个静态方法，第一个参数是cls。
            2. `__init__ `： 实例的初始化，内存空间填充值,  是一个实例方法，第一个参数是self，初始化属性，这个属性可以设默认值。
            3. `__call__` ： 实例像被函数一样调用。定义了`__call__`方法, `实例名()` 可被调用，否则是不可以的， 不能设默认值。

2. `__doc__`

   1. 类的描述信息, `"""  """` 中的内容

   2. ```
      class Foo:
          """ 描述类信息，这是用于看片的神奇 """
          def func(self):
              pass
      
      print(Foo.__doc__) 
      ```

      1. 返回值: `描述类信息`

3. `__module__` 和` __class__`

   1. `__module__`  当前的 对象/类 在那个模块, `__main__` 当前模块

   2. `__class__ `  当前的 对象/类 的类是什么

   3. 示例

      ```
      # main.py
      
      from test import Person
      
      obj = Person()
      print(obj.__module__)  # 输出 test 即：输出模块(包名)
      print(obj.__class__)  # 输出 test.Person 即：输出类
      ```

4. `__init__`

   1. 初始化方法.  类**创建对象**时，自动触发执行

   2. ```
      class Person:
          def __init__(self, name):
              self.name = name
              self.age = 18
      
      
      obj = Person('laowang')  # 自动执行类中的 __init__ 方法
      ```

5. `__del__`

   1. 内存中, 对象被释放时, 自动触发执行

   2. ```
      class Foo:
          def __del__(self):
              pass
      ```

   3. 注意

      1. `__del__`无须定义
      2. Python 高级语言，程序员 无需关心`内存`的分配和释放, 此工作交给Python解释器 执行
      3. `__del__`调用 由解释器在 垃圾回收时 自动触发执行

6. `__call__`

   1. 对象后 加`括号`，触发执行,  `对象()` 或者 `类()()`

      1. `__init__`方法  **创建对象** 触发的,  `对象 = 类名()`

   2. ```
      class Foo:
          def __init__(self):
              pass
      
          def __call__(self, *args, **kwargs):
              print('__call__')
      
      
      obj = Foo()  # 执行 __init__, 类名() 触发
      obj()  # 执行 __call__,  对象() 触发
      ```

7. `__dict__`

   1. 类或对象 的所有`属性`

      1. `实例名.__dict__`, 实例属性属于实例,  只含实例属性, 不含方法
      2. `类名.__dict__`, 类属性和方法等属于类,  含 类属性, 类方法

   2. 示例

      ```
      class Province(object):
          country = 'China'
      
          def __init__(self, name, count):
              self.name = name
              self.count = count
      
          def func(self, *args, **kwargs):
              print('func')
      
      # 获取类的属性，即：类属性、方法、
      print(Province.__dict__)
      # 输出：{'__module__': '__main__', 'country': 'China', '__init__': <function Province.__init__ at 0x00000000038A3820>, 'func': <function Province.func at 0x00000000038A3940>, '__dict__': <attribute '__dict__' of 'Province' objects>, '__weakref__': <attribute '__weakref__' of 'Province' objects>, '__doc__': None}
      
      obj1 = Province('山东', 10000)
      print(obj1.__dict__)
      # 获取 对象obj1 的属性
      # 输出：{'count': 10000, 'name': '山东'}
      
      obj2 = Province('山西', 20000)
      print(obj2.__dict__)
      # 获取 对象obj1 的属性
      # 输出：{'count': 20000, 'name': '山西'}
      ```

8. `__str__`

   1. 类中定义了`__str__`方法，print 对象 时， 输出该方法的返回值

   2. ```
      class Foo:
          def __str__(self):
              return 'laowang'
      
      
      obj = Foo()
      print(obj)
      # 输出：laowang
      ```

9. `__getitem__`, `__setitem__`,  `__delitem__`

   1. 如字典, 索引操作自动触发. 获取、设置、删除数据

   2. ```
      class Foo(object):
      
          def __getitem__(self, key):
              print('__getitem__', key)
      
          def __setitem__(self, key, value):
              print('__setitem__', key, value)
      
          def __delitem__(self, key):
              print('__delitem__', key)
      
      
      obj = Foo()
      
      result = obj['k1']      # 自动触发执行 __getitem__
      obj['k2'] = 'laowang'   # 自动触发执行 __setitem__
      del obj['k1']           # 自动触发执行 __delitem__
      ```

10. `__getslice__`,  `__setslice__`,  `__delslice__`

   1. 如 列表, 分片操作  自动触发, 获取 设置 删除

   2. ```
      class Foo(object):
      
          def __getslice__(self, i, j):
              print('__getslice__', i, j)
      
          def __setslice__(self, i, j, sequence):
              print('__setslice__', i, j)
      
          def __delslice__(self, i, j):
              print('__delslice__', i, j)
      
      obj = Foo()
      
      obj[-1:1]                   # 自动触发执行 __getslice__
      obj[0:1] = [11,22,33,44]    # 自动触发执行 __setslice__
      del obj[0:2]                # 自动触发执行 __delslice__
      ```


#### 13.6 面向对象设计

1. 面向对象

   1. 继承 - 基于类的 `属性`	(如X.name)
   2. 多态 - `X.method()`方法，method的意义取决于X的类型
   3. 封装 - `方法`和`运算符` 实现 `行为`，`数据隐藏` 是 默认惯例

##### 1 腾讯即时通信模块

1. 参考实例  -- 腾讯即时通信模块,初级封装.  部分代码，学习`类的设计`

   1. ```
      `#! /usr/bin/env python
      `# coding: utf-8
      
      import random
      import time
      
      
      class Message(object):
      
          def __init__(self, msgarr=[], toacc=''):
              self.msgbody = msgarr # 此处为MsgDict对象实例的列表或者空列表
              self.toacc = toacc # toacc为字符串(单发)或者列表(批量发)
              self.msgrandom = random.randint(1, 1000000000)
              self.msgrequest = {
                  'To_Account': toacc, # 消息接收方账号
                  'MsgRandom': self.msgrandom, # 消息随机数，由随机函数产生
                  'MsgBody': [t.msg for t in msgarr]
              }
      
          def del_option(self, option):
              if option in (set(self.msgrequest)-set(['To_Account', 'MsgRandom', 'MsgBody'])):
                  self.__dict__.pop(option)
                  self.msgrequest.pop(option)
      
          def append_msg(self, msg):
              self.msgbody.append(msg)
              self.msgrequest['MsgBody'].append(msg.msg)
      
          def insert_msg(self, index, msg):
              self.msgbody.insert(index, msg)
              self.msgrequest['MsgBody'].insert(msg.msg)
      
          def del_msg(self, index):
              if index in range(len(self.msgbody)):
                  del self.msgbody[index]
                  del sel.msgrequest['MsgBody'][index]
      
          def set_from(self, fromacc):
              # 指定消息的发送方，默认为服务器发送
              self.fromacc = fromacc
              self.msgrequest['From_Account'] = fromacc
      
          def set_to(self, toacc):
              # 指定消息的接收方，可以为String(单发),可以为List(批量发送)
              self.toacc = toacc
              self.msgrequest['To_Account'] = toacc
      
          def refresh_random(self):
              self.msgrandom = random.randint(1, 1000000000)
              self.msgrequest['MsgRandom'] = self.msgrandom, # 消息随机数，由随机函数产生
      
          def set_sync(self, sync):
              # 同步选项：1, 把消息同步到From_Account在线终端和漫游上
              #           2, 消息不同步至From_Account
              #           若不填写，默认情况下会将消息同步
              #           仅在单发单聊消息中可调用
              self.sync = sync
              self.msgrequest['SyncOtherMachine'] = sync
      
          def set_timestamp(self):
              # 设置消息时间戳，unix时间, 仅在单发单聊消息中可以调用
              self.timestamp = int(time.time())
              self.msgrequest['MsgTimeStamp'] = self.timestamp
      
          def set_offlinepush(self, pushflag=0, desc='', ext='', sound=''):
              # 仅适用于APNa推送，不适用于安卓厂商推送
              self.msgrequest['OfflinePushInfo'] = {
                  'PushFlag': pushflag,
                  'Desc': desc,
                  'Ext': ext,
                  'Sound': sound
              }
      
      
      class MsgDict(object):
      
          def __init__(self, msgtype='', msgcontent={}):
              self.msgtype = msgtype
              self.msgcontent = msgcontent
      
          @property
          def msg(self):
              return {
                  'MsgType': self.msgtype,
                  'MsgContent': self.msgcontent
              } 
      
          def set_content(self, content):
              self.msgcontent = content
      
      
      class TextMsg(MsgDict):
      
          def __init__(self, text='', msgtype='TIMTextElem'): 
              self.text = text
              content = {'Text': text}
              super(TextMsg, self).__init__(msgtype=msgtype, msgcontent=content)
      
          def set_text(self, text):
              self.text = text
              self.msgcontent['Text'] = text
      
      
      class LocationMsg(MsgDict):
      
          def __init__(self, desc='', latitude=0, longitude=0, msgtype='TIMLocationElem'): 
              self.desc = desc
              self.latitude = latitude
              self.longitude = longitude
              content = {
                  'Desc': desc,  # 地理位置描述信息, String
                  'Latitude': latitude, # 纬度, Number
                  'Longitude': longitude # 经度, Number
              }
              super(LocationMsg, self).__init__(msgtype=msgtype, msgcontent=content)
      
          def set_desc(self, desc):
              self.desc = desc
              self.msgcontent['Desc'] = desc
      
          def set_location(self, latitude, longitude):
              self.latitude = latitude
              self.longitude = longitude
              self.msgcontent['Latitude'] = latitude
              self.msgcontent['Longitude'] = longitude
      
          def set_latitude(self, latitude):
              self.latitude = latitude
              self.msgcontent['Latitude'] = latitude
      
          def set_longitude(self, longitude):
              self.longitude = longitude
              self.msgcontent['Longitude'] = longitude
      
      
      class FaceMsg(MsgDict):
      
          def __init__(self, index=1, data='', msgtype='TIMFaceElem'): 
              self.index = index 
              self.data = data
              content = {
                  'Index': index, # 表情索引，用户自定义, Number
                  'Data': data # 额外数据, String
              }
              super(TextMsg, self).__init__(msgtype=msgtype, msgcontent=content)
      
          def set_index(self, index):
              self.index = index
              self.msgcontent['Index'] = index
      
          def set_data(self, data):
              self.data = data
              self.msgcontent['Data'] = data
      
      
      class CustomMsg(MsgDict):
      
          def __init__(self, data='', desc='', ext='', sound='', msgtype='TIMCustomElem'): 
              self.data = data
              self.desc = desc
              self.ext = ext
              self.sound = sound
              content = {
                  'Data': data, # 自定义消息数据。不作为APNS的payload中字段下发，故从payload中无法获取Data字段, String
                  'Desc': desc, # 自定义消息描述，当接收方为iphone后台在线时，做ios离线Push时文本展示
                  'Ext': ext, # 扩展字段，当接收方为ios系统且应用处在后台时，此字段作为APNS请求包Payloads中的ext键值下发，Ext的协议格式由业务方确定，APNS只做透传
                  'Sound': sound # 自定义APNS推送铃声
              }
              super(CustomMsg, self).__init__(msgtype=msgtype, msgcontent=content)
      
          def set_data(self, data):
              self.data = data
              self.msgcontent['Data'] = data
      
          def set_desc(self, desc):
              self.desc = desc
              self.msgcontent['Desc'] = desc
      
          def set_ext(self, ext):
              self.ext = ext
              self.msgcontent['Ext'] = ext
      
          def set_sound(self, sound):
              self.sound = sound
              self.msgcontent['Sound'] = sound
      
      
      class SoundMsg(MsgDict):
      
          def __init__(self, uuid='', size=0, second=0, msgtype='TIMSoundElem'): 
              self.uuid = uuid 
              self.size = size 
              self.second = second
              content = {
                  'UUID': uuid, # 语音序列号，后台用于索引语音的键值，String
                  'Size': size, # 语音数据大小, Number 
                  'Second': second # 语音时长，单位秒 Number 
              }
              super(SoundMsg, self).__init__(msgtype=msgtype, msgcontent=content)
      
          def set_uuid(self, uuid):
              self.uuid = uuid
              self.msgcontent['UUID'] = uuid
      
          def set_size(self, size):
              self.size = size
              self.msgcontent['Size'] = size
      
          def set_second(self, second):
              self.second = second
              self.msgcontent['Second'] = second
      
      
      class ImageMsg(MsgDict):
      
          def __init__(self, uuid='', imgformat=0, imginfo=[], msgtype='TIMImageElem'): 
              self.uuid = uuid 
              self.imgformat = imgformat 
              self.imginfo = imginfo 
              content = {
                  'UUID': uuid, # 图片序列号，后台用于索引语音的键值，String
                  'ImageFormat': imgformat, # 图片格式， BMP=1, JPG=2, GIF=3, 其他=0, Number 
                  'ImageInfoArray': [t.info for t in imginfo] # 原图，缩略图或者大图下载信息, Array
              }
              super(ImageMsg, self).__init__(msgtype=msgtype, msgcontent=content)
      
          def set_uuid(self, uuid):
              self.uuid = uuid
              self.msgcontent['UUID'] = uuid
      
          def set_format(self, imgformat):
              self.imgformat = imgformat
              self.msgcontent['ImageFormat'] = imgformat
      
          def append_info(self, info):
              # info 为ImageInfo对象实例
              self.imginfo.append(info)
              self.msgcontnet['ImageInfoArray'].append(info.info)
      
          def insert_info(self, index, info):
              self.imginfo.insert(index, info)
              self.msgcontent['ImageInfoArray'].insert(index, info.info)
      
          def del_info(self, index):
              del self.imginfo[index]
              del self.msgcontent['ImageInfoArray'][index]
      
      
      class FileMsg(MsgDict):
      
          def __init__(self, uuid='', size=0, name='', msgtype='TIMFileElem'): 
              self.uuid = uuid 
              self.size = size 
              self.name = name
              content = {
                  'UUID': uuid, # 文件序列号，后台用于索引语音的键值，String
                  'FileSize': size, # 文件数据大小, Number 
                  'FileName': name # 文件名称/路径， String
              }
              super(FileMsg, self).__init__(msgtype=msgtype, msgcontent=content)
      
          def set_uuid(self, uuid):
              self.uuid = uuid
              self.msgcontent['UUID'] = UUID
      
          def set_size(self, size):
              self.size = size
              self.msgcontent['FileSize'] = size
      
          def set_name(self, name):
              self.name = name
              self.msgcontent['FileName'] = name
      
      
      class ImageInfo(object):
      
          def __init__(self, itype=1, size=0, width=0, height=0, url=''):
              #图片类型， 1-原图， 2-大图， 3-缩略图, 0-其他
              self.itype = itype 
              # 图片数据大小,Number
              self.size = size
              # 图片宽度,Number
              self.width = width
              # 图片高度, Number
              self.height = height
              # 图片下载地址,String
              self.url = url
      
          @property
          def info(self):
              return {
                  'Type': self.itype, 
                  'Size': self.size,  
                  'Width': self.width,  
                  'Height': self.height, 
                  'URL': self.url 
              }
      
          def set_type(self, itype):
              self.itype = itype
      
          def set_size(self, size):
              self.size = size
      
          def set_width(self, width):
              self.width = width
      
          def set_height(self, height):
              self.height = height
      
          def set_url(self, url):
              self.url = url
      ```

##### 2 微信开发包

1. 微信开发包，python实现, wechat_sdk开发

   1. `http://wechat-python-sdk.com/`

   2. ```
      from __future__ import unicode_literals
      
      import time
      
      from wechat_sdk.lib.crypto import BasicCrypto
      from wechat_sdk.lib.request import WechatRequest
      from wechat_sdk.exceptions import NeedParamError
      from wechat_sdk.utils import disable_urllib3_warning
      
      
      class WechatConf(object):
          """ WechatConf 配置类
      
          该类将会存储所有和微信开发相关的配置信息, 同时也会维护配置信息的有效性.
          """
      
          def __init__(self, **kwargs):
              """
              :param kwargs: 配置信息字典, 可用字典 key 值及对应解释如下:
                             'token': 微信 Token
      
                             'appid': App ID
                             'appsecret': App Secret
      
                             'encrypt_mode': 加解密模式 ('normal': 明文模式, 'compatible': 兼容模式, 'safe': 安全模式(默认))
                             'encoding_aes_key': EncodingAESKey 值 (传入此值必须保证同时传入 token, appid, 否则抛出异常)
      
                             'access_token_getfunc': access token 获取函数 (用于单机及分布式环境下, 具体格式参见文档)
                             'access_token_setfunc': access token 写入函数 (用于单机及分布式环境下, 具体格式参见文档)
                             'access_token_refreshfunc': access token 刷新函数 (用于单机及分布式环境下, 具体格式参见文档)
                             'access_token': 直接导入的 access token 值, 该值需要在上一次该类实例化之后手动进行缓存并在此处传入, 如果不
                                             传入, 将会在需要时自动重新获取 (传入 access_token_getfunc 和 access_token_setfunc 函数
                                             后将会自动忽略此处的传入值)
                             'access_token_expires_at': 直接导入的 access token 的过期日期, 该值需要在上一次该类实例化之后手动进行缓存
                                                        并在此处传入, 如果不传入, 将会在需要时自动重新获取 (传入 access_token_getfunc
                                                        和 access_token_setfunc 函数后将会自动忽略此处的传入值)
      
                             'jsapi_ticket_getfunc': jsapi ticket 获取函数 (用于单机及分布式环境下, 具体格式参见文档)
                             'jsapi_ticket_setfunc': jsapi ticket 写入函数 (用于单机及分布式环境下, 具体格式参见文档)
                             'jsapi_ticket_refreshfunc': jsapi ticket 刷新函数 (用于单机及分布式环境下, 具体格式参见文档)
                             'jsapi_ticket': 直接导入的 jsapi ticket 值, 该值需要在上一次该类实例化之后手动进行缓存并在此处传入, 如果不
                                             传入, 将会在需要时自动重新获取 (传入 jsapi_ticket_getfunc 和 jsapi_ticket_setfunc 函数
                                             后将会自动忽略此处的传入值)
                             'jsapi_ticket_expires_at': 直接导入的 jsapi ticket 的过期日期, 该值需要在上一次该类实例化之后手动进行缓存
                                                        并在此处传入, 如果不传入, 将会在需要时自动重新获取 (传入 jsapi_ticket_getfunc
                                                        和 jsapi_ticket_setfunc 函数后将会自动忽略此处的传入值)
      
                             'partnerid': 财付通商户身份标识, 支付权限专用
                             'partnerkey': 财付通商户权限密钥 Key, 支付权限专用
                             'paysignkey': 商户签名密钥 Key, 支付权限专用
      
                             'checkssl': 是否检查 SSL, 默认不检查 (False), 可避免 urllib3 的 InsecurePlatformWarning 警告
              :return:
              """
      
              self.__request = WechatRequest()
      
              if kwargs.get('checkssl') is not True:
                  disable_urllib3_warning()  # 可解决 InsecurePlatformWarning 警告
      
              self.__token = kwargs.get('token')
      
              self.__appid = kwargs.get('appid')
              self.__appsecret = kwargs.get('appsecret')
      
              self.__encrypt_mode = kwargs.get('encrypt_mode', 'safe')
              self.__encoding_aes_key = kwargs.get('encoding_aes_key')
              self.__crypto = None
              self._update_crypto()
      
              self.__access_token_getfunc = kwargs.get('access_token_getfunc')
              self.__access_token_setfunc = kwargs.get('access_token_setfunc')
              self.__access_token_refreshfunc = kwargs.get('access_token_refreshfunc')
              self.__access_token = kwargs.get('access_token')
              self.__access_token_expires_at = kwargs.get('access_token_expires_at')
      
              self.__jsapi_ticket_getfunc = kwargs.get('jsapi_ticket_getfunc')
              self.__jsapi_ticket_setfunc = kwargs.get('jsapi_ticket_setfunc')
              self.__jsapi_ticket_refreshfunc = kwargs.get('jsapi_ticket_refreshfunc')
              self.__jsapi_ticket = kwargs.get('jsapi_ticket')
              self.__jsapi_ticket_expires_at = kwargs.get('jsapi_ticket_expires_at')
      
              self.__partnerid = kwargs.get('partnerid')
              self.__partnerkey = kwargs.get('partnerkey')
              self.__paysignkey = kwargs.get('paysignkey')
      
          @property
          def token(self):
              """ 获取当前 Token """
              self._check_token()
              return self.__token
      
          @token.setter
          def token(self, token):
              """ 设置当前 Token """
              self.__token = token
              self._update_crypto()  # 改动 Token 需要重新更新 Crypto
      
          @property
          def appid(self):
              """ 获取当前 App ID """
              return self.__appid
      
          @property
          def appsecret(self):
              """ 获取当前 App Secret """
              return self.__appsecret
      
          def set_appid_appsecret(self, appid, appsecret):
              """ 设置当前 App ID 及 App Secret"""
              self.__appid = appid
              self.__appsecret = appsecret
              self._update_crypto()  # 改动 App ID 后需要重新更新 Crypto
      
          @property
          def encoding_aes_key(self):
              """ 获取当前 EncodingAESKey """
              return self.__encoding_aes_key
      
          @encoding_aes_key.setter
          def encoding_aes_key(self, encoding_aes_key):
              """ 设置当前 EncodingAESKey """
              self.__encoding_aes_key = encoding_aes_key
              self._update_crypto()  # 改动 EncodingAESKey 需要重新更新 Crypto
      
          @property
          def encrypt_mode(self):
              return self.__encrypt_mode
      
          @encrypt_mode.setter
          def encrypt_mode(self, encrypt_mode):
              """ 设置当前加密模式 """
              self.__encrypt_mode = encrypt_mode
              self._update_crypto()
      
          @property
          def crypto(self):
              """ 获取当前 Crypto 实例 """
              return self.__crypto
      
          @property
          def access_token(self):
              """ 获取当前 access token 值, 本方法会自行维护 access token 有效性 """
              self._check_appid_appsecret()
      
              if callable(self.__access_token_getfunc):
                  self.__access_token, self.__access_token_expires_at = self.__access_token_getfunc()
      
              if self.__access_token:
                  now = time.time()
                  if self.__access_token_expires_at - now > 60:
                      return self.__access_token
      
              self.grant_access_token()  # 从腾讯服务器获取 access token 并更新
              return self.__access_token
      
          @property
          def jsapi_ticket(self):
              """ 获取当前 jsapi ticket 值, 本方法会自行维护 jsapi ticket 有效性 """
              self._check_appid_appsecret()
      
              if callable(self.__jsapi_ticket_getfunc):
                  self.__jsapi_ticket, self.__jsapi_ticket_expires_at = self.__jsapi_ticket_getfunc()
      
              if self.__jsapi_ticket:
                  now = time.time()
                  if self.__jsapi_ticket_expires_at - now > 60:
                      return self.__jsapi_ticket
      
              self.grant_jsapi_ticket()  # 从腾讯服务器获取 jsapi ticket 并更新
              return self.__jsapi_ticket
      
          @property
          def partnerid(self):
              """ 获取当前财付通商户身份标识 """
              return self.__partnerid
      
          @property
          def partnerkey(self):
              """ 获取当前财付通商户权限密钥 Key """
              return self.__partnerkey
      
          @property
          def paysignkey(self):
              """ 获取商户签名密钥 Key """
              return self.__paysignkey
      
          def grant_access_token(self):
              """
              获取 access token 并更新当前配置
              :return: 返回的 JSON 数据包 (传入 access_token_refreshfunc 参数后返回 None)
              """
              self._check_appid_appsecret()
      
              if callable(self.__access_token_refreshfunc):
                  self.__access_token, self.__access_token_expires_at = self.__access_token_refreshfunc()
                  return
      
              response_json = self.__request.get(
                  url="https://api.weixin.qq.com/cgi-bin/token",
                  params={
                      "grant_type": "client_credential",
                      "appid": self.__appid,
                      "secret": self.__appsecret,
                  },
                  access_token=self.__access_token
              )
              self.__access_token = response_json['access_token']
              self.__access_token_expires_at = int(time.time()) + response_json['expires_in']
      
              if callable(self.__access_token_setfunc):
                  self.__access_token_setfunc(self.__access_token, self.__access_token_expires_at)
      
              return response_json
      
          def grant_jsapi_ticket(self):
              """
              获取 jsapi ticket 并更新当前配置
              :return: 返回的 JSON 数据包 (传入 jsapi_ticket_refreshfunc 参数后返回 None)
              """
              self._check_appid_appsecret()
      
              if callable(self.__jsapi_ticket_refreshfunc):
                  self.__jsapi_ticket, self.__jsapi_ticket_expires_at = self.__jsapi_ticket_refreshfunc()
                  return
      
              response_json = self.__request.get(
                  url="https://api.weixin.qq.com/cgi-bin/ticket/getticket",
                  params={
                      "type": "jsapi",
                  },
                  access_token=self.access_token,
              )
              self.__jsapi_ticket = response_json['ticket']
              self.__jsapi_ticket_expires_at = int(time.time()) + response_json['expires_in']
      
              if callable(self.__jsapi_ticket_setfunc):
                  self.__jsapi_ticket_setfunc(self.__jsapi_ticket, self.__jsapi_ticket_expires_at)
      
              return response_json
      
          def get_access_token(self):
              """
              获取 Access Token 及 Access Token 过期日期, 仅供缓存使用, 如果希望得到原生的 Access Token 请求数据请使用 :func:`grant_token`
              **仅为兼容 v0.6.0 以前版本使用, 自行维护 access_token 请使用 access_token_setfunc 和 access_token_getfunc 进行操作**
              :return: dict 对象, key 包括 `access_token` 及 `access_token_expires_at`
              """
              self._check_appid_appsecret()
      
              return {
                  'access_token': self.access_token,
                  'access_token_expires_at': self.__access_token_expires_at,
              }
      
          def get_jsapi_ticket(self):
              """
              获取 Jsapi Ticket 及 Jsapi Ticket 过期日期, 仅供缓存使用, 如果希望得到原生的 Jsapi Ticket 请求数据请使用 :func:`grant_jsapi_ticket`
              **仅为兼容 v0.6.0 以前版本使用, 自行维护 jsapi_ticket 请使用 jsapi_ticket_setfunc 和 jsapi_ticket_getfunc 进行操作**
              :return: dict 对象, key 包括 `jsapi_ticket` 及 `jsapi_ticket_expires_at`
              """
              self._check_appid_appsecret()
      
              return {
                  'jsapi_ticket': self.jsapi_ticket,
                  'jsapi_ticket_expires_at': self.__jsapi_ticket_expires_at,
              }
      
          def _check_token(self):
              """
              检查 Token 是否存在
              :raises NeedParamError: Token 参数没有在初始化的时候提供
              """
              if not self.__token:
                  raise NeedParamError('Please provide Token parameter in the construction of class.')
      
          def _check_appid_appsecret(self):
              """
              检查 AppID 和 AppSecret 是否存在
              :raises NeedParamError: AppID 或 AppSecret 参数没有在初始化的时候完整提供
              """
              if not self.__appid or not self.__appsecret:
                  raise NeedParamError('Please provide app_id and app_secret parameters in the construction of class.')
      
          def _update_crypto(self):
              """
              根据当前配置内容更新 Crypto 类
              """
              if self.__encrypt_mode in ['compatible', 'safe'] and self.__encoding_aes_key is not None:
                  if self.__token is None or self.__appid is None:
                      raise NeedParamError('Please provide token and appid parameters in the construction of class.')
                  self.__crypto = BasicCrypto(self.__token, self.__encoding_aes_key, self.__appid)
              else:
                  self.__crypto = None
      ```

   

#### 13.6 with与“上下文管理器”

1. `with` 关键字

   1. 系统资源的关闭

      1. `文件`、`数据库`、`socket` 等系统资源 
      2. 应用程序打开资源, 执行完业务逻辑之后，要关闭（断开）资源

   2. 不关闭会出现问题

      1. 文件,   "Too many open files" 的错误, 文件数量是有限的
      2. 数据库,  "Can not connect to MySQL server Too many connections"

   3. 关闭文件

      1. 普通版 方法

         1. `close()`
      
         2. ```
            def m1():
                f = open("output.txt", "w")
                f.write("python之禅")
             f.close()
            ```

         3. 潜在的问题
      
         1. write 的过程 出现了异常 后续代码无法继续执行
            2. close 方法无法被正常调用,资源 一直被该程序占用者释放

      2. 进阶版
      
         1. `try except finally` 与 `close()`配合使用
      
         2. ```
            def m2():
                f = open("output.txt", "w")
                try:
                    f.write("python之禅")
                except IOError:
                 print("oops error")
                finally:
                 f.close()
            ```

         3. 无论如何，finally 块的代码最终都会 执行
      
      3. 高级版
      
         1. `with`  开启 上下文管理器

         2. ```
            def m3():
             with open("output.txt", "r") as f:
                    f.write("Python之禅")
         ```
      
            1. 更加简洁、优雅
            2.  with 的作用和` try/finally` 一样
      
         3. 过程:
      
            1. open 方法的 返回值 赋值给变量 f
            2. 当离开 with 代码块的时，系统 自动调用 f.close() 方法

2. 上下文

   1. 上下文(context)-- 通俗理解 `环境`, `语境`-- 语言环境.  
   2. 上下文(文章的上下文)
   3. 程序里 一般 只有上文而已，只是叫的好听叫上下文
   4. 进程中断在操作系统中是有上有下的，不过 这个高深的问题 不要深究

3. 上下文管理器

   1. 实现了` __enter__() `和` __exit__() `的对象: `上下文管理器`

   2. 上下文管理器对象 用 `with` 关键字

   3. 文件（file）对象 实现了上下文管理器

      1. File类 实现 `__enter__()` 和 `__exit__()` 

      2. ```
         class File():
         
             def __init__(self, filename, mode):
                 self.filename = filename
                 self.mode = mode
         
             def __enter__(self):
                 print("entering")
                 self.f = open(self.filename, self.mode)
                 return self.f
         
             def __exit__(self, *args):
                 print("will exit")
                 self.f.close()
         ```

         1. 文件的数据结构 :  1. 文件名(文件路径),  2. 打开模式
         2. `__enter__()` 方法返回资源对象, 即 打开的文件对象，`__exit__()` 方法处理清除工作
   3. 类的方法内的属性, 各个方法都可以调用
         
   3. `File` 类实现了上下文管理器,  用 with 语句
      
         ```
         with File('out.txt', 'w') as f:
             print("writing")
             f.write('hello, python')
      ```
      
         1. 无需显式地调用 close 方法
            1. 系统自动调用
            2. 遇到异常  也会自动调用

4. 上下文管理器 另一实现方式

   1. Python  提供了`@contextmanager`装饰器

      1. 简化了上下文管理器的实现方式

      2. `yield` 将函数分成两部分

         1. yield 之前的语句在 `__enter__ `中执行
      
         2. yield 之后的语句在 `__exit__` 中执行

         3. 紧跟在`yield` 后面的值是函数的返回值

         4. 小结
      
            1. ```
               语句组  #yield前, 相当于__enter__()
               yield f # yield返回值
               语句组2 #yield后, 相当于__exit__()
               ```
      
      3. 示例

         ```
      from contextlib import contextmanager
         
         @contextmanager
         def my_open(path, mode):
             f = open(path, mode)
             yield f
             f.close()
             
      #调用
         with my_open('out.txt', 'w') as f:
          f.write("hello , the simplest context manager")
         ```
         
         
         

5. 总结

   1. with 语法 简化资源操作,  是`try/finally` 的替代，实现原理建立在上下文管理器之上。
   2.  `@contextmanager` 装饰器,  简化上下管理器类的实现方式

------------

视频

#### 04 - 方法解析顺序表MRO

##### 01-（重点）多继承中的MRO顺序

1. 一个类多个父类

2.  覆盖(重写),  重载

   1. 覆盖(重写): override  ---python有

      1. 发生于子类中与父类中: 方法名称相同, 参数个数类型可以不同/相同
      2. 子类中与父类中: 函数体可以不同/相同
      3. python为什么同名就覆盖了?
         1. python中类用字典存储 类的属性方法, 字典的键是即属性名或方法名, 因此会被覆盖
         2. 后面的同名方法覆盖前面的同名方法或 覆盖父类的同名方法

   2. 重载: 方法名相同, 但 参数类型或个数等不同-- python没有. java支持重载 overload

      1. 函数完整签名不一样了

      2. 不一定是 父类子类关系, 

      3. 方法的参数必须不同: 即参数的类型或个数，以 区分不同方法体

      4. 方法的返回类型、修饰符可以相同，也可以不同

      5. 示例

         1. ```
            def a(x):
                return x
             
            def a(x,y):
                return x+y
             
            print(a(1,2))
            print(a(1))  #报错, python是没有重载, 第二个同名的函数会把第一个覆盖掉
            ```

   3. 总结:

      1. python 没有重载的，最后的同名的函数覆盖前面的同名函数. 为什么?
         1. 第一: python 没有类型
         2. 第二: python 有可变参数

      2. 重写：子类中方法覆盖继承的父类的同名方法 
      3. 重载：多个方法名一样
         1. 但他们的参数类型，参数个数不一样（java）。
         2. python不支持重载. 为什么?
            1. 因为python 支持可变参数，python没有参数类型

3. 父类初始化

   1. 方法1: 子类的`__init__()`方法中添加:
      1. `Parent.__init__(self, name)`   #父类Parent初始化
      2. 特点: 顶层父类初始化多次
   2. 方法2: 子类的`__init__()`方法中添加:
      1. `super().__init__(name)`, 
      2. 或`super(A, self).__init__(name)`   #子类A的父类的初始化. 可指定当前类名A
      3. 两种方式 功能相同

4. `super()` 意义: 
   
   1.  保证类初始化父类只初始化一次的算法
   2. `Grandson.__mro__`   # 返回Grandson类及父类的列表a
   3. `__init__`所在类名在`__mro__`的列表, 第一个就是要`__init__`的类

##### 02-（重点）※args、※※kwargs的另外用处拆包

1. ```
   def test1(a, b, *args, **kwargs):  
   	pass
   
   def test2(a, b, *args, **kwargs):
   	test1(a, b, *args, **kwargs)  # 带*,**拆包
   ```

   1. 形参
      1.  *args 元组
      2.  **kwargs 字典 , 关键字参数

   2. 实参

      1.  *arg  拆包, 拆元组
      2.  **kwargs 拆包, 拆字典

      

##### 03-单继承中MRO、继承的面试题

1. ```
   class Parent(object):
   	x = 1   # 类属性, 可被子类继承, 可被子类的实例调用
   	def __init__(self):
   		self.y = 2  # 实例属性, 是复制
   ```

   1. Parent.x 值 1

2. 继承是指向**引用**, 不是复制

--------

#### 05-类对象和实例对象访问属性的区别和property属性

##### 01-类对象、实例对象、类方法、实例方法、类属性、实例属性、静态方法

1. ```
   class Foo(object):
   	class_att
   	
   	def __init__(self):
   		self.demo_att = 10
   		
   	def demo_fun(self):
       	pass
       	
       @classmethod  #类方法
       def class_fun(cls):
       	pass
       	
       @staticmethod  #静态方法
       def static_fun():
       	pass
   ```

   1.  class_att  # 类属性, 共有的
   2. demo_att  
      1. 实例对象属性, 每个实例各自有
      2. `__class__` 每个实例对象默认属性, 指向自己的类
      3. `obj.__class__.class_att` 可以修改类属性的值
   3. 实例方法
      1. 参数self, 传递实例引用
      2. 内部可调用所有类型的方法
   4.  类方法
      1. 参数 cls, 传递类引用
      2. 内部只能调用 类属性, 类方法, 静态方法. 已经先于该方法存在的
   5. 静态方法
      1. 寄存在类中, 不属于类和实例
   2. 逻辑关系: 寄存
   
2.  d = Foo()   #生成实例,  自动调用`__new__, __init__`

   1. 先调用`__new__()` , 生成内存空间
   2. 再调用`__init__()`  初始化内存空间的值.  python解释器把刚生成的内存空间的`引用`赋给 self

3. `@`  装饰器

##### 02-（重点）property属性

1.  ```
   class Goods():
   	@property
   	def size(self)
   		return 10
   		
   g = Goods()
   g.size
   ```

   1. @property  装饰器
   2.  调用方法  变为 调用属性.  
   3. 只允许有self 一个参数, 必须 return 值
   4.  调用时, `实例名.方法名`, 无() , 无参数
   5.  当对象属性调用

2.  `@property` 修饰的函数内部进行逻辑计算, 

##### 03-property属性的应用

1.  简单

##### 04-创建property属性的方式-装饰器

1. 经典类, 父类深度优先.  新式类, 父类 广度优先

2. 装饰器方式创建 property属性

   1. ```
      # 装饰器创建 @property
      class Goods():
      	@property   #经典类只有. 取值
      	def price(self):
      		print("取price")
      		return 10
      		
      	@price.setter  #设定值
      	def price(self, value):
      		print("设置price:%d" % value)
      		
      	@price.deleter  #删除值
      	def price(self):
      		print("删除price")
      		
      g = Goods()
      g.price  #取值
      g.price = 10  # 设置值
      del g.price  # 删除price值
      ```

   2. 获取, 设置setter , 删除 deleter

##### 05-创建property属性的方式-类属性

1. 类属性方式创建 property属性

2. ```
   class Goods():
   	def get_price(self):
   		print("取price")
   		return 10
   		
   	def set_price(self, value):
   		print("设置price%d" % value)
   		
   	def del_price(self):
   		print("删除price")
   		
   	price = property(get_price, set_price, del_price, "description...")
   	
   	
   g = Goods()
   g.price      # 自动调用第一个参数的方法
   g.price = 100  # 自动调用第二个参数的方法
   g.__doc__    # 自动调用第四个参数的值
   del g.price  # 自动调用第三个参数的方法
   ```

3. propery()函数: 
   1. 参数: 四个参数有顺序,  1,2,3是方法名, 4是字符串 `对象.属性.__doc__`
      1. 第一, 取值.  第二, 设定值.  第三, 删除.  第四, 描述



##### 06-property属性的应用2

1. 类属性方式设置 getter, setter

   ```
   class Goods():
   	def __init__(self):
   		self.__money = 0
   		
   	def get_price(self):
   		print("取price")
   		return self.__money
   		
   	def set_price(self, value):
   		self.__money = value
   		print("设置price%d" % self.__money)
   	
   	price = property(get_price, set_price)
   	
   	
   g = Goods()
   g.price      # 取值
   g.price = 100 # 设定值
   ```

   1. __money, 私有属性, 类内部的属性,  实例看不见不能用的,
   2.  不允许`实例名.私有属性` 即 `g.__money`  调用, 调用就报错

2. 装饰器方式 设置getter, setter

   ```
   class Goods():
   	@property
   	def price(self):
   		print("取price")
   		return 10
   	@price.setter	
   	def set_price(self, value):
   		print("设置price%d" % value)
   
   
   	
   g = Goods()
   g.price      # 取值
   g.price = 100 # 设定值
   
   ```

   ----------

   #### 06-私有属性和名字重整、魔法属性和方法、上下文管理器

   ##### 01-修改、查看私有属性、名字重整

   1. ```
      class Goods():
      	def __init__(self, price):
      		self.__money = price
      ```

      ```
      g = Goods(10)
      g.__dict__   #输出 {'_Goods__name', 10}
      ```

      1. _Goods__name   # 名字重整
      2.  __money 类私有,外部看不见, 实例内没有. 经名字重整, 以字典形式存于实例的`__dict__`属性中, 

   ##### 02-魔法属性、方法

   1.  魔法属性

      1. ```
         __doc__
         class Foo:
             """ 描述类信息，这是用于看片的神奇 """
         输出""""""中的文本    
         ```

      2. ```
         __module__
         f.__module__  # f对象所在的模块文件
         ```

      3. ```
         __class__
         f.__class__  # f实例的类
         ```

      4. ```
         __dict__   # 类的属性,方法  或实例的属性.  以字典形式存储
         
         class Foo():
         	count = 'China'
         	
           	def __init__(self, name, count):
             	self.name = name
             	
             def func(self, *args, **kwargs):
                 print('func')
         
         ```

         1. `f.__dict__ ` # 实例的所有属性

         2. `Foo.__dict__`  # 类的属性, 实例方法, 类方法, 静态方法

            

   2. 魔法方法

      1. ```
         __init__()  # 实例的内存空间初始化
         ```

      2. ```
         __del__()  # 该方法默认已经有了, 不用定义
         del g  # 删除g实例对象
         ```

      3. ```
         class Goods():
         	def __call__(self):
         		print("__call__")
         
         __call__()  # 实例名加括号，自动触发执行
         g()  # 自动调用, 自动触发
         ```

      4. ```
         __str__()   #打印 对象 时，默认输出该方法的返回值。
         
         class Goods():
         	def __str__(self):
         		return "AA"
         		
         g = Goods()
         print(g)  # 输出AA
         ```

      5. ```
         __getitem__、__setitem__、__delitem__
         索引操作，如字典。以上分别表示获取、设置、删除数据
         --------
         # -*- coding:utf-8 -*-
         
         class Foo(object):
         
             def __getitem__(self, key):
                 print('__getitem__', key)
         
             def __setitem__(self, key, value):
                 print('__setitem__', key, value)
         
             def __delitem__(self, key):
                 print('__delitem__', key)
         
         
         obj = Foo()
         
         result = obj['k1']      # 自动触发执行 __getitem__
         obj['k2'] = 'laowang'   # 自动触发执行 __setitem__
         del obj['k1']           # 自动触发执行 __delitem__
         ```

         1.  根据索引操作自动触发, 
            1.  获取, 设置, 删除
            2. 不用调用方法
         2.  用于索引操作

      6. ```
         __getslice__、__setslice__、__delslice__
         分片操作
         -----
         class Foo(object):
         	
             def __getslice__(self, i, j):
                 print('__getslice__', i, j)
         
             def __setslice__(self, i, j, sequence):
                 print('__setslice__', i, j)
         
             def __delslice__(self, i, j):
                 print('__delslice__', i, j)
         
         obj = Foo()
         
         obj[-1:1]                   # 自动触发执行 __getslice__
         obj[0:1] = [11,22,33,44]    # 自动触发执行 __setslice__
         del obj[0:2]                # 自动触发执行 __delslice__
         ```

         1.  分片时自动触发, 不用调用方法
         2.  切片操作

   3.  特殊的情况时, python会自动调用特殊的方法, 特殊的属性, 就是魔法

   4. f[-1: -3],  默认步长+1, 输出空[]

   ##### 03-面向对象设计

   1. 格式
      1.  类间 空2行
      2.  方法间 空 1行
      3.  """ 类的说明 """,  # 必须有
   2. 继承, 多态, 封装 : 数据隐藏

   ##### 04-（重点）with、上下文管理器
   1.  with

      1. 简洁优美
      2.  try...except...finally 与 close 的替代方法，实现原理建立在上下文管理器之上

   2. `__enter__, __exit__` 实现上下文管理器

      ```
      class File():
      
          def __init__(self, filename, mode):
              self.filename = filename
              self.mode = mode
      
          def __enter__(self):
              print("entering")
              self.f = open(self.filename, self.mode)  #内部具体实现代码
              return self.f
      
          def __exit__(self, *args):
              print("will exit")
              self.f.close()
      ```

      1. `__enter__() `   # return 文件资源对象
      2. `__exit__()`   # 异常, 出错, 结束后的清理工作, 文件关闭

      ```
      with File('out.txt', 'w') as f:
          print("writing")
          f.write('hello, python')
      ```

   3. 装饰器实现 上下文管理器

      ```
      from contextlib import contextmanager
      
      @contextmanager
      def my_open(path, mode):
          f = open(path, mode)
          yield f
          f.close()
      ```

      ```
      with my_open('out.txt', 'w') as f:
          f.write("hello , the simplest context manager")
      ```

      

​    



