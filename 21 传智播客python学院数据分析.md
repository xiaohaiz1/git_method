

#### 小结

1. pd中 对象的方法不一定有对应类的方法,  类的方法不一定有对象的方法. 类和对象共有的方法. 



## 传智播客Python学院数据分析

文档版本 -  黑马 数据分析v1.0课件

1. 技能
   1. 数据分析的流程，包括数据采集、处理、可视化等
   2. Python语言作 数据分析工具
   3. 非结构化数据的处理与分析
   4. 数据分析 的 建模知识

### 1 工作环境准备及数据分析建模理论基础

1. python 环境	

   1. `Anaconda（水蟒）`： 科学计算软件发行版
   2. 安装, 卸载, 升级
      1. 安装包：`pip install xxx`,`conda install xxx`
      2. 卸载包：`pip uninstall xxx`,`conda uninstall xxx`
      3. 升级包：`pip install upgrade xxx`,`conda update xxx`

2. IDE

   1. Jupyter Notebook, 命令：jupyter notebook

      2. 特点

         1. Anaconda自带，无需单独安装
   2. 实时查看运行过程
         3. 基本的web编辑器（本地）
         4. `.ipynb` 文件分享
         5. 可交互式
         6. 记录 历史运行结果
         
   2. ipython, 命令 `ipython`
   
      2. 特点
      1. Anaconda自带，无需单独安装
         2. Python的交互式命令行 Shell
         3. 可交互式
         4. 记录 历史运行结果
         5. 及时验证想法
      
   3. spyder, 命令 `spyder`
   
   2. 特点
         1. Anaconda自带，无需单独安装
      2. 免费，熟悉Matlab的用户
         3. 功能强大， 简单的图形界面开发环境
      
   4. Pycharm
   
      1. JetBrains的精品，全平台支持
   2. 安装：[https://www.jetbrains.com/pycharm/download](https://www.jetbrains.com/pycharm/download/)

#### 1.1 Python 3.x新特性和编码回顾

1. Python3.x 常用的新特性
   1. print() 输出函数
   2. input() 输入函数
   3. 文本和二进制数 做了更为清晰的区分
      1. 文本 :unicode表示，为str类型
      2. 二进制数 : bytes (字节包)表示， bytes类型
         1. 二进制数据 及被编码的 文本字符串 有前缀`b`
      3.  bytes 与 str 转换
         1. `encode()`  #str 成 bytes
         2. `decode()`  #bytes 成 str
   4. 格式化输出工具： format() 
   5. dict类型
      1. keys(), values(), items()
2. 字符串编码格式回顾：
   1. `ASCII`：最早. 英文编码
   2. `GB2312`： ASCII的中文扩展编码
   3. `GBK/GB18030`：GB2312的扩展编码，增加20000个汉字和符号
   4. `Unicode`：全球字符编码. 每个字符 3~4个字节, 浪费空间
   5. `UTF-8`：变长编码，互联网最广泛的 `Unicode` 实现.
      1. 语种决定字符长度，如 汉字3个字节， 字母1个字节
      2. Linux 默认编码
   6. 小结:  时间顺序: ASCII --> GBK2312 --> GBK --> Unicode --> UTF-8

#### 1.2 DIKW模型与数据工程

1. DIKW 体系: 数据、信息、知识、智慧的体系
   1. Data : 原始材料
   2. Information : 加工后有逻辑的数据
   3. Knowledge : 提炼信息间的联系, 行动的能力, 完成当下任务
      1. 对某个主题的确定认识，认识有潜在的能力为特定目的使用
   4. Wisdom : 关心未来, 预测未来的能力
2. 数据工程领域中的DIKW体系
   1. Data (数据)  : 原始数据
   2. Information (信息) : 按 规则，对原始数据 整合提取的更高层数据（具体数据）
   3. Knowledge (知识) :  信息的 实用化
   4. Wisdom (智慧) :  思考分析 知识, 得 结论
3. 数据工程 领域职业划分
   1. Data Engineer（数据工程师）
      1. 开发工具 完成数据处理自动化
      2.  属于 基础设施/工具（Infrastructure/Tools）层
      3. 角色 频率不多 
         1. 现成的MySQL, Oracle等数据库技术
         2. 很多大公司只要DBA(数据库管理员)就足够了
      4. Hadoop, MongoDB 等 NoSQL 技术的开源，大数据场景没有太多 数据工程师 的事
   2. Data Scientist（数据科学家）
      1. 与数学结合的中间角色, 用数学方法处理原始数据找出肉眼看不到的更高层数据
      2. 运用 统计机器学习（Statistical Machine Learning）或者 深度学习（Deep Learning）
      3. `数据科学家`  把 D 转为 I 或 K 的主力军
   3. Data Analyst（数据分析师）
      1. 对业务 深刻了解
      2. 针对性地 数据分析，发现成果展示，成果变为行动. 即从数据 得出 `Wisdom`
4. 什么是数据分析: 收集大量数据, 统计分析，提取信息, 形成结论的过程
5. 数据分析的过程: 数据的 收集, 处理, 分析, 展现的过程
   1. 收集：本地数据/网络数据的采集与操作.
   2. 处理：数据的`规整`，整合存储。
   3. 分析：数据的科学`计算`. 使用相关数据工具进行分析。
   4. 展现：数据`可视化`，使用相关工具对分析出的数据进行展示
6. 数据分析的工具
   1. `SAS`：SAS（STATISTICAL ANALYSIS SYSTEM，简称SAS）公司开发的统计分析软件
      2. 功能强大的数据库整合平台
      3. 价格 **贵**，银行或者大企业才买的起
      4. 离线的分析或者模型用。
   2. `SPSS`：SPSS（Statistical Product and Service Solutions，统计产品与服务解决方案）是IBM公司推出的
      2. 一系列用于 `统计学` 分析运算、数据挖掘、预测分析和决策支持任务的产品
      3. 40余年的成长历史，价格 **贵**。
   3. `R/MATLAB`： MATLAB（MathWorks公司出品的商业数学软件）是收费的。
      1. 适合 学术性质的数据分析
      2. 实际应用 由 Python或Scala 实现
   4. `Scala`： 
      1. 函数式编程语言，熟练使用后开发效率较高
      2. 配合Spark适合大规模的数据分析和处理
      3. Scala的运行环境是 **JVM**。
   5. `Python`：
      1. 在数据工程领域, 机器学习领域 有成熟的框架和算法库
         1. 可以只用 Python 构建以数据为中心的应用程序
      2. 数据工程领域和机器学习领域，Python非常流行。

#### 1.3 数据分析建模理论基础

1. 大数据分析场景和模型应用
   1. 数据分析建模
      1. 先明确 业务需求
      2. 后选择 `描述型分析` 或 `预测型分析`。
   2. 描述型数据分析
      1. 关联规则、序列规则 、 聚类 等模型。
   3. 预测型数据分析
      1. 量化未来某个事件的发生概率
      2. 两大预测分析模型:  分类预测 和 回归预测 
2. 算法模型
   1. 分类与回归
      1. 分类: 训练已有的训练样本 得到模型, 再用这个模型将输入映射为相应的输出，判断输出, 实现分类的目的
      2. 回归: 基于观测数据, 建立变量间依赖关系。用于预报、控制等问题
      3. 应用： 信用卡申请人风险评估
      4. 原理
         1. 分类： 数据 映射到 已有的类
            1. 基于 特征值 定义类别，特征映射到类别。
            2. 没有逼近概念，正确结果只有一个。
            3. 机器学习方法中，分类属于 `监督学习`
         2. 回归：用 历史数据 预测未来趋势
            1. 先假设函数 匹配目标数据， 后分析匹配误差，选取最小误差 函数
            2. 回归是对真实值的 逼近预测。
      5. 区别 : 分类模型 用 离散值, 回归模型 用 连续值
   2. 聚类
      1. 聚类：相似的聚一起，不相似的不同的类别
         1. 机器学习方法里，聚类属于无监督学习
      2. 聚类分析, 又称 `群分析`，是分类的 统计分析方法，也是数据挖掘的 算法
      3. 应用：
         1. 根据症状归纳 特定疾病
         2. 发现信用卡高级用户
         3. 根据上网行为对客户分群从而进行精确营销
      4. 原理
         1. 没有给定 类别，根据信息相似度进行 聚类.  输入是一组 未被标记的数
         3. 原则:  最大的组内相似性和最小的组间相似性
         4. 聚类 没有训练样本，直接对数据 建模
         5. 机器学习算法，聚类属于无监督学习
   3. 时序模型: （time series）时间序列数据 是结构化数据
      2. 应用：
         1. 下个季度的商品销量或库存量是多少
         2. 明天用电量是多少
         3. 今天的北京地铁13号线的人流情况
      3. 原理
         1. 基于时间 规律或趋势，并对其建模
         2. 与回归一样，用已知的数据 预测 未来的值
            1. 数据的区别是 变量所处时间的不同
            2. 数据在 时间维度上的 关联性
3. 数据分析应用场景
   1. 市场营销
      1. 自动推荐系统(协同过滤推荐，基于内容推荐，基于人口统计推荐，基于知识推荐，组合推荐，关联规则)
      2. 营销响应分析建模(逻辑回归，决策树)
      3. 净提升度分析建模(关联规则)
      4. 客户保有分析建模(卡普兰梅尔分析，神经网络)
      5. 购物蓝分析(关联分析Apriori)
      6. 客户细分(聚类)
      7. 流失预测(逻辑回归)
   2. 风险管理
      1. 客户信用风险评分(SVM，决策树，神经网络)
      2. 市场风险评分建模(逻辑回归和决策树)
      3. 运营风险评分建模(SVM)
      4. 欺诈检测(决策树，聚类，社交网络)

### 2 科学计算工具NumPy

1. numpy (numerical python)
   1. Python的科学计算的基础库，重在数值计算
      1. 多维数组（矩阵）处理
      2. 存储和处理大型矩阵
      3. C语言开发
   2. 特点: 科学计算和数据分析的基础包, 一般的数据处理 `numpy` 够用
      2. ndarray，多维数组（矩阵）
         1. 矢量运算
         2. 矩阵运算
         3. 线性代数、随机数生成
   3. `import numpy as np`
2. scipy
   1. 基于Numpy, 科学计算的工具集, 科学 工程设计的Python工具包。
      1. 统计、优化、整合、线性代数模块、傅里叶变换、信号和图像处理、常微分方程求解、稀疏矩阵等
   2. 特点: 数学系 或 工程 用的多一些, 和数据处理 关系不大， 知道即可, 不讲
   3. `import scipy as sp`

#### 2.1 ndarray的创建与数据类型

1. ndarray 多维数组(N Dimension Array)

   1. ndarray 数组 即 numpy数组
      1. 多维的数组对象（矩阵），称 `ndarray`
      2. 矢量运算, 广播能力
      2. ndarray的元素: 下标从0开始,  同类型
   3. ndarray对象的属性
      1. `ndim`：维度个数,  arr.ndim
      2. `shape`：维度大小, arr.shape
      3. `dtype`：元素的类型, arr.dtype

2. numpy.random:  `随机数` 创建, <class 'numpy.ndarray'>

   1. 随机抽样 (numpy.random) 生成随机数据

   2. 示例

      1. ```python
         #导入numpy，别名np
         import numpy as np
         ```

      2. ```python
         #指定维度大小（3行4列）的 浮点型，rand固定区间0.0 ~ 1.0
         arr = np.random.rand(3, 4)
         ```

      3. ```python
         #维度大小（3行4列）的 整型，randint()区间（-1, 5）
         arr = np.random.randint(-1, 5, size = (3, 4)) # 'size='可省略
         ```

      4. ```
         #维度大小（3行4列）的 浮点型（二维） 均匀分布，uniform()指定区间（-1, 5）
         arr = np.random.uniform(-1, 5, size = (3, 4)) # 'size='可省略
         ```

   3. `np.random.shuffle()`

      1. 打乱数组 序列（类似 洗牌）, 原array 改变
      2. `np.random.shuffle(arr)`

3. numpy.ndarray类型 的创建

   1. `np.array(collection)`

      1. collection 为 序列型对象(list)、嵌套序列对象(list of list)

      2. list序列转换为 ndarray

         1. ```
            lis = range(10)
            arr = np.array(lis)
            ```

      3. list of list嵌套序列转换为ndarray

         1. ```
            lis_lis = [range(10), range(10)]
            arr = np.array(lis_lis)
            ```

            

   2. `np.zeros()`,  全0数组。

      2. 第一个参数是元组，指定大小，如(3, 4)
      3. `np.zeros((3, 4))`
      
   3. `np.ones()`, 全1数组。
   
      2. 第一个参数是元组， 指定大小，如(3, 4)
      3. `np.ones((2, 3))`
      
   4. `np.empty()` ,  初始化数组，返回的是 随机值（内存里的随机值）

      1. `np.empty((3, 3))`
      2. 指定 类型
      
         1. `np.empty((3, 3), int)`
      
   5. `np.arange()` , 创建 一维 ndarray 数组,
   
      2. 类似 python 的 range()
      3. `np.arange(15)`  # 15个元素的 一维数组
      
   6.  `reshape()` , 调整 维数

      2. `arr.reshape(3, 5))`  #3x5个元素的 二维数组
      
   
4. ndarray的元素的类型

   1. `dtype`,  指定数组的数据类型

      2. 值: 类型名位数，如 np.float64, np.int32
      3. `np.zeros((3, 4), dtype=np.float64)`
   
2. `astype()`方法, 实例方法, 没有类方法
   
   1. 转换数据类型
      2. `arr.astype(np.int32)`  #数组的元素类型转换为int32
   
   

#### 2.2 ndarray的矩阵处理

1. 数组 与 矩阵: 数组是编程概念,  矩阵、矢量是数学概念

   1. 计算机编程中，矩阵 用数组定义，矢量 用结构定义
   
2. 矢量运算

   1. 矢量运算：2个相同大小的数组, 运算作用在元素上

      1. ```
         #矢量与矢量运算
         arr = np.array([[1, 2, 3],[4, 5, 6]])
         ```

      2. `arr * arr`

         1. ```
            #对应元素相乘
            array([[ 1,  4,  9],
                   [16, 25, 36]])
            ```

      3. `arr + arr`

         1. ```
            #对应元素相加
            array([[ 2,  4,  6],
                   [ 8, 10, 12]])
            ```

   2. 矢量和标量运算：

      1. "广播" :  标量"广播"到各个元素

      2. `1. / arr`

         1. ```
            array([[1.        , 0.5       , 0.33333333],
                   [0.25      , 0.2       , 0.16666667]])
            ```

      3. `2. * arr`

         1. ```
            array([[ 2.,  4.,  6.],
                   [ 8., 10., 12.]])
            ```

3. ndarray的索引与切片[],  切片是原数据上的操作

   1. 一维数组的 索引与切片, 与 Python 的列表索引 相似

      2. ```
         arr = np.arange(10)
         arr[2:5]
         ```
      
         1. `array([2, 3, 4])`

   2. 多维数组的 索引与切片

      1. `arr[r1:r2, c1:c2]`

      2. `arr[1,1]` 等价 `arr[1][1]`

      3. `[:]`  某个维度的数据

      4. 示例

         1. `arr = np.arange(12).reshape(3,4)`

            1. ```
               array([[ 0,  1,  2,  3],
                      [ 4,  5,  6,  7],
                      [ 8,  9, 10, 11]])
               ```
   
         2. ```
            arr[0:2, 2:]=10  #切片改变原数组
            arr
            ```
   
            1. ```
               array([[ 0,  1, 10, 10],
                      [ 4,  5, 10, 10],
                      [ 8,  9, 10, 11]])
               ```
   
         3. 切片改变原数组,  python 中 list 也改变原 list

            1. ```
               list1= [2, 3, 5, 9] #输出[2, 3, 5, 9]
               list1[2:4]=[100,100] #切片修改
               list1 #输出[2, 3, 100, 100]
               ```
   
               

   3. 条件索引与bool索引

      1. arr[condition]  #condition 多个条件组合

         1. 条件组合 用 `&与, |或` 连接， 不是Python的 `and or`

      2. 示例

         1. `data_arr = np.random.rand(3,3)`

            1. data_arr

               1. ```
                  array([[0.10572477, 0.28315792, 0.41886806],
                         [0.25816292, 0.18435384, 0.31301756],
                         [0.16584744, 0.8978951 , 0.87378172]])
                  ```
   
         2. ```
            year_arr = np.array([[2000, 2001, 2000],
                                 [2005, 2002, 2009],
                                 [2001, 2003, 2010]])
            ```
   
         3. ```
            is_year_after_2005 = year_arr >= 2005 #得bool值ndarray
            is_year_after_2005  #输出bool值数组
            ```
   
            1. ```
               #type是<class 'numpy.ndarray'>
               array([[False, False, False],
                      [ True, False,  True],
                      [False, False,  True]])
               ```
            
         4. `data_arr[is_year_after_2005]`,   bool索引
         
            1. `array([0.25816292, 0.31301756, 0.87378172])`
         
         5. `data_arr[(year_arr <= 2005) & (year_arr % 2 == 0)]`  #条件索引, 条件由bool数组组成
         
            1. `array([0.10572477, 0.41886806, 0.18435384])`
   
4. ndarray类型的数组维度转换函数：transpose()

   1. 高维数组 指定 维度索引 (0, 1, 2, …)，参数是元组

   2. 示例
   
      1. ```
         arr = np.random.rand(2,3)    # 2x3 数组
         arr.transpose()  # 转换为 3x2 数组
         ```
   
      2. ```
         arr = np.random.rand(2,3,4) # 2x3x4 数组，2对应0，3对应1，4对应3
         arr.transpose((1,0,2)) #根据维度编号，转为为3x2x4 数组
         ```
   
         

#### 2.3 ndarray的元素处理

1. 元素计算 函数 , (只有 类方法, 没有实例方法)

   1. 函数
      1. `np.ceil()`: 向上最接近的整数，参数是 number 或 array
      2. `np.floor()`: 向下最接近的整数，参数是 number 或 array
      3. `np.rint()`: 四舍五入，参数是 number 或 array
      4. `np.isnan()`: 判断元素是否为 NaN(Not a Number)，参数是 number 或 array
      5. `np.multiply()`: 元素相乘，参数是 number 或 array
      6. `np.divide()`: 元素相除，参数是 number 或 array
      7. `np.abs()`：元素的绝对值，参数是 number 或 array
      8. `np.where(condition, x, y)`: 三元运算符，x if condition else y
   2. 示例
      1. `arr = np.random.randn(2,3)`
      2. `np.ceil(arr)`   
      3. 报错: arr.ceil()  #'numpy.ndarray' object has no attribute 'ceil'

2. 元素统计 , (只有 类方法, 没有实例方法)

   1. 函数

      1. `np.mean()` : 所有元素的平均值，参数是 number 或 array
      2. `np.sum()`：所有元素的平均值，参数是 number 或 array
      3. `np.max()` : 所有元素的最大值，参数是 number 或 array
      4.  `np.min()`：所有元素的最小值，参数是 number 或 array
      5. `np.std()` : 所有元素的标准差，参数是 number 或 array
      6.  `np.var()`：所有元素的方差，参数是 number 或 array
      7. `np.argmax()` :  最大值的下标索引值，参数是 number 或 array
      8. `np.argmin()`：最小值的下标索引值，参数是 number 或 array
      9. `np.cumsum()` : 返回一个一维数组，每个元素是之前所有元素的 累加和，参数是 number 或 array
      10. `np.cumprod()`：返回一个一维数组，每个元素都是之前所有元素的累乘积，参数是 number 或 array
      11. 小结
          1. 多维数组默认统计全部维度
          2. `axis`参数指定轴 统计，`0`按列统计，`1`按行统计

   2. 示例

      1. ```
         arr = np.arange(12).reshape(3,4)
         np.cumsum(arr)
         ```

3. 元素判断函数

   1. 数据: `arr = np.random.randn(2,3)`
   2. `np.any()`
      1. 有一个元素满足条件，返回True
      2. `np.any(arr > 0)`
   3. `np.all()`
      1.  所有的元素满足条件，返回True
      2. `np.all(arr > 0)`
   
4. `np.unique()` #元素去重排序函数,  类似于Python的set集合

   1. 示例

      1. ```
         arr = np.array([[1, 2, 1], [2, 3, 4]])
         np.unique(arr)
         ```
   
         1. `[1 2 3 4]`

#### 2.4实战案例：2016美国总统大选民意调查统计

1. 项目

   1. 地址：https://www.kaggle.com/fivethirtyeight/2016-election-polls
   2. 数据集 : 包含了2015年11月至2016年11月期间对于2016美国大选的选票数据，共27列数据

2. 读取 2方法

   1. `np.loadtxt()` 方法读取

      1. ```
         # loadtxt
         import numpy as np
         
         # csv 名逗号分隔值文件
         filename = './presidential_polls.csv'
         
         # 通过loadtxt()读取本地csv文件 
         data_array = np.loadtxt(filename,      # 文件名
                                 delimiter=',', # 分隔符
                                 dtype=str,     # 数据类型，数据是Unicode字符串
                                 usecols=(0,2,3)) # 指定读取的列号
         
         # 打印ndarray数据，默认保留第一行
         print(data_array, data_array.shape)
         ```

   2. `np.genfromtxt()`  方法读取

      1. ```
         import numpy as np
         # 读取列名，即第一行数据
         with open(filename, 'r') as f:
             col_names_str = f.readline()[:-1] # [:-1]表示不读取末尾的换行符'\n'
         
         # 将字符串拆分，并组成列表
         col_name_lst = col_names_str.split(',')
         
         # 使用的列名：结束时间，克林顿原始票数，川普原始票数，克林顿调整后票数，川普调整后票数
         use_col_name_lst = ['enddate', 'rawpoll_clinton', 'rawpoll_trump','adjpoll_clinton', 'adjpoll_trump']
         
         # 获取列的索引号
         use_col_index_lst = [col_name_lst.index(use_col_name) for use_col_name in use_col_name_lst]
         
         # 通过genfromtxt()读取本地csv文件，
         data_array = np.genfromtxt(filename,      # 文件名
                                 delimiter=',', # 分隔符
                                 
                                 dtype=str,     # 数据类型，数据不再是Unicode字符串
                                 usecols=use_col_index_lst)# 自定义列索引号
         
         
         # genfromtxt()与loadtxt区别: genfromtxt()不能通过 skiprows 跳过第一行的
         # ['enddate' 'rawpoll_clinton' 'rawpoll_trump' 'adjpoll_clinton' 'adjpoll_trump']
         
         # 去掉第一行
         data_array = data_array[1:]
         
         # 打印ndarray数据
         print(data_array[1:], data_array.shape)
         ```

   3. `np.genfromtxt()`与np.loadtxt 用 usecols 参数指定列标签

### 3 数据分析工具Pandas

1. Pandas : 名称来自 面板数据（panel data）和Python数据分析（data analysis）
   2. 结构化数据的分析工具集， 有 `高级数据结构` 和 `数据操作工具`
   3. 基础是 NumPy， 高性能 矩阵运算
   4. 应用: 数据挖掘，数据分析 , 数据清洗
2. 网址 : `http://pandas.pydata.org`

#### 3.1 Pandas的数据结构

1. Pandas的数据结构

   1. Series 和 DataFrame
   2. `import pandas as pd`

2. Series:  似 一维数组

   1. 组成
      1. 索引(index)在左，数据(values)在右, 一一对应
      2. 索引 默认自动创建
      
   2. 构建 `np.Series()`
   
      1. list构建Series
   
         1. `ser = pd.Series(range(10))`
            1. 自动创建 index 索引
            2. 返回类型: `pandas.core.series.Series`
   
      2. dict构建Series
   
         1. ```
            year_data = {2001: 17.8, 2002: 20.1, 2003: 16.5}
            ser = pd.Series(year_data)
            ```
   
            1. ```
               2001    17.8
               2002    20.1
               2003    16.5
               dtype: float64
               ```
   
            2. 返回类型: `pandas.core.series.Series`
   
   3. 属性
   
      1. `ser.values`  #取数据
   
      2. `ser.index`  #取索引
   
         1. 索引类型: 数值型 索引, 字符串/字符 索引
         2. 运算结果不影响索 引与数据的对应关系
            1. `ser * 2`
            2. `ser > 15`
         3. 索引取数据
            1. `ser[idx]`,  如:  `ser[3]`
         
      3. name属性,  取值或赋值都可以
      
         1. 对象名：ser.name
      
         2. 对象索引名：ser.index.name
      
         3. 示例
      
            1. ```
               #name属性
               ser.name = 'temp'
               ser.index.name = 'year'
               ```
      
         4. `ser.head()`    #查看前5个数据
      
            1. ```
               year
               2001    17.8
               2002    20.1
               2003    16.5
               Name: temp, dtype: float64
               ```
      
   4. 方法
      
      1. `ser.head(3)`
   
3. DataFrame :  类似 二维数组 或 表格 (如，excel, R中的data.frame)

   1. 每列数据的 类型 可以不同

      1. 结构 : 列标签columns, 行索引 index, 数据 values
      
   2. 构建 `np.DataFrame()`

      1. ndarray构建DataFrame

         1. ```
            arr = np.random.randn(5,4)
            df = pd.DataFrame(arr)
            ```
   
      2. dict构建DataFrame

         1. ```
            data_dict = {'A': 1,    #自动添加数值
                         'B': pd.Timestamp('20170426'),#自动添加时间
                         'C': pd.Series(1, index=list(range(4)),dtype='float32'),
                         'D': np.array([3] * 4,dtype='int32'),
                         'E': ["Python","Java","C++","C"],
                         'F': 'ITCast' }
            
            df = pd.DataFrame(data_dict)
            ```
   
            1. 结果

               1. ```
                     A          B    C  D       E       F
                  0  1 2017-04-26  1.0  3  Python  ITCast
                  1  1 2017-04-26  1.0  3    Java  ITCast
                  2  1 2017-04-26  1.0  3     C++  ITCast
                  3  1 2017-04-26  1.0  3       C  ITCast
                  ```
   
   3. 属性

      1. 列索引

         1. `列索引` 获取列数据（返回: Series类型）

            1. `df[col_idx]` 或 `df.col_idx`
            2. 示例
               1. `df['A']`
               2. `df.A`
         
         2. 增加 列数据
         
            1. `df[new_col_idx] = data`
         
               1. 类似 Python 的 dict 添加 key-value
         
            2. 示例
         
               1. ```
                  # 增加列
                  df['G'] = df_obj2['D'] + 4
                  ```
         
                  1. ```
                     #df
                          A          B    C  D       E       F  G
                     0  1.0 2017-01-02  1.0  3  Python  ITCast  7
                     1  1.0 2017-01-02  1.0  3    Java  ITCast  7
                     2  1.0 2017-01-02  1.0  3     C++  ITCast  7
                     3  1.0 2017-01-02  1.0  3       C  ITCast  7
                     ```
         
         3. del, 真的删除 列数据
         
            1. `del df[col_idx]`
            2. 示例:  `del df['G']`

#### 3.2 Pandas的索引操作

1. 索引对象 Index: Series 和 DataFrame 的索引都是 Index 对象

   1. `ser.index`,   与 `df.index`
   2. 索引对象 不可变，为了数据的 安全
      1. 报错  df.index[2]=2
      1. type(ser.index)   #<class 'pandas.core.indexes.base.Index'>
   3. Index种类
      1. Index，索引
      2. Int64Index，整数索引
      3. MultiIndex，层级索引
      4. DatetimeIndex，时间戳类型
   
2. Series索引, []

   1. 参数:  index 指定 行索引名

      1. `ser = pd.Series(range(5), index = ['a', 'b', 'c', 'd', 'e'])`

   2. 行标签 / 行索引

      1. `ser[‘label’]`  #标签索引
      2.  `ser[pos]`  #下标索引

   3. 切片:  行索引切片, 行标签切片

      1. ser[2:4]  #下标索引 切片
      2. ser[‘label1’: ’label3’]  #标签索引 切片
         1. 标签索引切片, 包含终止索引

   4. 不连续索引

      1. `ser[‘label1’, ’label2’, ‘label3’]]`

   5. 布尔索引

      1. ```
         #布尔索引
         ser_bool = ser > 2
         ser[ser_bool]
         ```

3. DataFrame索引

   1. DataFrame索引

      1.  Row index,  df.index
      2. Column index, df.columns
      3.  DataFrame 列标签 不能直接切片
          1.  正确: df[1:3],   df[['a','b','c']]
          2.  不正确: df[1:3, ['a', 'b', 'c']]

   2. columns 指定 列索引名

      1. `df = pd.DataFrame(np.random.randn(5,4), columns = ['a', 'b', 'c', 'd'])`

         1. ```
            #输出结果
                      a         b         c         d
            0  1.440459  0.778108 -1.289494 -0.065919
            1 -0.071190 -0.904778  1.223471  1.376177
            2 -0.176678  2.218539 -0.622548 -0.309570
            3  0.160144  0.505210 -1.671180 -2.451756
            4 -1.407006  0.729106 -0.153094  0.954613
            ```

   3. 列索引 取值

      1. `df['a']`  #返回Series类型
      2. `df[['a']]` #返回 DataFrame类型

   4. 不连续索引 取值

      1. `df[[‘label1’, ‘label2’]]`
      2. 示例
         1. `df[['a','c']]`
         2. `df[[1, 3]]`

4. 高级索引：标签、位置、混合
   1. 高级索引 有3种
   2. loc类:  标签索引
      1. DataFrame  列不能直接切片
         1. loc 做切片, 列标签 
      2. Series, 可直接切片
         1. `ser['b':'d']`
         2. `ser.loc['b':'d']`
      3. DataFrame
         1. `df.loc[0:2, 'a']]`  #行索引切片，列索引
            1. 此处的 `0:2` 是标签名切片. 标签名与下标名 重名
   3. iloc类:  位置索引
      1. 基于标签索引的 下标索引
      2. Series,  可直接切片
         1. `ser[1:3]`
         2. `ser.iloc[1:3]`
      3. DataFrame, 不能直接切片
         1. df.iloc[0:2, 0],  #此处的 `0:2` 是下标切片. 下标名与标签名 重名
   4. ix 标签与位置混合索引
      1. 此功能没有了
   5. 注意
      1. DataFrame索引操作，看作ndarray的索引操作
      2. 标签的切片索引 包含末位

#### 3.3 Pandas的对齐运算

 1. `+`对齐运算: 1. 索引对齐 运算. 没对齐的位置 补NaN, 	2. 数据清洗 过程

 2. Series 对齐运算

     1. 数据

         1. ```
            ser1 = pd.Series(range(10, 20), index=range(10))
            ser2 = pd.Series(range(20, 25), index=range(5))
            ```

     2. Series 按 行索引 对齐

     3. Series的对齐运算

         1. `s1 + s2`

             1. ```
                0    30.0
                1    32.0
                2    34.0
                3    36.0
                4    38.0
                5     NaN
                6     NaN
                7     NaN
                8     NaN
                9     NaN
                dtype: float64
                ```

                 	1. 没对齐的 补 `NaN`

 3. DataFrame的对齐运算

     1. 数据

         1. ```
            df1 = pd.DataFrame(np.ones((2,2)), columns = ['a', 'b'])
            df2 = pd.DataFrame(np.ones((3,3)), columns = ['a', 'b', 'c'])
            ```

     2. DataFrame按行索引、列索引对齐

     3. DataFrame的对齐运算

         1. ```
            # DataFrame对齐操作
            df1 + df2
            ```

            1. ```
                    a    b   c
               0  2.0  2.0 NaN
               1  2.0  2.0 NaN
               2  NaN  NaN NaN
               ```

 4. 填充未对齐的数据

     	1. fill_value
          	1. `add`, `sub`, `div`, `mul`
          	2. `fill_value`指定填充值
               	1. 未对齐的数据 和 填充值 `fill_value` 运算
          	3. 示例
               	1. `s1.add(s2, fill_value=-1)`  #对象方法, 没有类方法
               	2. `df1.sub(df2, fill_value=2.)`  #对象方法, 没有类方法

#### 3.4 Pandas的函数

1. apply 和 applymap

   1. 直接用 NumPy 的函数	

      1. ```
         #Numpy ufunc 函数
         df = pd.DataFrame(np.random.randn(5,4) - 1)
         np.abs(df)
         ```

   2. apply 应用函数 到列或行

      1. `df.apply(lambda x: x.max())`  #每一列 应用到 max() 函数
         1. 默认axis=0，方向是列
      2. `df.apply(lambda x : x.max(), axis=1)`    #轴 axis=1, 指定行
      
   3. applymap 应用函数到 每个元素
   
      1. ```python
         func = lambda x : '%.2f' % x
         df.applymap(func)
         ```
   
         1. 运行结果
   
            1. ```
                      0      1      2      3
               0   0.10  -0.93  -0.70  -0.13
               1  -0.80  -0.53   2.42  -1.79
               2  -2.18  -1.18   0.28   0.56
               3  -0.13  -0.01  -2.17  -1.11
               4  -1.45  -0.64  -1.73  -0.77
               ```
   
2. 排序

   1. `sort_index()`  #索引 排序.  有对象方法, 无类方法

      1. 默认 升序，ascending=False 降序

      2. Series

         1. ```
         # Series
            ser = pd.Series(range(10, 15), index = np.random.randint(5, size=5))
         
            ser.sort_index() # 0 0 1 3 3
         
            ```
         
      3. DataFrame
      
         1. 注意 参数 axis
      
         1. ```
            #DataFrame
            df = pd.DataFrame(np.random.randn(3, 5), 
                            index=np.random.randint(3, size=3),
                               columns=np.random.randint(5, size=5))
            
            df_sort_index = df.sort_index(axis=1, ascending=False)
            ```
      
   2. sort_values(by='column name')   实例方法, 无类方法

      1. 根据 唯一的列名 排序， 有相同列名则报错

      2. 示例

         1. Series: `ser.sort_values()`

         2. DataFrame:  `df_sort_value = df.sort_values(by=0, ascending=False)`

            

3. NaN  :  缺失数据

   1. `df = pd.DataFrame([np.random.randn(3), [1., 2., np.nan], [np.nan, 4., np.nan], [1., 2., 3.]])`

      1. ```
             0         1         2
         0 -0.284696 -0.666545 -0.171306
         1  1.000000  2.000000       NaN
         2       NaN  4.000000       NaN
         3  1.000000  2.000000  3.000000
         ```
      
   2. `isnull()`,  `isna()`,   判断是否存在缺失值
   
      1. `df.isnull()`,  `df.isna()`   #实例方法
      2. `pd.isnull()`, `pd.isna()`   #类方法
      3. 小结, 都可以
      
   3. dropna()  #丢弃缺失值,  返回新的, 原数据不变
   
      1. `df1.dropna()`  #实例方法, 无类方法
      2. `df1.dropna(axis=1)`  #参数 axis轴方向，丢弃 含NaN的行或列
      
   4. fillna()    #填充缺失数据：

      1. `df1.fillna(5)`  #实例方法, 无类方法
   
         

#### 3.5 Pandas的层级索引

 1. 层级索引（hierarchical indexing）

     1. Series对象， 索引Index 是两个子 list 组成的 list，第一个子list 外层索引，第二个list 内层索引

     2. `ser = pd.Series(np.random.randn(12), index=[ ['a', 'a', 'a', 'b', 'b', 'b', 'c', 'c', 'c', 'd', 'd', 'd'], [0, 1, 2, 0, 1, 2, 0, 1, 2, 0, 1, 2]])`

         1. ```
        a  0    0.076993
                1    0.998832
                2    0.310744
             b  0   -1.206850
                1   -0.860901
                2    0.220352
             c  0    0.648334
                1    0.374740
                2   -0.357488
             d  0   -0.549048
                1   -0.037095
                2    0.402417
             dtype: float64
           ```
     
         2. `type(ser.index)`   #`pandas.core.indexes.multi.MultiIndex`

 2. MultiIndex 索引对象

      1. `ser.index`    #类型: `pandas.indexes.multi.MultiIndex`
      
 3. 取子集

        1. 用途: 分组、生成透视表
               2. 外层选取,  `ser['a']`  #外层索引
               3. 内层选取: `ser[[:, '0']]`  #参数list, 传入两个元素，前者外层索引，后者内层索引
     
 4. swaplevel()  #交换内层与外层索引, 不改变原数据
      1. `ser.swaplevel()`  #交换内外层索引
      
 5. ~~排序分层~~

      1. ~~sortlevel()~~   #pandas的包中在0.23.4版本中就取消了sort方法

#### 3.6 Pandas统计计算和描述

1. `df = pd.DataFrame(np.random.randn(5,4), columns = ['a', 'b', 'c', 'd'])`

   1. ```
      #df
                a         b         c         d
      0  0.859905  0.436309  1.054864  0.579178
      1  0.962565 -1.418776  0.777851  0.979219
      2 -1.230637  2.532247 -0.971146 -0.917077
      3  0.454459 -0.947036 -1.843697  0.120356
      4 -0.587429 -0.647012 -0.362542  0.267256
      ```
   
2. 统计计算: sum, mean, max, min #实例方法, 无类方法

   1.  参数
      1. axis=0 每列统计，axis=1 每行统计
      2. skipna 排除缺失值， 默认 True
   2.  `df1.min(axis=1, skipna=False)`
   
3. describe()  #统计描述, 实例方法, 无类方法
   1. `df1.describe()`
   
4. 统计描述方法

   1. | 方法               | 实例方法        | 类方法 | 说明                                                         |
      | ------------------ | --------------- | ------ | ------------------------------------------------------------ |
      | count              | df.count()      | 无     | 非NA值的数量                                                 |
      | describe           | df.describe()   | 无     | 针对Series或各DataFrame列计算汇总统计                        |
      | min、max           | df.min()        | 无     | 计算最小值和最大值                                           |
      | ~~argmin、argmax~~ |                 | 无     | ~~计算能够获取到最小值和最大值的索引位置(整 数)~~, 已经取消了 |
      | idxmin、idxmax     | df.idxmin()     | 无     | 计算能够获取到最小值和最大值的索引值                         |
      | quantile           | df.quantile()   | 无     | 计算样本的分位数(0到1)                                       |
      | sum                | df.sum()        | 无     | 值的总和                                                     |
      | mean               | df.mean()       | 无     | 值的平均数                                                   |
      | median             | df.median()     | 无     | 值的算术中位数(50%分位数)                                    |
      | mad                | df.mad()        | 无     | 根据平均值计算 平均绝对离差                                  |
      | var                | df.var()        | 无     | 样本值的方差                                                 |
      | std                | df.std()        | 无     | 样本值的标准差                                               |
      | skew               | df.skew()       | 无     | 样本值的偏度(三阶矩)                                         |
      | kurt               | df.kurt()       | 无     | 样本值的峰度(四阶矩)                                         |
      | cumsum             | df.cumsum()     | 无     | 样本值的累计和                                               |
      | cummin、cummax     | df.cummin()     | 无     | 样本值的累计最大值和累计最小值                               |
      | cumprod            | df.cumprod      | 无     | 样本值的累计积                                               |
      | diff               | df.diff()       | 无     | 计算一阶差分(对时间序列很有用)                               |
      | pct_ change        | df.pct_change() | 无     | 计算百分数变化                                               |

      

#### 3.7 Pandas分组与聚合

1. 分组 (groupby)

   1. 数据集 分组，每组 统计分析
      1. SQL : 数据过滤，分组聚合
      2. pandas : groupby 分组
   2. 分组运算过程：split->apply->combine
      1. 拆分：分组的根据
      2. 应用：每个分组 计算规则
      3. 合并：每个分组的计算结果合并

2. 分组返回类型 : GroupBy对象( DataFrameGroupBy，SeriesGroupBy) 分组

   1. groupby() 分组,  按列名分组：df.groupby(‘label’)

      1. 返回 GroupBy对象：
         1. 两种: DataFrameGroupBy，SeriesGroupBy
         2. GroupBy对象没有 实际运算，只是分组的中间数据
         3. GroupBy对象支持 迭代操作
      
   2. 分组 : 按列标签
   
      1. ```
      data_dict = {'key1' : ['a', 'b', 'a', 'b', 
                               'a', 'b', 'a', 'a'],
                     'key2' : ['one', 'one', 'two', 'three',
                               'two', 'two', 'one', 'three'],
                     'data1': np.random.randn(8),
                     'data2': np.random.randn(8)}
         df = pd.DataFrame(data_dict)
         ```
         
         1. ```
            #df
              key1   key2     data1     data2
            0    a    one -1.716734 -0.061253
            1    b    one -1.015811 -0.150768
            2    a    two  1.475870  1.111706
            3    b  three -0.265859 -0.485278
            4    a    two -2.137109 -1.239068
            5    b    two -0.418076 -0.126101
            6    a    one -0.080054  0.548272
            7    a  three -0.645072  0.933395
            ```
         
            
   
      2. 按 列标签 分组 (按列分组)
   
         1.  dataframe  按 列标签 key1 分组 (即 DataFrame 分组)
            1. `df.groupby('key1')`
            2. 返回值类型: `pandas.core.groupby.generic.DataFrameGroupBy`
         2. dataframe的 data1 列数据 按列标签 key1 分组 (即 Series 分组)
            1. `df['data1'].groupby(df['key1'])`
            2. 返回值类型: `pandas.core.groupby.generic.SeriesGroupBy`,  因为 df['data1'] 是 Series类型
         3. 多列多层分组, 按 key 的顺序进行
            1. df.groupby([‘label1’, ‘label2’])  #多层列表分组
            2. `df.groupby(['key2', 'key1'])`
      
      3. 按 自定义 key 分组
   
         1. df.groupby(self_def_key)
   
            1. self_def_key 是自定义的 key, 是`列表` 或 `多层列表`, 个数相等
   
         2. 示例
   
            1. 自定义 key , 一层列表 分组
   
               1. ````
                  self_def_key = [0, 1, 2, 3, 3, 4, 5, 7]
                  df1.groupby(self_def_key)
                  ````
      
            2. 自定义 key，多层列表 分组
   
               1. `df1.groupby([df['key1'], df['key2']])`
                  1. 用 df['key1'], df['key2'] 构建 list 作为 key
      
      4. 按 数据类型 分组

         1. 类型:  df.dtypes   #返回各列的数据类型
   
         2. 分组: 
   
            1. `df.groupby(df.dtypes, axis=1)`   #axis=0, 为空
   
            2. ```
               for i in df.groupby(df.dtypes, axis=1):
               	print(i)
               ```
         
               1. ```
                  (dtype('float64'),       data1     data2
                  0  1.247981 -0.019596
                  1 -0.402840  1.287640
                  2  0.101324  1.195777
                  3  0.910502 -0.571829
                  4 -0.352370 -0.338540
                  5 -2.015607 -1.057835
                  6  0.482916  0.416173
                  7 -0.190400 -1.078155)
                  (dtype('O'),   key1   key2
                  0    a    one
                  1    b    one
                  2    a    two
                  3    b  three
                  4    a    two
                  5    b    two
                  6    a    one
                  7    a  three)
                  ```
         
      5. 其他分组方法
      
         1. 数据
   
            1. ```
               df = pd.DataFrame(np.random.randint(1, 10, (5,5)),
                                      columns=['a', 'b', 'c', 'd', 'e'],
                                      index=['A', 'B', 'C', 'D', 'E'])
               df.iloc[1, 1:4] = np.NaN
               ```
      
            2. `df`
      
               1. ```
                        a    b    c    d  e
                  A  7  1.0  1.0  9.0  1
                  B  3  NaN  NaN  NaN  3
                  C  2  2.0  3.0  7.0  1
                  D  7  7.0  1.0  1.0  4
                  E  3  3.0  8.0  6.0  1
                  ```
      
                  
      
         2. 按指定列标签分组
   
            1. dict_rule = {'a':'Python', 'b':'Python', 'c':'Java', 'd':'C', 'e':'Java'} 
            2. df.groupby(dict_rule, axis=1)    #按列标签分组
            3. 计算
               1. df.groupby(dict_rule, axis=1).size()
               2. df.groupby(dict_rule, axis=1).count()  #非NaN的个数
               3. df.groupby(dict_rule, axis=1).sum()
      
         3. 按 函数 分组，函数的参数 行索引或列索引
      
            1. ```
               def func_key(id):
                   """
                       id 为列索引或行索引
                   """
                   return len(idx)
               ```
      
            2. `df.groupby(func_key)`  #等价于 `df.groupby(len)`
      
         4. 按索引级别分组分组
   
            1. ```
               #定义多重列标签
               columns = pd.MultiIndex.from_arrays([['Python', 'Java', 'Python', 'Java', 'Python'],['A', 'A', 'B', 'C', 'B']], names=['level1', 'level2'])
               ```
      
            2. ```
               #创建 DataFrame
               df = pd.DataFrame(np.random.randint(1, 10, (5, 5)), columns=columns)
               ```
      
            3. ```
               # 根据 level1 分组, 必须指定axis=1
               df.groupby(level='level1', axis=1)
               ```
      
            4. ```
               # 根据 level2 分组, 必须指定axis=1
               df.groupby(level='level2', axis=1)
               ```
      
   3. 分组运算
   
      1. GroupBy对象 分组运算/多重分组运算的特点，如mean()
   
         1. 非数值数据不 分组运算
         
      2. 分组运算 示例
      
         1. `mean()`  #实例方法, 无类方法
      
            1. ```
               data_grouped = df['data1'].groupby(df['key1'])
               data_grouped.mean()
               ```
      
               1. ```
                  key1
                  a   -0.620620
                  b   -0.566582
                  Name: data1, dtype: float64
                  ```
      
         2. `size()`  #返回每个分组的元素个数. 实例方法, 无类方法
      
            1. `data_grouped.size()`
      
               1. ```
                  key1
                  a    5
                  b    3
                  Name: data1, dtype: int64
                  ```
            
      
      
   
3. GroupBy对象支持 迭代 , 

   1. 每次迭代返回一个 元组: (group_name, group_data)
      1.  可用于分组数据的具体运算

   ```
   data_dict = {'key1' : ['a', 'b', 'a', 'b', 
                         'a', 'b', 'a', 'a'],
               'key2' : ['one', 'one', 'two', 'three',
                         'two', 'two', 'one', 'three'],
               'data1': np.random.randn(8),
               'data2': np.random.randn(8)}
   df = pd.DataFrame(data_dict)
   ```

   1. 单层分组 迭代

      1. 单层分组，根据key1

         1. grouped = df.groupby('key1')

      2. Groupby 对象 迭代

         1. ```
            for group_name, group_data in grouped:
                 print(group_name)
                 print(group_data)
            ```

         2. 输出结果

            1. ```
               a   #print(group_name)
                 key1   key2     data1     data2
               0    a    one -1.716734 -0.061253
               2    a    two  1.475870  1.111706
               4    a    two -2.137109 -1.239068
               6    a    one -0.080054  0.548272
               7    a  three -0.645072  0.933395
               b
                 key1   key2     data1     data2
               1    b    one -1.015811 -0.150768
               3    b  three -0.265859 -0.485278
               5    b    two -0.418076 -0.126101
               ```

   2. 多层分组 迭代

      1. 多层分组，根据key1 和 key2

         1. grouped = df.groupby(['key1', 'key2'])

      2. Groupby对象 迭代

         1. ```
            for group_name, group_data in grouped:
                 print(group_name)
                 print(group_data)
            ```

         2. ```
            ('a', 'one')
              key1 key2     data1     data2
            0    a  one -1.716734 -0.061253
            6    a  one -0.080054  0.548272
            ('a', 'three')
              key1   key2     data1     data2
            7    a  three -0.645072  0.933395
            ('a', 'two')
              key1 key2     data1     data2
            2    a  two  1.475870  1.111706
            4    a  two -2.137109 -1.239068
            ('b', 'one')
              key1 key2     data1     data2
            1    b  one -1.015811 -0.150768
            ('b', 'three')
              key1   key2     data1     data2
            3    b  three -0.265859 -0.485278
            ('b', 'two')
              key1 key2     data1     data2
            5    b  two -0.418076 -0.126101
            ```
            
            

4. GroupBy类型 转换成 list 或 dict 类型

   1. GroupBy对象转换为 list

      1. list(grouped)

         1. ```
            [('a',
                key1   key2     data1     data2
              0    a    one  1.247981 -0.019596
              2    a    two  0.101324  1.195777
              4    a    two -0.352370 -0.338540
              6    a    one  0.482916  0.416173
              7    a  three -0.190400 -1.078155),
             ('b',
                key1   key2     data1     data2
              1    b    one -0.402840  1.287640
              3    b  three  0.910502 -0.571829
              5    b    two -2.015607 -1.057835)]
            ```
      
   2. GroupBy类型先转为list类型, 再转换为 dict 类型

      1. `dict(list(grouped))`

         1. ```
            {'a':   key1   key2     data1     data2
             0    a    one  1.247981 -0.019596
             2    a    two  0.101324  1.195777
             4    a    two -0.352370 -0.338540
             6    a    one  0.482916  0.416173
             7    a  three -0.190400 -1.078155,
             'b':   key1   key2     data1     data2
             1    b    one -0.402840  1.287640
             3    b  three  0.910502 -0.571829
             5    b    two -2.015607 -1.057835}
            ```
      
   3. 小结
   
      1. dict(grouped)  #报错, attribute of type 'str' is not callable  #str是不可调用的
      2. list 类型是可调用的
   
5. 聚合 (aggregation)

   1. agg()  聚合 : 分组数据 产生标量，如mean()、count(). 即 对分组后的数据 计算

      3. ```
         data_dict  = {'key1' : ['a', 'b', 'a', 'b', 'a', 'b', 'a', 'a'],
                     'key2' : ['one', 'one', 'two', 'three','two', 'two', 'one', 'three'],
                     'data1': np.random.randint(1,10, 8),
                     'data2': np.random.randint(1,10, 8)}
                     
         df = pd.DataFrame(data_dict)
         ```
      
   2. 内置的聚合函数
   
      1. sum(), mean(), max(), min(), count(), size(), describe(), std(), var(), prod() 非NaN值的积, first() 每组第一个非NaN值, last 每分组的最后一非NaN值
         1. 只有 实例方法, 没有类方法
         2. `df.groupby('key1').sum()`  #pd.sum(df) 报错 因为没有类方法
         3. `df.groupby('key1').describe()`
   
   3. grouped.agg(func)  #自定义函数，传入到agg
   
      1. ```
         #自定义聚合函数
         def peak_range(df):
             """
                 返回数值范围
             """
             return df.max() - df.min()
         ```
      
         1. `df..groupby('key1').agg(peak_range)`
      
   4. 多个聚合函数 
   
      1.  默认函数名为列名
         1. `df.groupby('key1').agg(['mean', 'std', 'count', peak_range])`
      2.  元组提供新的列名
         1. `df.groupby('key1').agg(['mean', 'std', 'count', ('range', peak_range)])`
      3. 小结
         1. list 类型不可hash的,  即 unhashable type: 'list'
         2. set 类型不可hash的,  即 unhashable type: 'set'
         3. dict 类型不可hash的,  即 unhashable type: 'dict'
         4. tuple 类型是可hash的

   5. dict 实现 不同的列 用聚合函数
   
      1. 每列 指定不同的聚合函数
         1. func_dict = {'data1':'mean', 'data2':'sum'}
         2. df.groupby('key1').agg(func_dict)  #指定列用指定的集合操作
      2. 同一列 指定多个聚合函数
         1. func_dict = {'data1':['mean', 'max'], 'data2':'sum'}
         2. df.groupby('key1').agg(func_dict)
   
6. 聚合运算后会改变原始数据的形状，如何保持原始数据的形状

   1. 方法1: merge() 方法   #有类方法, 实例方法

      1. merge的外连接，比较复杂
      2. pd.merge(df1, df2)  #类方法
      3. df.merge(df2)  #实例方法

   2. 方法2 :  transform(函数名 / lambda表达式)   #有实例方法, 无类方法

      1. transform 计算结果和原始数据的形状一致

      2. df.groupby('key1').transform(np.sum).add_prefix('sum_')

         1. `add_prefix()` 为返回数据的 列标签添加前缀
         1. df.groupby('key1').transform(sum).add_prefix('sum_')   #结果一致
         1. df.groupby('key1').transform('sum').add_prefix('sum_')   #结果一致

      3. 传入自定义函数
      
         1. ```
            # 自定义函数
            def diff_mean(s):
                """
                    返回数据与均值的差值
                """
                return s - s.mean()
            ```
      
         2. ```
            #transform() 传入自定义函数
            df.groupby('key1').transform(diff_mean)
            ```
      
            

7. groupby.apply(func名)   #函数名没有引号

   1. 各分组 分别调用 func，最后结果用 pd.concat 合并 

   1. ```
      #读取数据, 定义列标签
      df_data = pd.read_csv(dataset_path, usecols=['LeagueIndex', 'Age', 'HoursPerWeek', 'TotalHours', 'APM'])
      
      #定义函数func
      def func(df, n=3, column='APM'):  #带默认参数
          """
              返回每个分组按 column 的 top n 数据
          """
          return df.sort_values(by=column, ascending=False)[:n]
          
      df.groupby('LeagueIndex').apply(func)  #每个分组分别调用 func 函数
      ```
      
      1. 结果
      
         1. ```
             LeagueIndex   Age  HoursPerWeek  TotalHours       APM
            LeagueIndex                                                            
             1           2214            1  20.0          12.0       730.0  172.9530
                        2246            1  27.0           8.0       250.0  141.6282
                        1753            1  20.0          28.0       100.0  139.6362
            2           3062            2  20.0           6.0       100.0  179.6250
                        3229            2  16.0          24.0       110.0  156.7380
                        1520            2  29.0           6.0       250.0  151.6470
            3           1557            3  22.0           6.0       200.0  226.6554
                        484             3  19.0          42.0       450.0  220.0692
            ```
      
         2. 产生层级索引：外层索引是 `分组名`，内层索引是 df 的行索引
      
         3. 禁止产生层级索引, group_keys=False
         1. df.groupby('LeagueIndex', group_keys=False).apply(func)

#### 3.8 数据清洗、拼接、转化和重构

1. 数据清洗 缺失值`NaN`

   1. 数据清洗 是数据分析关键一步
   2. 缺失数据：pd.fillna()，pd.dropna()

2. 数据拼接/合并

   1. 合并: `pd.merge(df1, df2)`, `df1.merge(df2)`  #有类方法, 有实例方法

      ```
      pd.merge(left, right, how='inner', on=None, left_on=None, right_on=None, left_index=False, right_index=False,sort=True, suffixes=('_x', '_y'), copy=True, indicator=False, validate=None)

   1. 拼接: `pd.concat([df1, df2])`  #类方法, 无实例方法

   2. pd.merge(df1, df2) 或 df1.merge(df2), #有类方法, 实例方法

      1. 根据键连接 DataFrame 行

      2. 类似 SQL 的 join

      3. 数据

         1. ```
            df1 = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'],
                                    'data1' : np.random.randint(0,10,7)})
            df2 = pd.DataFrame({'key': ['a', 'b', 'd'],
                                    'data2' : np.random.randint(0,10,3)})
            ```
      
         2. ```
            #df1
              key  data1
            0   b      0
            1   b      0
            2   a      5
            3   c      1
            4   a      3
            5   a      7
            6   b      1
            ```
      
         3. ```
            #df2
              key  data2
            0   a      1
            1   b      7
            2   d      2
            
            ```
      
      1. pd.merge  合并方法

         1. 默认内合并, 等效 参数 how='inner', 即 结果中的键是交集

            1. 默认 相同列名作 “连接键”连接, 同名列合并为一列, 

               1. `pd.merge(df1, df2)`

                  1. ```
                       key  data1  data2
                     0   b      0      7
                     1   b      0      7
                     2   b      1      7
                     3   a      5      1
                     4   a      3      1
                     5   a      7      1
                     ```
            
            2. on 指定“连接键”,  同名标签做一列, 其他列标签各自成列
      
               1. `pd.merge(df1, df2, on='key')`   #on连接键合并为一列

                  1. ```
                       key  data1  data2
                     0   b      0      7
                     1   b      0      7
                     2   b      1      7
                     3   a      5      1
                     4   a      3      1
                     5   a      7      1
                     ```
            
            3. left_on，左表连接键，right_on，右连接键”. 左右连接键不合并, 各列标签各自成列
      
               1. ```
                  df1 = df1.rename(columns={'key':'key1'})
                  df2 = df2.rename(columns={'key':'key2'})
                  
                  # left_on，right_on分别指定左侧数据和右侧数据的“外键”
                  pd.merge(df1, df2, left_on='key1', right_on='key2')
                  ```
            
                  1. ```
                       key1  data1 key2  data2
                     0    b      0    b      7
                     1    b      0    b      7
                     2    b      1    b      7
                     3    a      5    a      1
                     4    a      3    a      1
                     5    a      7    a      1
                     ```
            
         2. “外连接”(outer)，结果是两表并集
      
            1. `df.merge(df1, df2, left_on='key1', right_on='key2', how='outer')`

               1. ```
                     data1 key1  data2 key2
                  0    8.0    b    0.0    b
                  1    8.0    b    0.0    b
                  2    6.0    b    0.0    b
                  3    3.0    a    9.0    a
                  4    4.0    a    9.0    a
                  5    9.0    a    9.0    a
                  6    5.0    c    NaN  NaN
                  7    NaN  NaN    3.0    d
                  ```
      
                  1. 缺失值 补 `NaN`
      
         3. “左连接”(left),  保留左表, 右表缺失值 补'NaN'

            1. `pd.merge(df1, df2, left_on='key1', right_on='key2', how='left')`

               1. ```
                     data1 key1  data2 key2
                  0      8    b    0.0    b
                  1      8    b    0.0    b
                  2      3    a    9.0    a
                  3      5    c    NaN  NaN
                  4      4    a    9.0    a
                  5      9    a    9.0    a
                  6      6    b    0.0    b
                  ```
      
                  
      
         4. “右连接”(right),  保留右表, 左表缺失值 补'NaN'

            1. `pd.merge(df1, df2, left_on='key1', right_on='key2', how='right')`

               1. ```
                     data1 key1  data2 key2
                  0    8.0    b      0    b
                  1    8.0    b      0    b
                  2    6.0    b      0    b
                  3    3.0    a      9    a
                  4    4.0    a      9    a
                  5    9.0    a      9    a
                  6    NaN  NaN      3    d
                  ```
      
         5. 按索引合并
      
            1. 方法: 参数 `left_index=True` 或 `right_index=True`

            2. ```
               df1 = pd.DataFrame({'key': ['b','b','a','c','a','a','b'],'data1' : np.random.randint(0,10,7)})
               df2 = pd.DataFrame({'data2' : np.random.randint(0,10,3)}, index=['a', 'b', 'd'])
               ```
      
               1. | df1                                                          | df2                                          |
                  | ------------------------------------------------------------ | -------------------------------------------- |
                  | key  data1<br/>0   b      3<br/>1   b      4<br/>2   a      8<br/>3   c      5<br/>4   a      2<br/>5   a      9<br/>6   b      4 | data2<br/>a      6<br/>b      3<br/>d      7 |

            3. pd.merge(df1, df2, left_on='key', right_index=True)     #左表的`key` 与 右表的 `index` 相同的值连接作结果的连接键 , 用左表的index作合成表的index
      
               1. ```
                      key  data1  data2
                  0   b      3      3
                  1   b      4      3
                  6   b      4      3
                  2   a      8      6
                  4   a      2      6
                  5   a      9      6
                  ```
      
      2. 参数 suffixes : 处理重名列标签, 添加指定的后缀
      
         1.  ```
            df1 = pd.DataFrame({'key': ['b','b','a','c','a','a','b'],'data' : np.random.randint(0,10,7)})
            df2 = pd.DataFrame({'key': ['a', 'b', 'd'],'data' : np.random.randint(0,10,3)})
            ```
      
         2.  默认重名列后缀: `_x` ,  `_y`
      
            1. `pd.merge(df1, df2, on='key')`
      
               1. ```
                    key  data_x  data_y
                  0   b       6       8
                  1   b       6       8
                  2   b       1       8
                  3   a       7       8
                  4   a       5       8
                  5   a       5       8
                  ```
      
         3.  指定后缀
      
            1. `pd.merge(df1, df2, on='key', suffixes=('_left', '_right'))`
      
               1. ```
                    key  data_left  data_right
                  0   b          6           8
                  1   b          6           8
                  2   b          1           8
                  3   a          7           8
                  4   a          5           8
                  5   a          5           8
                  ```
      
   3. pd.concat 数据 连接 : 轴方向, 多个对象连接

      1. `np.concatenate()`  之 numpy的连接

         1. ```
            arr1 = np.random.randint(0, 10, (3, 4))
            arr2 = np.random.randint(0, 10, (3, 4))
            ```
      
            1. | arr1                                                         | arr2                                                         |
               | ------------------------------------------------------------ | ------------------------------------------------------------ |
               | array([[9, 7, 0, 2],<br/>            [4, 5, 0, 8],<br/>            [0, 2, 8, 3]]) | array([[0, 2, 3, 6],<br/>            [9, 7, 3, 8],<br/>            [3, 9, 5, 1]]) |
      
               

         2. `np.concatenate([arr1, arr2])`  #默认 axis=0

            1. ```
               array([[9, 7, 0, 2],
                      [4, 5, 0, 8],
                      [0, 2, 8, 3],
                      [0, 2, 3, 6],
                      [9, 7, 3, 8],
                      [3, 9, 5, 1]]
               ```
      
         3. `np.concatenate([arr1, arr2], axis=1)`

            1. ```
               array([[9, 7, 0, 2, 0, 2, 3, 6],
                      [4, 5, 0, 8, 9, 7, 3, 8],
                      [0, 2, 8, 3, 3, 9, 5, 1]])
               ```
      
      2. pd.concat() 之 pandas 连接

         ```
         pd.concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False, keys=None, levels=None, names=None, verify_integrity=False)
         
         #参数
         axis 轴方向，默认axis=0
         join 指定合并方式，默认为outer
         
         #注意事项
         Series合并时查看行索引有无重复
         ```
         
         1. pd.concat() 之 Series连接/合并
         
            1. index 没有重复的情况	
      
               1. ```
                  ser1 = pd.Series(np.random.randint(0, 10, 5), index=range(0,5))
                  ser2 = pd.Series(np.random.randint(0, 10, 4), index=range(5,9))
                  ser3 = pd.Series(np.random.randint(0, 10, 3), index=range(9,12))
                  ```

                  1. | ser1                                                         | ser2                                                     | ser3                                             |
                     | ------------------------------------------------------------ | -------------------------------------------------------- | ------------------------------------------------ |
                     | 0    5<br/>1    5<br/>2    4<br/>3    6<br/>4    9<br/>dtype: int32 | 5    8<br/>6    2<br/>7    9<br/>8    6<br/>dtype: int32 | 9     2<br/>10    0<br/>11    0<br/>dtype: int32 |
         
               2. `pd.concat([ser2, ser3])`
         
                  1. 返回类型: `pandas.core.series.Series`
         
                  2. ```
                     5     8
                     6     2
                     7     9
                     8     6
                     9     2
                     10    0
                     11    0
                     dtype: int32
                     ```
         
               3. `pd.concat([ser1, ser2, ser3], axis=1)`
         
                  1. 返回类型: `pandas.core.frame.DataFrame`
         
                  2. ```
                           0    1
                     5   8.0  NaN
                     6   2.0  NaN
                     7   9.0  NaN
                     8   6.0  NaN
                     9   NaN  2.0
                     10  NaN  0.0
                     11  NaN  0.0
                     ```
      
               4. 小结

                  1. index, 连接键
                  2. axis=0, 或 默认时, 数据 轴 0  连接
                  3. axis=1, 数据 轴 1 连接
         
            2. index 有重复的情况
      
               1. ```
                  ser1 = pd.Series(np.random.randint(0, 10, 5), index=range(5))
                  ser2 = pd.Series(np.random.randint(0, 10, 4), index=range(4))
                  ser3 = pd.Series(np.random.randint(0, 10, 3), index=range(3))
                  ```

                  1. | ser1                                                         | ser2                                                     | ser3                                          |
                     | ------------------------------------------------------------ | -------------------------------------------------------- | --------------------------------------------- |
                     | 0    0<br/>1    0<br/>2    7<br/>3    4<br/>4    0<br/>dtype: int32 | 0    9<br/>1    8<br/>2    8<br/>3    3<br/>dtype: int32 | 0    0<br/>1    4<br/>2    5<br/>dtype: int32 |
         
               2. `pd.concat([ser2, ser3])`  #index值重复的, 都保留
         
                  1. ```
                     0    9
                     1    8
                     2    8
                     3    3
                     0    0
                     1    4
                     2    5
                     dtype: int32
                     ```
                     
                  2. ```
                     pd.concat([ser2, ser3])[1] 
                     #返回Series, 有两个值
                     1    5
                     1    5
                     dtype: int32
                     ```
            
                     
         
               3. pd.concat([ser2, ser3], axis=1)
         
                  1. ```
                          0    1
                     0  9  0.0
                     1  8  4.0
                     2  8  5.0
                     3  3  NaN
                     ```
            
               4. `pd.concat([ser2, ser3], axis=1, join='inner')`  #join='inner'  去除NaN所在的行或列
            
                  1. ```
                        0  1
                     0  9  0
                     1  8  4
                     2  8  5
                     ```
            
            3. pd.concat() 之 DataFrame连接/合并
            
               * 查看 行索引 和 列索引 有无重复
            
               * ```
                 df1 = pd.DataFrame(np.random.randint(0, 10, (3, 2)), index=['a', 'b', 'c'],columns=['A', 'B'])
                 df2 = pd.DataFrame(np.random.randint(0, 10, (2, 2)), index=['a', 'b'],columns=['C', 'D'])
                 ```
                 
                 * | df1                                      | df2                          |
                   | ---------------------------------------- | ---------------------------- |
                   | A  B<br/>a  8  0<br/>b  9  2<br/>c  8  3 | C  D<br/>a  4  0<br/>b  8  2 |
                 
                   
            
               1. axis=0 默认连接
            
                  1. `pd.concat([df1, df2])`  #等价于 axis=0  join='outer'
            
                     1. ```
                             A    B    C    D
                        a  8.0  0.0  NaN  NaN
                        b  9.0  2.0  NaN  NaN
                        c  8.0  3.0  NaN  NaN
                        a  NaN  NaN  4.0  0.0
                        b  NaN  NaN  8.0  2.0
                        ```
               
                  2. `pd.concat([df1, df2], join='inner')`
         
                     1. ```
                        Empty DataFrame
                        Columns: []
                        Index: [a, b, c, a, b]
                        ```
               
               2. axis=1 连接
            
                  1. `pd.concat([df1, df2], axis=1)`  ##等价于  join='outer'
            
                     1. ```
                           A  B    C    D
                        a  8  0  4.0  0.0
                        b  9  2  8.0  2.0
                        c  8  3  NaN  NaN
                        ```
               
                  2. `pd.concat([df1, df2], axis=1, join='inner')  `
            
                     1. ```
                           A  B  C  D
                        a  8  0  4  0
                        b  9  2  8  2
                        ```

3. 数据重构 : 数据 结构的新构建, 

   1. 数据结构: 表结构, 大括号结构

   2. 数据重构 方法

      1. `stack()`, `unstack()`  

         1. 有 实例方法, 没有类方法
         
      2. `df.stack()`, `ser.unstack()`
      
      2. stack(): 本质, 把 表结构 转为 大括号结构
      
         1. DataFrame->Series  :   列索引旋转为行索引，完成层级索引
      
         2. `df = pd.DataFrame(np.random.randint(0,10, (5,2)), columns=['data1', 'data2'])`  
      
            1. ```
                     data1  data2
               0      0      6
               1      3      4
               2      9      1
               3      6      8
               4      5      4
            ```
         
         3. `df_stack = df.stack()`
         
            1. ```
               0  data1    0
                  data2    6
               1  data1    3
                  data2    4
               2  data1    9
                  data2    1
               3  data1    6
                  data2    8
               4  data1    5
                  data2    4
               dtype: int32
               ```
         
            2. DataFrame --> Series, 或从 表 转成 大括号形式
         
         4. 小结: 有 实例方法 `df.stack()` , 无类方法
         
      3. unstack()
      
         1. 将层级行索引展开
         1. Series->DataFrame. 从 大括号转成表
         
      2. 默认操作内层索引，即 `level=-1`
            1. `df_stack.unstack()`
            
      3. level 指定 索引的级别
            1. `df_stack.unstakc(level=0)`

4. 数据转换:  去重 duplicated(), 映射 map() , 替换 replace()

   1. 处理重复数据

      * df = pd.DataFrame({'data1' : ['a'] * 4 + ['b'] * 4, 'data2' : np.random.randint(0, 4, 8)})

        * ```
            data1  data2
          0     a      2
          1     a      1
          2     a      0
          3     a      1
          4     b      3
          5     b      1
          6     b      3
          7     b      0
          ```
   
      1. `df.duplicated()`   #实例方法, 无类方法
   
         1. 返回 布尔型Series, 表示当前行 是否与前面的行重复

         2. ```
            0    False
            1    False
            2    False
            3     True
            4    False
            5    False
            6     True
            7    False
            dtype: bool
            ```
   
      2. df.duplicated(['data2'])  #检测列data2是否有重复
   
      3. `df1.drop_duplicates()` 过滤重复行
   
         1. 默认过滤全部列: `df.drop_duplicates()`
         2. 指定 某些列
            1. `df.drop_duplicates(['data1', 'data2'])`  #data1, data2同时不同
            2. df.drop_duplicates('data1')  #data1 列不同
   
   2. `map()` 对Series对象用传入的函数, 对每个元素转换
   
      1. Series根据`map()`传入的函数 对每个元素 转换
         1. `ser = pd.Series(np.random.randint(0, 10, 3))`
         2. `ser.map(lambda x : x ** 2)`
      2. 与 apply() 方法一样
   
   3. applymap() 对DataFrame 对象用传入的函数对每个元素转换
   
      1. DataFrame 对象没有 map属性
   
      2. DataFrame 对象有 apply() 方法
   
         
   
   4. `replace` # 换数据替换, 只有实例方法, 没有类方法
   
      1. 单个值替换单个值
         1. `ser.replace(1, -100)`  #所有的值 `1` 用 `-100` 替换, 生成新表, 原表不变
         2. `df.replace(1, -100)`  #所有的值 `1` 用 `-100` 替换, 生成新表, 原表不变
   
      2. 多个值替换
         1. `ser.replace([7,6], -5)`  #所有的值 `7`, `6`  用`-5` 替换
         2. `df.replace([1,5], -100)`  #所有的值 `1`, `5` 用 `-100` 替换
         3. `df.replace([1, 5], [20, 50])`  #所有的值 `1`, `5` 分别用 `20`, `50` 替换
   
         
   

#### 3.9聚类模型 -- K-Means介绍

1. K-Means : 聚类模型. 聚类, 无监督学习（unsupervised learning）
   2. 无类别标记
2. K-Means算法: 
   1. 参数k: 样本点划分为k个聚类
   3. 同类 样本相似度 高； 不同类 样本相似度 小
3. 算法思想
   1. 初始: k个样本点为中心 聚类， 最靠近的样本点归类
   2. 迭代: 逐步更新 各聚类中心， 达到最好的聚类效果
4. 算法描述
   1. 初始化 k 个聚类的中心
   2. 第n次迭代，任意一个样本点，求其到k个聚类中心的距离，将该 样本点归类到距离最小的中心所在的聚类
   3. 更新各类的中心值 : 均值算法
   4. k个聚类中心，如果利用2,3步的迭代更新后，稳定，则迭代 结束
   5. 小结
      1. 初始化, 2 第n步, 3 更新, 4 结束
5. 优缺点
   1. 优点：速度快，简单
   2. 缺点：
      1. 最终结果和初始点的选择 相关
      2. 局部最优
      3. 要先给定 k 值

#### 3.10 实战案例：全球食品数据分析

1. 项目参考：https://www.kaggle.com/bhouwens/d/openfoodfacts/world-food-facts/how-much-sugar-do-we-eat/discussion

2. 代码

   1. ```
      #-*- coding : utf-8 -*-
      
      #处理zip压缩文件
      import zipfile
      import os
      import pandas as pd
      import matplotlib.pyplot as plt
      ```

   2. ```
      def unzip(zip_filepath, dest_path):
          """ 解压zip文件 """
          with zipfile.ZipFile(zip_filepath) as zf:
              zf.extractall(path=dest_path)
      ```
      
   3. ```
      def get_dataset_filename(zip_filepath):
          """ 获取数据集文件名 """
          with zipfile.ZipFile(zip_filepath) as zf:
              return zf.namelist()[0]
      ```
      
   4. ```
      def run_main():
          """ 主函数 """
          # 声明变量
          dataset_path = './data'  # 数据集路径
          zip_filename = 'open-food-facts.zip'  # zip文件名
          zip_filepath = os.path.join(dataset_path, zip_filename)  # zip文件路径
          dataset_filename = get_dataset_filename(zip_filepath)  # 数据集文件名（在zip中）
          dataset_filepath = os.path.join(dataset_path, dataset_filename)  # 数据集文件路径
      
          print('解压zip...', end='')
          unzip(zip_filepath, dataset_path)
          print('完成.')
      
          # 读取数据
          data = pd.read_csv(dataset_filepath, usecols=['countries_en', 'additives_n'])
      
          #分析各国家食物中的食品添加剂种类个数
          #1. 数据清理
          #1.1 去除缺失数据
          data = data.dropna()    # 或者data.dropna(inplace=True)
      
          #1.2 将国家名称转换为小写
          # 课后练习：经过观察发现'countries_en'中的数值不是单独的国家名称，
          # 有的是多个国家名称用逗号隔开，如 Albania, Belgium, France, Germany, Italy, Netherlands, Spain
          # 正确的统计应该是将这些值拆开成多个行记录，然后进行分组统计
          data['countries_en'] = data['countries_en'].str.lower()
      
          #2. 数据分组统计
          country_additives = data['additives_n'].groupby(data['countries_en']).mean()
      
          #3. 按值从大到小排序
          result = country_additives.sort_values(ascending=False)
      
          #4. pandas可视化top10
          result.iloc[:10].plot.bar()
          plt.show()
      
          #5. 保存处理结果
          result.to_csv('./country_additives.csv')
      
          # 删除解压数据，清理空间
          if os.path.exists(dataset_filepath):
              os.remove(dataset_filepath)
      ```
      
   5. ```
      if __name__ == '__main__':
          run_main()
      ```

### 4 数据可视化工具

1. Matplotlib
2. Seaborn
3. 交互式数据可视化—Bokeh

#### 4.1 Matplotlib绘图

1. Matplotlib :  Python 的 2D 绘图库

   2. `http://matplotlib.org`
   3. `import matplotlib.pyplot as plt`
      1. pyploy 模块含 matplotlib API 函数

2. figure图片对象: Matplotlib 图像位于其中

   1. `fig = plt.figure()`  #创建figure对象

   2. ```
      # 引入matplotlib包
      import matplotlib.pyplot as plt
      import numpy as np
      
      %matplotlib inline #在jupyter notebook 里需要使用这一句命令
      
      # 创建figure对象
      fig = plt.figure() #创建 matplotlib.figure.Figure 对象

3. subplot

   1. fig.add_subplot(a, b, c)  #生成子图, 并添加子图

      1. 参数
         1. a,b  将fig分割成 a*b 个的区域
         2. c  当前选中的区域，
         3. 从 `1` 编号（不是从0开始）
      2. plot (绘图)区域是最后指定 subplot 的位置

   2. ```
      fig = plt.figure() #创建 matplotlib.figure.Figure 对象
      
      #指定 plot 区域
      ax1 = fig.add_subplot(2,2,1)   #添加子图
      ax2 = fig.add_subplot(2,2,2)
      ax3 = fig.add_subplot(2,2,3)
      ax4 = fig.add_subplot(2,2,4)  #最后指定subplot位置是 plot 区域, 在ax4上绘图
      
      #绘图数据
      random_arr = np.random.randn(100)
      
      # 默认在最后一次 subplot 位置上作图
      plt.plot(random_arr)
      
      #显示绘图
      plt.show()
      ```

4. 直方图：hist

   1. ```
      import matplotlib.pyplot as plt
      import numpy as np
      plt.hist(np.random.randn(100), bins=10, color='b', alpha=0.3) 
      plt.show()
      ```
      
      1. 参数 
         1. 一维数组 数据
         2. bins 直方图中 bin(柱子) 的个数
         3. alpha 透明度
         4. color 填充色

5. 散点图：scatter 

   1. ```
      import matplotlib.pyplot as plt
      import numpy as np
      
      # 绘制散点图
      x = np.arange(50)
      y = x + 5 * np.random.rand(50)
      plt.scatter(x, y)  #模块的方法
      ```

      1. 参数: 
         1. x, y. 两个一维数组

6. 柱状图：bar

   1. ```
      x = np.arange(5)
      y1, y2 = np.random.randint(1, 25, size=(2, 5))
      width = 0.25
      ax = plt.subplot(1,1,1)   #创建子图ax对象
      ax.bar(x, y1, width, color='r')
      ax.bar(x+width, y2, width, color='g')
      ax.set_xticks(x+width)
      ax.set_xticklabels(['a', 'b', 'c', 'd', 'e'])  #指定的刻度标签代替默认标签
      plt.show()
      ```

   2. 小结

      1. `ax.set_xticks()` #设置 x 刻度, 只有对象方法, 没有类方法
      
         plt.set_xticks(x+width) 设置刻度报错, 因为 module 'matplotlib.pyplot' has no attribute 'set_xticks' 
      
      2. `ax.set_xticklabels()`  #设定 x 轴标签

7. 混淆矩阵绘图：plt.imshow()

   1. 混淆矩阵, 二维表格中某个值的颜色显示

   2. ```
      import matplotlib.pyplot as plt
      import numpy as np
      
      #矩阵绘图
      #返回<class 'numpy.ndarray'>
      m = np.random.rand(10,10)  #m是 10*10的数组
      print(m)
      plt.imshow(m, interpolation='nearest', cmap=plt.cm.ocean)  #混淆矩阵
      plt.colorbar()  #颜色条
      plt.show()
      ```

8. plt.subplots()  #返回值 `figure` 图片对象 和 `subplot`类型的元素的数组

   1. jupyter 正常显示，推荐 这种方式创建多个图表
      
   2. ```
      fig, subplot_arr = plt.subplots(2,2)   
      # bins 显示个数，一般小于等于数值个数
      subplot_arr[1,0].hist(np.random.randn(100), bins=10, color='b', alpha=0.3)
      plt.show()
      ```

      

9. 颜色、标记、线型

   1. `ax.plot(x, y, linestyle=‘--’, color=‘r’)`  #等价于 ax.plot(x, y, 'r--')

   2. 颜色

      1. | 代号 : 说明                                                  |      |
         | ------------------------------------------------------------ | ---- |
         | b : blue , g : green, r : red, C : cyan, m : magenta, y : yellow, k : black, W : white |      |

   3. 标记

      1. | marker: description                                          |      |
         | ------------------------------------------------------------ | ---- |
         | `”.” : point, ”,”: pixel , “o” : circle , “V” : triangle_down, "^" : triangle_ up , “<" : triangle_ left` |      |

   4. 线形

      1. | linestyle : description                                      |      |
         | ------------------------------------------------------------ | ---- |
         | `'-' or 'solid' : solid line , '--' or 'dashed' : dashed line , '-.' or 'dashdot' : dash-dotted line , ': ' or'dotted ' : dotted line , None : draw nothing, ' ' : draw nothing ,'' : draw nothing` |      |

10. 刻度、标签、图例

    1. 设置方法: 子图实例方法,  图片类方法

       1. 设置 刻度范围
          1. plt.xlim(), plt.ylim()
          2. ax.set_xlim(), ax.set_ylim()
       2. 设置显示的刻度
          1. plt.xticks(), plt.yticks()
          2. ax.set_xticks(), ax.set_yticks()
       3. 设置刻度标签
          1. ax.set_xticklabels(), ax.set_yticklabels()
       4. 设置坐标轴标签
          1. plt.xlabel, plt.ylabel
          1. ax.set_xlabel(), ax.set_ylabel()
       5. 设置标题
          1. plt.title()
          1. ax.set_title()
       6. 图例
          1. ax.plot(label=‘legend’)
          2. ax.legend(),  plt.legend()
          3. loc=‘best’：自动选择 图例最佳位置

       7. ```
          def make_plt(context, host, status):
              #定义y轴展示信息字典
              label1_dict = {'0': 'in_bytes', '1': 'out_bytes', '2': 'all_bytes'}
              label2_dict = {'0': 'in_packets', '1': 'out_packets', '2': 'all_packets'}
              path = MEDIA_ROOT + 'images/' + '0.png'
              #设置横纵坐标的名称以及对应字体格式
              font_xy = {'family': 'Times New Roman', 'weight': 'normal', 'size': 120}
              #生成一个（200， 50）大小的图像
              plt.figure(figsize=(220, 50))
              
              #获得当前图片的坐标对象
              ax = plt.gca()
              
              #将数据填充进图像
              ax.plot(context['time_list'], context['bytes_list'], lw=6, color='blue', label=label1_dict[status])
              ax.set_ylabel(label1_dict[status], fontsize=120)
              
              #设置y轴坐标从0开始
              if context['bytes_list']:
                  plt.ylim((0, max(context['bytes_list'])))
              #设置刻度大小
              plt.tick_params(labelsize=80)
              # 设置注释放置位置
              plt.legend(loc=2, fontsize=100)
              #使用栅格
              plt.grid(color="black", which="both", linestyle=':', linewidth=1)
              plt.fill(color='g', alpha=0.3)
           
              #复制兄弟轴
              ax2 = ax.twinx()
              ax2.plot(context['time_list'], context['packets_list'], lw=6, color='red', label=label2_dict[status])
              ax2.set_ylabel(label2_dict[status], fontsize=120)
              #获得x轴的坐标范围
              start, end = ax.get_xlim()
              # 设置x轴刻度的显示步长
              plt.xticks(np.linspace(start, end, 9))
              # 设置坐标轴名称
              plt.title(host, font_xy)
              #设置刻度大小
              plt.tick_params(labelsize=80)
              #剔除图框上边界和右边界的刻度
              plt.tick_params(top='on', right='on')
              #设置注释放置位置
              plt.legend(loc=0, fontsize=100)

    2. 示例

       1. ```
          import matplotlib.pyplot as plt
          import numpy as np
          
          #创建图片,和一个子图
          fig, ax = plt.subplots(1)
          ax.plot(np.random.randn(1000).cumsum(), label='line0')
          
          #设置刻度范围
          #plt.xlim([0,500])
          ax.set_xlim([0, 800])
          
          #设置显示的刻度
          #plt.xticks([0,500])
          ax.set_xticks(range(0,500,100))
          
          #设置刻度标签
          ax.set_yticklabels(['Jan', 'Feb', 'Mar'])
          
          #设置坐标轴标签
          ax.set_xlabel('Number')
          ax.set_ylabel('Month')
          
          #设置标题
          ax.set_title('Example')
          
          #绘图
          ax.plot(np.random.randn(1000).cumsum(), label='line1')
          ax.plot(np.random.randn(1000).cumsum(), label='line2')
          
          #图例
          ax.legend()
          ax.legend(loc='best')
          #plt.legend()
          ```

#### 4.2 seaborn绘图

1.  seaborn:  对matplotlib 更高级的 API 封装, 做图更容易, 支持 numpy 和 pandas 的数据结构可视化

   2. seaborn 为 matplotlib 的 补充， 不是替代

   3. 可视化线性回归模型中的 独立变量 及 不独立变量

   4. ```
      import numpy as np
      import pandas as pd
      # from scipy import stats
      import matplotlib.pyplot as plt
      
      import seaborn as sns
      # %matplotlib inline
      ```
   
      
   
2. 数据集分布可视化

   1. 单变量分布 sns.distplot()

      1. 单变量分布
         1. `x1 = np.random.normal(size=1000)`
         2. `sns.distplot(x1)`
         
      2. 直方图 sns.distplot(kde=False)
         1. `sns.distplot(x1, bins=20, kde=False, rug=True)`
            
            ![img](file:///./pic/sns直方图.png)
         
      3. 核密度估计 sns.distplot(hist=False) 或 sns.kdeplot()
         1. `sns.distplot(x1, hist=False, rug=True)`
            
            ![img](file:///./pic/sns核密度估计.png)
   
   2. 双变量分布
   
      1. 双变量分布
         1. `df1 = pd.DataFrame({"x": np.random.randn(500), "y": np.random.randn(500)})`
         2. `df2 = pd.DataFrame({"x": np.random.randn(500),"y": np.random.randint(0, 100, 500)})`
         
      2. 散布图 sns.jointplot()
         1. `sns.jointplot(data=df1, x="x", y="y")`
            
            ![img](./pic/sns散布图.png)
         
      3. 二维直方图 Hexbin,   sns.jointplot(kind=‘hex’)
         1. `sns.jointplot(data=df1, x="x", y="y", kind="hex")`
            
            ![img](./pic/sns二维直方图.png)
         
      4. 核密度估计 sns.jointplot(kind=‘kde’)
         1. `sns.jointplot(data=df1, x="x", y="y", kind="kde");`
            
            ![img](./pic/sns_双变量_核密度估计.png)
   
   3. 数据集中 变量间关系可视化 sns.pairplot()
   
      1. 数据集
   
         1. `dataset = sns.load_dataset("tips")`

            1. ```
                    total_bill   tip     sex smoker   day    time  size
               0         16.99  1.01  Female     No   Sun  Dinner     2
               1         10.34  1.66    Male     No   Sun  Dinner     3
               2         21.01  3.50    Male     No   Sun  Dinner     3
               ..          ...   ...     ...    ...   ...     ...   ...
               243       18.78  3.00  Female     No  Thur  Dinner     2
               [244 rows x 7 columns]
               ```
   
      2. 数据集中变量间关系可视化

         1. sns.pairplot(dataset)
         
            ![img](./pic/sns_数据集_变量.png)
         
            
      
   4. 类别数据可视化
   
      1. 数据
   
         1. `dataset = sns.load_dataset('exercise')`
   
            1. ```
                   Unnamed: 0  id     diet  pulse    time     kind
               0            0   1  low fat     85   1 min     rest
               1            1   1  low fat     85  15 min     rest
               2            2   1  low fat     88  30 min     rest
               ..         ...  ..      ...    ...     ...      ...
               88          88  30   no fat    111  15 min  running
               89          89  30   no fat    150  30 min  running
               [90 rows x 6 columns]
               ```
   
      2. 类别内 散布图 之 散点图 : sns.stripplot(), sns.swarmplot()
   
         1. `sns.stripplot(x="diet", y="pulse", data=dataset)`  #sns.stripplot() 数据点会 重叠
            
            ![img](./pic/sns_类别_散布图.png)
            
         2. `sns.swarmplot(x="diet", y="pulse", data=dataset, hue='kind')`  #sns.swarmplot() 数据点避免重叠，hue指定子类别
            
            ![img](./pic/sns_类别_散布图_swarmplot.png)
      
      3. 类别内 数据分布 之 盒子图 sns.boxplot() , 小提琴图 sns.violinplot()
      
         1. `sns.boxplot(x="diet", y="pulse", data=dataset)`  #盒子图 sns.boxplot(), hue指定子类别
            
            ![img](./pic/sns_类别_盒子图.png)
            
         2. `sns.violinplot(x="diet", y="pulse", data=dataset, hue='kind')`  #小提琴图 sns.violinplot(), hue指定子类别
            
            ![img](./pic/sns_类别_小提琴图.png)
      
      4. 类别内 统计图 之 柱状图 sns.barplot() , 点图 sns.pointplot()
      
         1. `sns.barplot(x="diet", y="pulse", data=dataset, hue='kind')`    #柱状图 sns.barplot()
            
            ![img](./pic/sns_类别_柱状图.png)
            
         2. `sns.pointplot(x="diet", y="pulse", data=dataset, hue='kind')`   #点图 sns.pointplot()
            
            ![img](./pic/sns_类别_点图.png)
      
      5. heatmap热度图
      
         1. heatmap热度图，seaborn中常用的图
      
            重要点思维：拿到一批数据一般会求特征之间的相关系数，可以用pands直接求出来相关系数，放到heatmap，
      
            可以很清楚的看到两个特征的相关程度，这是一个固定的数据思维
      
         2. sns.heatmap(diabetes.corr(), annot=True, cmap="YlGnBu");
            plt.savefig("./heatmap.png")
      
            

#### 4.3 Bokeh绘图

​	* `http://liyangbit.com/pythonvisualization/bokeh-glyphs-1st/`

 1. Bokeh :  Web浏览器的交互式、可视化Python绘图库
       1. 支持Python (或Scala, R, Julia…), 不用Javascript
       2. 处理大量、动态或数据流.  做像 D3.js 简洁漂亮的交互可视化效果， 难度低于D3.js
       1. 独立的 HTML文档 或 服务端程序
            	

 2. bokeh 绘图有两类主要的接口  (Bokeh Interface)， bokeh.plotting 和 bokeh.models 
       1.  plotting 是高级接口. 一般情况用 bokeh.plotting

 3. bokeh 绘制图表的常规步骤

      1. 加载 bokeh 库，声明在 notebook 或 html 文件中显示或输出绘制的图表
      2. output_notebook()  #输出为html , output_file(r'') #输出到文件
          1. `from bokeh.plotting import figure, output_file, show`
          2. `output_file(r"C:\Users\Lenovo\Desktop\pandas数据分析\fig_obj5846.html")` 

      3. 创建图表框架 `figure()`,  
      4. 在 figure 上绘制具体的`图形`，比如 circle，line，bar等
      5. 显示图片，show()

 4. circle()

    1. 参数
       1. alpha，数值越小，透明度越高
      2. angle,   对于 circle 而言，angle参数似乎没有用……（因为本身就是圆的，旋转角度没有影响）
      3. fill_color, fill_alpha, 这两个参数用来设置圆体的填充颜色和透明度
      4. line_alpha , 线条透明度
      5. line_color, 线条颜色
      6. line_cap ,似乎没有效果， line_cap 的值可以是 (butt, round, square)
      7. line_width, 圆圈边缘线条的宽度




   1. 绘制图表框架 figure()，设置图表大小（plot_wihth,plot_height），设置图表框架旁的工具(tools), 工具的 value : "crosshair, pan, wheel_zoom, box_zoom, reset, box_select, lasso_select, save”

   3. ```
      from bokeh.plotting import output_notebook, figure, show
      import pandas as pd
      import numpy as np
      
      output_notebook()
      tools="pan,box_zoom,wheel_zoom,reset,save"
      #工具可以是以下 value，可以根据实际情况来选择合作的工具使用
      tools = "crosshair,pan,wheel_zoom,box_zoom,reset,box_select,lasso_select,save"
      
      p = figure(plot_width=400, plot_height = 400, tools=tools)
      
      p.circle([1,2,3,4],[5,6,7,8],size=20, color='red', alpha=0.5)
      
      show(p)

   

 5. figure 详细解读: figure() 的基本用法

    1. ```
       #加载bokeh库
       from bokeh.plotting import figure, output_notebook, show
          
       output_notebook()
       
       
       #1. plot_width, plot_weight  设置绘图区 宽度和高度
       p = figure(plot_width=400, plot_height = 400)
       
       p.circle([1,2,3,4],[5,6,7,8],size=20, color='red', alpha=0.5)
       
       show(p)
       
       #2. width, weight  设置绘图区的宽度和高度. 
       #官方文档，用 plot_width 和 plot_weight 两个参数. 实际上 width 和 height 同样的效果
       
       p = figure(width=400, height = 400)
       
       p.circle([1,2,3,4],[5,6,7,8],size=20, color='red', alpha=0.5)
       
       show(p)
       
       
       #3. tools工具的 value 是“crosshair, pan, wheel_zoom, box_zoom, reset, box_select, lasso_select, save,help”，可选择工具。
       
       tools = "crosshair,pan,wheel_zoom,reset,save"
       
       p = figure(width=400, height = 400, tools=tools)
       
       p.circle([1,2,3,4],[5,6,7,8],size=20, color='red', alpha=0.5)
       
       show(p)
       
       #4. toolbar_location  工具栏位置，参数值： “below, above, left, right”，默认值 “auto”
       
       tools = "crosshair, pan, wheel_zoom, reset"
       p = figure(width=400, height = 400, tools=tools, toolbar_location='above')
       p.circle([1,2,3,4],[5,6,7,8],size=20, color='red', alpha=0.5)
       show(p)
       
       
       #5. x_minor_ticks, y_minor_ticks 默认值是 “auto”，其他值可以是大于1的整数
       
       p = figure(width=400, height = 400,x_minor_ticks=2, y_minor_ticks=10)
       
       p.circle([1,2,3,4],[5,6,7,8],size=20, color='red', alpha=0.5)
       
       show(p)
       
       
       #6. x_range, y_range, 是 list 或 tuple 表示范围的形式数值
       
       p = figure(width=400, height = 400, x_range=[2,4], y_range=[5.5, 7.5])
       
       p.circle([1,2,3,4],[5,6,7,8],size=20, color='red', alpha=0.5)
       
       show(p)
       
       
       #7. x_axis_label, x_axis_location
       #7.1. x_axis_label, 设置 x 轴 的名称
       #7.2. x_axis_location, 设置 x 轴 的位置.  两个值，在上面（”above”）  在下面 （”below”），默认值是 “below”
       p = figure(width=400, height = 400,
                     x_axis_label='x label above', x_axis_location='above',
                     y_axis_label='y label')
                     
       #x_axis_location, default value: "below", values are [ above, below]
       p.circle([1,2,3,4],[5,6,7,8],size=20, color='red', alpha=0.5)
       
       show(p)
       
       #8. y_axis_label, y_axis_location
       #8.1. y_axis_label, 设置 y 轴 的名称
       #8.2. y_axis_location, 设置 y 轴 的位置，有两个值，在左边显示（”left”） 和在右边显示（”right”），默认值是 “left”
       
       #9. x_aixs_type，y_axis_type
       #9.1. x_aixs_type: “datetime”
       #9.2. 若是时间数据，可以将 x 轴或者 y 轴的数值设置为 日期形式
       import pandas as pd
       rng = pd.date_range('2018-1-1', periods=7)
       rng
       ```
       
    2. ```
       x = [1.0, 1.5, 2.0, 2.5, 3.0, 3.5, 4.0]
       # y = [10**(i**2) for i in x]
       y = [i**2 for i in x]
       
       p = figure(width=400, height = 400, title='datetime type example',
       x_axis_label='x label datetime', x_axis_location='below',
       y_axis_label='y label', y_axis_location='left',
       y_axis_type='linear', x_axis_type='datetime')
       # y_axis_type, "auto" 和 "linear" 的效果是一样的
       
       p.line(rng,y,color='red',line_width=3, alpha=0.9)
       
       show(p)
       ```

    3. y_aixs_type: “log” ,  将 x 或 y 轴的数据设置成 对数（log）形式

       ```
       x = [1.0, 1.5, 2.0, 2.5, 3.0, 3.5, 4.0]
       y = [10**(i**2) for i in x]
       
       p = figure(width=400, height = 400, title='log type example',
       x_axis_label='x label', x_axis_location='below',
       y_axis_label='y label log', y_axis_location='left',
       y_axis_type='log')
       
       p.line(x,y,color='red',line_width=3, alpha=0.9)
       
       show(p)
       ```

    4. x_axis_type, y_aixs_type: “mercator”

       ```
       k = [1.0, 1.5, 2.0, 2.5, 3.0, 3.5, 4.0]
       x = [i*10000 for i in k]
       y = [i*10000+30000 for i in k]
       
       #mercator 好像代表的是 数值 10万，简写为 1
       p = figure(width=400, height = 400, title='mercator type example',
       x_axis_label='x label', x_axis_location='below',
       y_axis_label='y label', y_axis_location='left',
       x_axis_type='mercator', y_axis_type='mercator')
       
       p.line(x,y,color='red',line_width=3, alpha=0.9)
       
       show(p)

    

    5. 小结: figure 的部分参数：

       1. plot_width, plot_weight 或 width, height，设置绘图区的长度和宽度
       2. tools，设置绘图区旁边所用的工具(tools)，其值可以是 “crosshair,pan,wheel_zoom,box_zoom,reset,box_select,lasso_select,save,help”
       3. toolbar_location， 设置工具栏显示的位置，参数值可以是： “below, above, left, right”，默认值 “right”
       4. x_minor_ticks, y_minor_ticks 默认值是 “auto”，其他值可以是大于1的整数
       5. x_range, y_range 可以是 list 或 tuple 表示范围的形式数值
       6. x_axis_label, y_axis_label, 设置 x 轴 和 y 轴 的名称
       7. x_axis_location, default value: “below”, values are [ above, below]
       8. y_axis_location, default value: “left”, values are [ left, right]
       9. x_aixs_type，y_axis_type， x 和 y 轴数据的表现形式，其值可以是 “ linear, log, datetime, mercator”, 默认是 “auto”

    

 6. 29种Bokeh基础可视化图形

     1. 加载bokeh库， 准备基础数据

         1. ```
            from bokeh.plotting import figure, output_notebook, show
            from bokeh.layouts import gridplot
            import numpy as np
            
            output_notebook()
            
            np.random.seed(15)
            
            x=np.random.randint(1, 20, size=6)
            y=np.random.randint(20, 50, size=6)
            
            print(x)  #[ 9 13  6  1  8 12]
            print(y)  #[41 42 35 49 37 33]
            
            #circle, circle_cross, circle_x, cross
            p1 = figure(title='circle')
            p1.circle(x, y, size=20, color='#0071c1')
            
            p2 = figure(title='circle_cross')
            p2.circle_cross(x, y, size=20, color='#0071c1', fill_alpha=0.2, line_width=2)
            
            p3 = figure(title='circle_x')
            p3.circle_x(x, y, size=20, color='#0071c1', fill_alpha=0.2, line_width=2)
            
            p4 = figure(title='cross')
            p4.cross(x,y,size=20, color='#0071c1', line_width=2)
            
            grid=gridplot([p1,p2,p3,p4], ncols=2, plot_width=300, plot_height=300)
            
            show(grid)
            ```
            
         2. ![img](./pic/bokeh_circle.png)

     2. diamond, diamond_cross, asterisk, x

        1. ```
           p1 = figure(title='diamond')
           p1.diamond(x,y,size=20, color='#0071c1')
           
           p2 = figure(title='diamond_cross')
           p2.diamond_cross(x,y,size=20, color='#0071c1',fill_alpha=0.2, line_width=2)
           
           p3 = figure(title='asterisk')
           p3.asterisk(x,y,size=20, color='#0071c1',fill_alpha=0.2, line_width=2)
           
           p4 = figure(title='x')
           p4.x(x,y,size=20, color='#0071c1', line_width=2)
           
           
           grid=gridplot([p1,p2,p3,p4],ncols=2, plot_width=300,plot_height=300)
           
           show(grid)
           ```

     3. square, square_cross, square_x

        1. ```
           p1 = figure(title='square')
           p1.square(x,y,size=20, color='#0071c1')
           p2 = figure(title='square_cross')
           p2.square_cross(x,y,size=20, color='#0071c1',fill_alpha=0.2, line_width=2)
           
           p3 = figure(title='square_x')
           p3.square_x(x,y,size=20, color='#0071c1',fill_alpha=0.2, line_width=2)
           
           
           grid=gridplot([p1,p2,p3],ncols=2, plot_width=300,plot_height=300)
           
           show(grid)
           ```

     4. triangle, inverted_triangle

        1. ```
           p3 = figure(title='triangle')
           p3.triangle(x,y,size=20, color='#0071c1', line_width=2)
           
           p4 = figure(title='inverted_triangle')
           p4.inverted_triangle(x,y,size=20, color='#0071c1', line_width=2)
           
           grid=gridplot([p3,p4],ncols=2, plot_width=300,plot_height=300)
           show(grid)
           ```

     5. annulus, ellipse, wedge, oval

        1. annulus, 绘制圆环;  wedge, 楔形图;  ellipse, 绘制椭圆;  oval，绘制椭圆

        1. ```
           p1 = figure(title='annulus')
           p1.annulus(x,y,color='#0071c1', inner_radius=0.8, outer_radius=1.2)
           p2 = figure(title='wedge')
           p2.wedge(x,y,color='#0071c1', radius=0.8, start_angle=0.5, end_angle=4.5, direction='anticlock')
           
           p3 = figure(title='ellipse')
           p3.ellipse(x,y,color='#0071c1', width=2, height=3.6, angle=30)
           
           p4 = figure(title='oval')
           p4.oval(x,y,color='#0071c1', width=2, height=3.6, angle=30)
           
           
           grid=gridplot([p1,p2,p3,p4],ncols=2, plot_width=300,plot_height=300)
           show(grid)
           ```

     6. hbar, vbar

        1. 绘制柱状图，分为水平柱状图和垂直柱状图，这个也是平时我们用的比较多的类型之一

        2. ```
           p2 = figure(title='hbar')
           p2.hbar(y=y, height=0.3, left=0, right=x, color='#0071c1')
           
           p3 = figure(title='vbar')
           p3.vbar(x=x, width=0.3, bottom=0, top=y, color='#0071c1')
           
           grid=gridplot([p2,p3],ncols=2, plot_width=300,plot_height=300)
           
           show(grid)
           ```

     7. line, multi_line  : 线状图或者称为折线图

        1. ```
           p1 = figure(title='line')
           p1.line(x,y,color='#0071c1', line_width=2)
           p1.circle(x,y,size=10,color='#0071c1',fill_color='white')
           
           from bokeh.plotting import figure, output_file, show
           
           p2 = figure(title='multi_line')
           p2.multi_line(xs=[[1, 2, 3], [2, 3, 4]], ys=[[6, 7, 2], [4, 5, 7]],
           color=['red','green'])
           
           grid=gridplot([p1,p2],ncols=2, plot_width=300,plot_height=300)
           
           show(grid)
           ```

     8. patch, patches  #中文翻译不确定如何解释为好， 且称之为 “块状图”

        1. ```
           #patch(x, y, **kwargs)
           #patches(xs, ys, **kwargs)
           
           
           p1 = figure(title='patch')
           p1.patch(x,y,color='#0071c1')
           p2 = figure(title='patches')
           p2.patches(xs=[[1, 2, 3, 4], [2, 3, 4]], ys=[[6, 7, 2, 3], [4, 5, 7]],
           color=['red','green'])
           
           grid=gridplot([p1,p2],ncols=2, plot_width=300,plot_height=300)
           
           show(grid)
           ```

     9. quad, quadratic

        1. ```
           #quad(left, right, top, bottom, **kwargs)，四方的
           #quadratic(x0, y0, x1, y1, cx, cy, **kwargs)， 二次方程的
           ```

     10. ray, rect, segment, step

         1. ray(x, y, length, angle, **kwargs) ， 射线
         2. rect(x, y, width, height, angle=0.0, dilate=False, **kwargs) ， 矩形
         3. segment(x0, y0, x1, y1, **kwargs) ， 分段，段落



#### 4.4 实战案例：世界高峰数据可视化

 1. 参考 `https://www.kaggle.com/alex64/d/abcsds/highest-mountains/let-s-climb`

 2. ```
    import pandas as pd
    import matplotlib.pyplot as plt
    from matplotlib import style
    
    style.use('ggplot')     # 设置图片显示的主题样式
    
    # 解决matplotlib显示中文问题
    plt.rcParams['font.sans-serif'] = ['SimHei']  # 指定默认字体
    plt.rcParams['axes.unicode_minus'] = False  # 解决保存图像是负号'-'显示为方块的问题
    
    dataset_path = './dataset/Mountains.csv'
    
    
    def preview_data(data):
        """ 数据预览 """
        # 数据预览
        print(data.head())
    
        # 数据信息
        print(data.info())
    
    
    def proc_success(val):
        """ 处理 'Ascents bef. 2004' 列中的数据 """
        if '>' in str(val):
            return 200
        elif 'Many' in str(val):
            return 160
        else:
            return val
    
    
    def run_main():
        """ 主函数 """
        data = pd.read_csv(dataset_path)
    
        preview_data(data)
    
        #数据重构
        #重命名列名
        data.rename(columns={'Height (m)': 'Height', 'Ascents bef.2004': 'Success', 'Failed attempts bef.2004': 'Failed'}, inplace=True)
    
        # 数据清洗
        data['Failed'] = data['Failed'].fillna(0).astype(int)
        data['Success'] = data['Success'].apply(proc_success)
        data['Success'] = data['Success'].fillna(0).astype(int)
        data = data[data['First ascent'] != 'unclimbed']
        data['First ascent'] = data['First ascent'].astype(int)
    
        #可视化数据
        #1. 登顶次数 vs 年份
    
        plt.hist(data['First ascent'].astype(int), bins=20)
        plt.ylabel('高峰数量')
        plt.xlabel('年份')
        plt.title('登顶次数')
        plt.savefig('./first_ascent_vs_year.png')
        plt.show()
    
        #2. 高峰vs海拔
        data['Height'].plot.hist(color='steelblue', bins=20)
        plt.bar(data['Height'],
                (data['Height'] - data['Height'].min()) / (data['Height'].max() - data['Height'].min()) * 23,   # 按比例缩放
                color='red',
                width=30, alpha=0.2)
        plt.ylabel('高峰数量')
        plt.xlabel('海拔')
        plt.text(8750, 20, "海拔", color='red')
        plt.title('高峰vs海拔')
        plt.savefig('./mountain_vs_height.png')
        plt.show()
    
        #3. 首次登顶
        data['Attempts'] = data['Failed'] + data['Success']  # 攀登尝试次数
        fig = plt.figure(figsize=(13, 7))
        fig.add_subplot(211)
        plt.scatter(data['First ascent'], data['Height'], c=data['Attempts'], alpha=0.8, s=50)
        plt.ylabel('海拔')
        plt.xlabel('登顶')
    
        fig.add_subplot(212)
        plt.scatter(data['First ascent'], data['Rank'].max() - data['Rank'], c=data['Attempts'], alpha=0.8, s=50)
        plt.ylabel('排名')
        plt.xlabel('登顶')
        plt.savefig('./mountain_vs_attempts.png')
        plt.show()
    
        # 课后练习，尝试使用seaborn或者bokeh重现上述显示的结果
    
    if __name__ == '__main__':
        run_main()
    ```

### 5 自然语言处理NLTK



#### 5.1 NLTK与自然语言处理基础

1. NLTK (Natural Language Toolkit)

   1. NLP 常用的一个Python库, 处理英文。文本分析工具
   2. NLTK配套有: 文档, 语料库, 书籍。

2. 安装步骤

   1. `pip install nltk`  #下载NLTK包

   2. 运行Python， 输入指令

      1. ```
         import nltk
         nltk.download()
         ```

      2. 安装位置: `C:\Users\Administrator\AppData\Roaming\nltk_data`

   3. 测试

      1. from nltk.corpus import brown
      2. brown.readme()
      3. brown.words()[0:5]
      4. len(brown.words())

3. nltk.corpus  :  语料库

   1. ```
      import nltk
      from nltk.corpus import brown # 需要下载brown语料库
      # 引用布朗大学的语料库
      
      # 查看语料库包含的类别
      print(brown.categories())
      
      # 查看brown语料库
      print('共有{}个句子'.format(len(brown.sents())))
      print('共有{}个单词'.format(len(brown.words())))
      ```
   
4. 分词 (tokenize)

   1. 句子拆分为有意义的词. 中、英文分词: 英文中， 空格 自然分界符 ; 中文中, 没有形式上的分界符，分词复杂

   2.  jieba  #中文分词

      1.  `pip install jieba `   , 结巴分词. 中文分词工具
   
   2. 除了分词, 中英文的后续处理没有太大区别
   
   3. ```
         # 导入jieba分词
      import jieba
         
      seg_list = jieba.cut("欢迎来到黑马程序员Python学科", cut_all=True)
         print("全模式: " + "/ ".join(seg_list))  # 全模式
         
         seg_list = jieba.cut("欢迎来到黑马程序员Python学科", cut_all=False)
         print("精确模式: " + "/ ".join(seg_list))  # 精确模式
      ```
   
         1. generator 可用 `''.join(generator对象)` 打印输出
            1. 输出一次后, 里面的内容就没有了, 生成器就边空了
   
   3. nltk.word_tokenize()  #英文分词

5. 词形问题

   1. 词形问题 : 

      1. 如: look, looked, looking 的词干 是 `look`
      2. 影响语料 学习的准确度
      3. 词形 归一化

   2. 词形问题处理方法:  4种

      1. stem() , 词干提取(stemming) ,  如将ing, ed去掉，只留单词主干

         1. NLTK中的stemmer:  PorterStemmer,  SnowballStemmer,  LancasterStemmer

         2. PorterStemmer
   
            1. ```
               from nltk.stem.porter import PorterStemmer
               
               porter_stemmer = PorterStemmer()
               porter_stemmer.stem('looked')
               porter_stemmer.stem('looking')
               
               # 运行结果：
               # look
               # look
               ```

         3. SnowballStemmer
   
            1. ```
               from nltk.stem import SnowballStemmer
               
               snowball_stemmer = SnowballStemmer('english')
               snowball_stemmer.stem('looked')
               snowball_stemmer.stem('looking')
               
               # 运行结果：
               # look
               # look
               ```

         4. LancasterStemmer
   
            1. ```
               from nltk.stem.lancaster import LancasterStemmer
               
               lancaster_stemmer = LancasterStemmer()
               lancaster_stemmer.stem('looked')
               lancaster_stemmer.stem('looking')
               
               # 运行结果：
               # look
               # look
               ```

      2. lemmatize() , 词形归类(lemmatization)

         1. 各种词形归类成一种形式，如 `am, is, are -> be` ,  `went->go`
            1. NLTK中的lemma : WordNetLemmatizer
            2. 参数pos 指明词性 更准确地 lemma
   
         2. 示例
   
            1. ```
               from nltk.stem import WordNetLemmatizer 
               # 需要下载wordnet语料库
               
               wordnet_lematizer = WordNetLemmatizer()
               wordnet_lematizer.lemmatize('cats')
               wordnet_lematizer.lemmatize('boxes')
               wordnet_lematizer.lemmatize('are')
               wordnet_lematizer.lemmatize('went')
               
               # 运行结果：
               # cat
               # box
               # are
               # went
               ```
   
         3. 指明词性可以更准确地进行lemma
   
            1. ```
               wordnet_lematizer.lemmatize('are', pos='v')
               wordnet_lematizer.lemmatize('went', pos='v')
               
               # 运行结果：
               # be
               # go
               ```
   
      3. nltk.word_tokenize() ,  NLTK 词性标注 (Part-Of-Speech)
   
         1. 示例

            1. ```
               import nltk
               
               words = nltk.word_tokenize('Python is a widely used programming language.')  #需要 tokenizers/punkt
               
               nltk.pos_tag(words) # 需要 averaged_perceptron_tagger
               
               # 运行结果：
               # [('Python', 'NNP'), ('is', 'VBZ'), ('a', 'DT'), ('widely', 'RB'), ('used', 'VBN'), ('programming', 'NN'), ('language', 'NN'), ('.', '.')]
               ```
   
               1. `tokenizers/punkt` 放到 `C:\Users\Administrator\AppData\Roaming\nltk_data\`
               2. `taggers/averaged_perceptron_tagger` 放到 `C:\Users\Administrator\AppData\Roaming\nltk_data\`
   
      4. `stopwords.words()`  ,  NLTK的停用词
   
         1. 节省存储空间, 提高搜索效率
   
         2. 停用词表 : 人工输入、非自动化生成的
   
         3. 停用词分类

            1. 英文停用词
               1. 功能词，如the, is…
               2. 词汇词， 使用广泛的词，如want
            2. 中文停用词
               1. 中文停用词库
               2. 哈工大停用词表
               3. 四川大学机器智能实验室停用词库
               4. 百度停用词列表
            3. 其他语言停用词表
               1. `http://www.ranks.nl/stopwords`
   
         5. 示例
   
            1. ```
               from nltk.corpus import stopwords # 需要下载stopwords
               
               filtered_words = [word for word in words if word not in stopwords.words('english')]  #去掉英文类型停用词
               print('原始词：', words)
               print('去除停用词后：', filtered_words)
               
               # 运行结果：
               # 原始词： ['Python', 'is', 'a', 'widely', 'used', 'programming', 'language', '.']
               # 去除停用词后： ['Python', 'widely', 'used', 'programming', 'language', '.']
               ```

               1. `corpora/stopwords` 放入 `C:\Users\Administrator\AppData\Roaming\nltk_data\corpora`
   
   3. 典型的 文本预处理流程
   
      1. 文本预处理流程
   
         1. 原始文本
         2. 分词
         3. 词形归一化
         4. 去掉停用词
   
      2. 示例
   
         1. ```
            import nltk
            from nltk.stem import WordNetLemmatizer
            from nltk.corpus import stopwords
            
            # 原始文本
            raw_text = 'Life is like a box of chocolates. You never know what you\'re gonna get.'
            
            # 分词
            raw_words = nltk.word_tokenize(raw_text)
            
            # 词形归一化
            wordnet_lematizer = WordNetLemmatizer() # 词形归一化对象
            words = [wordnet_lematizer.lemmatize(raw_word) for raw_word in raw_words]
            
            # 去除停用词
            filtered_words = [word for word in words if word not in stopwords.words('english')]  #.. not in ..
            
            print('原始文本：', raw_text)
            print('预处理结果：', filtered_words)
            
            #输出结果
            #原始文本： Life is like a box of chocolates. You never know what you're gonna get.
            #预处理结果： ['Life', 'like', 'box', 'chocolate', '.', 'You', 'never', 'know', "'re", 'gon', 'na', 'get', '.']
            ```
   
   4. 案例
   
      1. 流程
   
         1. 导包
         2. 分句:  文档分为句子,  文档对象.tokenize(paragraph)
         3. 分词: WordPunctTokenizer.tokenize(文档)
   
      2. 示例
   
         1.  分句

            1. ```
               import nltk
               from nltk.tokenize import WordPunctTokenizer
               
               sent_tokenizer = nltk.data.load('tokenizers/punkt/english.pickle')  
               paragraph = "The first time I heard that song was in Hawaii on radio.  I was just a kid, and loved it very much! What a fantastic song!"  
               
               # 分句
               sentences = sent_tokenizer.tokenize(paragraph) #对象.方法(对象)
               print(sentences)
               
               #输出结果
               #['The first time I heard that song was in Hawaii on radio.','I was just a kid, and loved it very much!','What a fantastic song!']
               ```
   
         2. 分词
   
            1. ```
               sentence = "Are you old enough to remember Michael Jackson attending. the Grammys with Brooke Shields and Webster sat on his lap during the show?"  
               
               # 分词
               words = WordPunctTokenizer().tokenize(sentence.lower())  #对象.方法(对象)
               print(words)
               
               #输出结果
               #['are', 'you', 'old', 'enough', 'to', 'remember', 'michael', 'jackson', 'attending', '.', 'the', 'grammys', 'with', 'brooke', 'shields', 'and', 'webster', 'sat', 'on', 'his', 'lap', 'during', 'the', 'show', '?']
               
               ```

               

#### 5.2 jieba分词

1. jieba : python写成的, 工业界的分词开源库

   1. github地址为：[https://github.com/fxsjy/jieba](https://github.com/fxsjy/jieba])

   2. Python里的安装方式： `pip install jieba`

   3. 分词模式: 全模式, 精确模式(默认), 索引引擎模式

   4. ```
      import jieba as jb
      
      seg_list = jb.cut("我来到北京清华大学", cut_all=True)
      "/ ".join(seg_list) # 全模式, 我/ 来到/ 北京/ 清华/ 清华大学/ 华大/ 大学'
      
      seg_list = jb.cut("我来到北京清华大学", cut_all=False)
      "/ ".join(seg_list) # 精确模式, '我/ 来到/ 北京/ 清华大学'
      
      seg_list = jb.cut("他来到了网易杭研大厦")  
      "/ ".join(seg_list)  # 默认 精确模式, 他/ 来到/ 了/ 网易/ 杭研/ 大厦'
      
      seg_list = jb.cut_for_search("小明硕士毕业于中国科学院计算所，后在日本京都大学深造")  
      "/ ".join(seg_list) # 搜索引擎模式, '小明/ 硕士/ 毕业/ 于/ 中国/ 科学/ 学院/ 科学院/ 中国科学院/ 计算/ 计算所/ ，/ 后/ 在/ 日本/ 京都/ 大学/ 日本京都大学/ 深造'
      
      ```

      

2. jieba 的基本思路: jieba 对已收录词和未收录词 有相应的算法

   2. 处理思路
      1. 加载 `词典dict.txt`
      2. 用已有的 词典 构建句子的DAG（有向无环图）
      3. 词典未收录词, 用HMM模型的 viterbi 算法 分词处理
      4. 已收录词和未收录词 分词完毕后， 用 dp 寻找 DAG 的最大概率路径, 输出分词结果
   
3. 案例：

   1. ```
      #!/usr/bin/env python
      # -*- coding:utf-8 -*-
      
      import jieba
      import requests
      from bs4 import BeautifulSoup
      
      def extract_text(url):
          #发送url请求,获取响应文件
          page_source = requests.get(url).content
          bs_source = BeautifulSoup(page_source, "lxml")
      
          #解析出所有的p标签(含p)
          report_text = bs_source.find_all('p')
      
          text = ''
          # 将p标签里的所有内容都保存到一个字符串里
          for p in report_text:
              text += p.get_text()
              text += '\n'
      	#返回提取的文档
          return text
      
      def word_frequency(text):
          from collections import Counter
          #jieba分词. 创建长度大于等于2 的词的列表
          words = [word for word in jieba.cut(text, cut_all=True) if len(word) >= 2]
      
          #Counter 简单的计数器，统计字符出现的个数, 将单词列表将被转化为字典
          c = Counter(words)
      
          for word_freq in c.most_common(10):
              word, freq = word_freq
              print(word, freq)
      
      if __name__ == "__main__":
          url = 'http://www.gov.cn/premier/2017-03/16/content_5177940.htm'
          text = extract_text(url)
          word_frequency(text)
      ```
   
   2. 结果
   
      ```
      发展 134
      改革 85
      经济 71
      推进 66
      建设 59
      社会 49
      人民 47
      企业 46
      加强 46
      政策 46
      ```
   
   3. 流程介绍
   
      1. 首先， 从网上抓取政府工作报告的全文。
      2. 然后，用 jieba 分词。jieba的全模式分词，句子中所有的词语都扫描出来, 精确模式下，返回的词频数据不准确。
      3. 分词时，添加一个len(word) >= 2的条件即可。
      4. 最后，用Counter类，单词列表 转化为 字典，键值 是键的出现次数

#### 5.3 情感分析

1. 自然语言处理(NLP) :自然语言（文本） 转化为 计算机程序能理解的形式

   1. 预处理: 字符串 向量化
   2. NLP 应用 : 
      1. 情感分析
      2. 文本相似度
      3. 文本分类
   
2. 简单的情感分析

   1. 情感字典（sentiment dictionary）
      1. 人工构造 字典:  `like` -> 1, `good` -> 2, `bad` -> -1, `terrible`-> -2
      2. 关键词 匹配
   2. AFINN 针对价数的英语单词列表
      1. ` http://www2.imm.dtu.dk/pubdb/views/publication_details.php?id=6010`
      2. 简单粗暴，但很实用
   3. 问题
      1. 新词，特殊词等，扩展性较差

3. 机器学习模型，`nltk.classify`

   1. 案例： 用机器学习实现

   2. ```
      import nltk
      from nltk.stem import WordNetLemmatizer
      from nltk.corpus import stopwords
      from nltk.classify import NaiveBayesClassifier
      
      text1 = 'I like the movie so much!'
      text2 = 'That is a good movie.'
      text3 = 'This is a great one.'
      text4 = 'That is a really bad movie.'
      text5 = 'This is a terrible movie.'
      
      def proc_text(text):
          """ 预处处理文本 """
          #分词
          raw_words = nltk.word_tokenize(text)
      
          #词形归一化
          wordnet_lematizer = WordNetLemmatizer()    
          words = [wordnet_lematizer.lemmatize(raw_word) for raw_word in raw_words]
      
          #去除停用词
          filtered_words = [word for word in words if word not in stopwords.words('english')]
      
          #True 表示该词在文本中，为了使用nltk中的分类器
          return {word: True for word in filtered_words}
             
      
      # 构造训练样本
      train_data = [[proc_text(text1), 1],
                    [proc_text(text2), 1],
                    [proc_text(text3), 1],
                    [proc_text(text4), 0],
                    [proc_text(text5), 0]]
      
      #用 NaiveBayesClassifier分类器 训练得模型
      nb_model = NaiveBayesClassifier.train(train_data)
      
      # 测试模型
      text6 = 'That is a bad one.'
      nb_model.classify(proc_text(text6))
      ```
      
   3. proc_text(text)返回的 值

      1. ```
         [[{'I': True, 'like': True, 'movie': True, 'much': True, '!': True}, 1],
          [{'That': True, 'good': True, 'movie': True, '.': True}, 1],
          [{'This': True, 'great': True, 'one': True, '.': True}, 1],
          [{'That': True, 'really': True, 'bad': True, 'movie': True, '.': True}, 0],
          [{'This': True, 'terrible': True, 'movie': True, '.': True}, 0]]
         ```

#### 5.4 文本相似度和分类

1. 文本相似度

   1. 词频: 文本中单词出现的频率或次数
   2. 用词频表示文本特征, 度量文本间的相似性
   
2. 文本相似度案例

   1. FreqDist方法获取在文本中每个出现的标识符的频率分布。通常情况下，函数得到的是每个标识符出现的次数与标识符的map映射

      | 标识符 | 出现次数 |
      | ------ | -------- |
      | are    | 209      |
      | the    | 660      |
      | people | 550      |

   2. ```
      import nltk
      from nltk import FreqDist
      
      text1 = 'I like the movie so much '
      text2 = 'That is a good movie '
      text3 = 'This is a great one '
      text4 = 'That is a really bad movie '
      text5 = 'This is a terrible movie'
      
      text = text1 + text2 + text3 + text4 + text5
      
      #分词
      words = nltk.word_tokenize(text)
      
      freq_dist = FreqDist(words) #创建对象
      print(freq_dist['is'])  # 4
      
      #取出常用的n=5个单词
      n = 5
      
      #创建“常用单词列表”
      most_common_words = freq_dist.most_common(n)
      print(most_common_words)  # [('a',4),('movie',4),('is',4),('This',2),('That',2)]
      
      def lookup_pos(most_common_words):
          """ 查找常用单词的位置 """
          result = {}
          pos = 0
          for word in most_common_words:
              result[word[0]] = pos
              pos += 1
          return result  #{'a':0, 'movie':2,....}
      
      #记录位置
      std_pos_dict = lookup_pos(most_common_words)
      print(std_pos_dict)  #{'movie': 0, 'is': 1, 'a': 2, 'That': 3, 'This': 4}
      
      
      #新文本
      new_text = 'That one is a good movie. This is so good!'
      
      # 初始化向量
      freq_vec = [0] * n
      
      #分词
      new_words = nltk.word_tokenize(new_text)
      
      #在“常用单词列表”上计算词频
      for new_word in new_words:
          if new_word in list(std_pos_dict.keys()):
              freq_vec[std_pos_dict[new_word]] += 1
      
      print(freq_vec)  #[1, 2, 1, 1, 1]
      ```

3. 文本分类

   1. TF-IDF （词频-逆文档频率）

      1. TF, Term Frequency（词频）, 单词在 在文本中的次数
      2. IDF, Inverse Document Frequency（逆文档频率）， 衡量单词 普遍性
      3. TF-IDF = TF * IDF
         1. $TF = \frac{某单词在一个文本中出现的次数}{文本中单词的总数}$
         2. $IDF= log(\frac{文本总数}{出现某单词的文本的个数})$
      4. 示例
         1. 含100个单词的文本中 单词cat个数为3，则 TF=3/100=0.03
         2. 样本中 共有10,000,000个文本， 出现cat的文本数为1,000个，则  IDF=log(10,000,000/1,000)=4
         3. TF-IDF = `TF *IDF `= 0.03* 4 = 0.12
      5. NLTK实现TF-IDF  :  `TextCollection.tf_idf()`
      6. TF-IDF应用:    （1）搜索引擎；（2）关键词提取；（3）文本相似性；（4）文本摘要

   2. 案例

      1. nltk.text.TextCollection类是Text的集合
      
         | 方法                                     | 作用                              |
         | ---------------------------------------- | --------------------------------- |
         | nltk.text.TextCollection([text1,text2,]) | 对象构造                          |
         | idf(term)                                | 计算词term在语料库中的逆文档频率  |
         | tf(term,text)                            | 统计term在text中的词频            |
         | tf_idf(term,text)                        | 计算term在句子中的tf_idf,即tf*idf |
      
         
      
      2. ```
         from nltk.text import TextCollection
         
         text1 = 'I like the movie so much '
         text2 = 'That is a good movie '
         text3 = 'This is a great one '
         text4 = 'That is a really bad movie '
         text5 = 'This is a terrible movie'
         
         #构建TextCollection对象, 索引文本合在一块了成一个大文本
         tc = TextCollection([text1,text2,text3,text4,text5])
         
         new_text = 'That one is a good movie. This is so good!'
         word = 'That'
         
         #计算单词that在句子中的tf_idf值
         tf_idf_val = tc.tf_idf(word, new_text)
         tf_idf_val #0.02181644599700369, 即在文本new_text中'that'的TF-IDF的值
         ```

#### 5.5 实战案例：微博情感分析

1. 数据：每个文本文件包含相应类的数据

   1. 0_simpligyweibo.txt
   2. 1_simpligyweibo.txt
   3. 2_simpligyweibo.txt
   4. 3_simpligyweibo.txt
   5. 0：喜悦；1：愤怒；2：厌恶；3：低落

2. 步骤

   1. 文本 `读取`
   2. `分割` 训练集、测试集
   3. 特征 `提取`
   4. 模型 `训练`、`预测`

3. 代码

   1. tools.py

      1. jieba.posseg   #jieba的一个包, 有`__init__.py`模块, 有分词和词性标注功能
      
         rstrip()  #删除字符串右侧的后缀
      
         open('./哈工大停用词表.txt', 'r', encoding='utf-8')  #读取文档
      
         df.iterrows()   #取出df的每一行,  DataFrame（数据框）的遍历函数, 一般与循环搭配.  返回有一个元祖类型，第一个是行的索引，第二个是series，是每一行的内容. 
      
         ```
         for index, row in dataframe.iterrows():
             ...
         ```
      
         
      
         df.iteritems()  #取出df的每一列
      
         enumerate()    #python的内置函数，同时获得索引和值, 一般与循环搭配
      
         ```
         for index,item in enumerate(data)：
             ...
         ```
      
         
      
      2. ```
         #-*- coding: utf-8 -*-
         
         import re
         import jieba.posseg as pseg
         import pandas as pd
         import math
         import numpy as np
         
         #加载常用停用词
         stopwords1 = [line.rstrip() for line in open('./中文停用词库.txt', 'r', encoding='utf-8')]
         #stopwords2 = [line.rstrip() for line in open('./哈工大停用词表.txt', 'r', encoding='utf-8')]
         # stopwords3 = [line.rstrip() for line in open('./四川大学机器智能实验室停用词库.txt', 'r', encoding='utf-8')]
          
         #stopwords = stopwords1 + stopwords2 + stopwords3
         stopwords = stopwords1
         
         
         def proc_text(raw_line):
             """ 处理每行的文本数据, 返回分词结果 """
             #1. 使用 正则表达式 去除非中文字符
             filter_pattern = re.compile('[^\u4E00-\u9FD5]+')
             chinese_only = filter_pattern.sub('', raw_line)
         
             #2. 结巴分词+词性标注
             words_lst = pseg.cut(chinese_only)
         
             #3. 去除停用词
             meaninful_words = []
             for word, flag in words_lst:
                 # if (word not in stopwords) and (flag == 'v'):
                     # 也可根据词性去除非动词等
                 if word not in stopwords:
                     meaninful_words.append(word)
         
             return ' '.join(meaninful_words)
         
         
         def split_train_test(text_df, size=0.8):
             """ 分割训练集和测试集 """
             #为保证每个类中的数据能在训练集中和测试集中的 比例相同，要依次对每个类进行处理
             
             #创建两个DataFrame对象
             train_text_df = pd.DataFrame()
             test_text_df = pd.DataFrame()
         
             labels = [0, 1, 2, 3]
             for label in labels:
                 #找出label的记录
                 text_df_w_label = text_df[text_df['label'] == label]
                 
                 #重新设置索引，保证每个类的记录是从0开始索引，方便之后的拆分
                 text_df_w_label = text_df_w_label.reset_index()
         
                 #默认按80%训练集，20%测试集分割
                 #这里为了简化操作，取前80%放到训练集中，后20%放到测试集中
                 #当然也可以随机拆分80%，20%（尝试实现下DataFrame中的随机拆分）
         
                 n_lines = text_df_w_label.shape[0]  #该类数据的行数
                 split_line_no = math.floor(n_lines * size)  #行数
                 text_df_w_label_train = text_df_w_label.iloc[:split_line_no, :]
                 text_df_w_label_test = text_df_w_label.iloc[split_line_no:, :]
         
                 #放入整体训练集，测试集中
                 train_text_df = train_text_df.append(text_df_w_label_train)
                 test_text_df = test_text_df.append(text_df_w_label_test)
         
             train_text_df = train_text_df.reset_index()
             test_text_df = test_text_df.reset_index()
             return train_text_df, test_text_df
         
         
         def get_word_list_from_data(text_df):
             """ 将数据集中的单词放入到一个列表中 """
             word_list = []
             for _, r_data in text_df.iterrows():
                 word_list += r_data['text'].split(' ')
             return word_list
         
         
         def extract_feat_from_data(text_df, text_collection, common_words_freqs):
             """ 特征提取 """
             #这里只选择TF-IDF特征作为例子
             #可考虑使用词频或其他文本特征作为额外的特征
         
             n_sample = text_df.shape[0]
             n_feat = len(common_words_freqs)
             common_words = [word for word, _ in common_words_freqs]
         
             #初始化
             X = np.zeros([n_sample, n_feat])
             y = np.zeros(n_sample)
         
             print('提取特征...')
             for i, r_data in text_df.iterrows():
                 if (i + 1) % 5000 == 0:
                     print('已完成{}个样本的特征提取'.format(i + 1))
         
                 text = r_data['text']
         
                 feat_vec = []
                 for word in common_words:
                     if word in text:
                         # 如果在高频词中，计算TF-IDF值
                         tf_idf_val = text_collection.tf_idf(word, text)
                     else:
                         tf_idf_val = 0
         
                     feat_vec.append(tf_idf_val)
         
                 #赋值
                 X[i, :] = np.array(feat_vec)
                 y[i] = int(r_data['label'])
         
             return X, y
         
         
         def cal_acc(true_labels, pred_labels):
             """ 计算准确率 """
             n_total = len(true_labels)
             correct_list = [true_labels[i] == pred_labels[i] for i in range(n_total)]
         
             acc = sum(correct_list) / n_total
             return acc
         ```
      
   2. main.py
   
      1. ```
         #main.py
         
         #-*- coding: utf-8 -*-
         
         
         import os
         import pandas as pd
         import nltk
         from tools import proc_text, split_train_test, get_word_list_from_data, extract_feat_from_data, cal_acc
         from nltk.text import TextCollection
         from sklearn.naive_bayes import GaussianNB
         
         dataset_path = './dataset'
         text_filenames = ['0_simplifyweibo.txt', '1_simplifyweibo.txt', '2_simplifyweibo.txt', '3_simplifyweibo.txt']
         
         #原始数据的csv文件
         output_text_filename = 'raw_weibo_text.csv'
         
         #清洗好的文本数据文件
         output_cln_text_filename = 'clean_weibo_text.csv'
         
         #处理和清洗文本数据的时间较长，通过 is_first_run 配置
         #如果第一次运行要对原始文本数据进行处理和清洗，要设为True
         #如果之前已经处理了文本数据，并已保存了清洗好的文本数据，设为False即可
         is_first_run = True
         
         
         def read_and_save_to_csv():
             """ 读取原始文本数据，将标签和文本数据保存成csv """
         
             text_w_label_df_lst = []
             
             for text_filename in text_filenames:
                 text_file = os.path.join(dataset_path, text_filename)
         
                 #获取标签，即0, 1, 2, 3
                 label = int(text_filename[0])
         
                 #读取文本文件
                 with open(text_file, 'r', encoding='utf-8') as f:
                     lines = f.read().splitlines()
         
                 labels = [label] * len(lines)
         
                 text_series = pd.Series(lines)
                 label_series = pd.Series(labels)
         
                 #构造dataframe
                 text_w_label_df = pd.concat([label_series, text_series], axis=1)
                 text_w_label_df_lst.append(text_w_label_df)
         
             result_df = pd.concat(text_w_label_df_lst, axis=0)
         
             #保存成csv文件
             result_df.columns = ['label', 'text']
             result_df.to_csv(os.path.join(dataset_path, output_text_filename), index=None, encoding='utf-8')
         
         
         def run_main():
             """ 主函数 """
             #1. 数据读取，处理，清洗，准备
             if is_first_run:
                 print('处理清洗文本数据中...', end=' ')
                 #如果 第一次运行 要对原始文本数据进行处理和清洗
         
                 #读取原始文本数据，将标签和文本数据保存成csv
                 read_and_save_to_csv()
         
                 #读取处理好的csv文件，构造数据集
                 text_df = pd.read_csv(os.path.join(dataset_path, output_text_filename), encoding='utf-8')
         
                 #处理文本数据
                 text_df['text'] = text_df['text'].apply(proc_text)
         
                 #过滤空字符串
                 text_df = text_df[text_df['text'] != '']
         
                 #保存处理好的文本数据
                 text_df.to_csv(os.path.join(dataset_path, output_cln_text_filename), index=None, encoding='utf-8')
                 print('完成，并保存结果。')
         
             #2. 分割训练集、测试集
             print('加载处理好的文本数据')
             clean_text_df = pd.read_csv(os.path.join(dataset_path, output_cln_text_filename), encoding='utf-8')
             
             #分割训练集和测试集
             train_text_df, test_text_df = split_train_test(clean_text_df)
             
             #查看训练集测试集基本信息
             print('训练集中各类的数据个数：', train_text_df.groupby('label').size())
             print('测试集中各类的数据个数：', test_text_df.groupby('label').size())
         
             #3. 特征提取
             # 计算词频
             n_common_words = 200
         
             #将训练集中的单词拿出来进行统计词频
             print('统计词频...')
             all_words_in_train = get_word_list_from_data(train_text_df)
             #单词进行词频统计
             fdisk = nltk.FreqDist(all_words_in_train)
             common_words_freqs = fdisk.most_common(n_common_words)
             
             print('出现最多的{}个词是：'.format(n_common_words))
             
             for word, count in common_words_freqs:
                 print('{}: {}次'.format(word, count))
             print()
             
         
             #在训练集上提取特征
             text_collection = TextCollection(train_text_df['text'].values.tolist())
             print('训练样本提取特征...', end=' ')
             train_X, train_y = extract_feat_from_data(train_text_df, text_collection, common_words_freqs)
             print('完成')
             print()
         	
             print('测试样本提取特征...', end=' ')
             test_X, test_y = extract_feat_from_data(test_text_df, text_collection, common_words_freqs)
             print('完成')
         
             #4. 训练模型Naive Bayes
             print('训练模型...', end=' ')
             gnb = GaussianNB()
             gnb.fit(train_X, train_y)
             print('完成')
             print()
         
             #5. 预测
             print('测试模型...', end=' ')
             test_pred = gnb.predict(test_X)
             print('完成')
         
             #输出准确率
             print('准确率：', cal_acc(test_y, test_pred))
         
         if __name__ == '__main__':
             run_main()
         ```

