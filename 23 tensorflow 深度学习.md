https://doc.codingdict.com/tensorflow/tfdoc/tutorials/overview.html

https://wiki.jikexueyuan.com/project/tensorflow-zh/resources/dims_types.html

https://www.w3cschool.cn/tensorflow_python/tensorflow_python-36ws28ue.html

## 23 深度学习

TensorFlow课件, HTML文档版本

## Tensorflow课件

1. 关于 TensorFlow
   1. TensorFlow 采用数据流图（data flow graphs），用于数值计算的开源软件
   2. TensorFlow 最初由Google大脑小组开发出来的
2. 数据流图（Data Flow Graph）
   1. “结点”（nodes）和“线”(edges)的 `有向图` 描述数学计算
   2. “节点” : 数学操作， 也表示数据输入（feed in）的起点/输出（push out）的终点，或 读取/写入持久变量（persistent variable）的终点
   3. 线” : “节点”之间的输入/输出关系, 节点间相互联系的 `多维数组`，即`张量`（tensor）
      1. "线" 输运 “size可动态调整”的 `多维数组`，即`“张量”`（tensor）
      2. 张量从图中流过的 `直观图像`, 这是取名 “Tensorflow” 的原因
3. Tensorflow的特征
   1. 高度的灵活性
      1. 计算表示为一个数据流图，就可以使用Tensorflow。
      2. 自己构建图，描写驱动计算的内部循环
   2. 真正的可移植性（Portability）
      1. Tensorflow 在CPU和GPU上运行， 如说可 运行在台式机、服务器、手机移动设备等等
         1. 训练模型在 多个CPU上 `规模化` 运算
         2. 训练好的模型作为产品的一部分用到 `手机app`
         3. 模型作为 `云端服务` 运行在自己的服务器, 或者运行在Docker容器
   3. 多语言支持
      1. python, c++, Go，Java，Lua，Javascript，或者是R
   4. 性能最优化
      1. Tensorflow 提供了 线程、队列、异步操作, 最佳的支持

### 1 认识Tensorflow

1. Tensorflow 下载安装
2. 张量, 计算图, 会话

#### 1.1 下载以及安装

1. 选择类型
   1. TensorFlow仅支持CPU支持
      1. 系统没有NVIDIA®GPU 时的安装版本
   2. TensorFlow支持GPU
      1. 系统有NVIDIA®GPU 时的安装版本
      2. 更快
2. Ubuntu和Linux
   1. 安装GPU版本,
      1. 要安装一大堆NVIDIA软件(不推荐)
         1.  CUDA®Toolkit 8.0
            1. 确保 将相关的Cuda路径名附加到 LD_LIBRARY_PATH环境变量中，如NVIDIA文档中所述。 与CUDA Toolkit 8.0相关的NVIDIA驱动程序
         2. cuDNN v5.1
            1. 确保 CUDA_HOME按照NVIDIA文档中的描述创建环境变量
         3. 有 CUDA Compute Capability 3.0或更高版本的GPU卡
         4. libcupti-dev库，即NVIDIA CUDA Profile Tools界面
            1. 提供高级分析支持
   2. 安装此库的 命令 (用pip安装, 有2.7和3.6版本)
      1. 方法1, 安装 2.7版本
         1. ` pip install https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.1-cp27-none-linux_x86_64.whl`
      2. 方法2, 安装3.6版本
         1. `pip3 install https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.1-cp36-cp36m-linux_x86_64.whl`
3. Mac
   1. 安装2.7和3.4、3.5的CPU版本
      1. 方法1 安装 2.7版本
         1. `pip install https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-1.0.1-py2-none-any.whl`
      2. 方法2 , 安装 3.4, 3.5版本
         1. `pip3 install https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-1.0.1-py3-none-any.whl`

#### 1.2 初识tf

1. Tensorflow 步骤

   1. tensor 表示 数据.
   2. 图 (graph) 表示 计算任务.
   3. 会话（session) 运行 图 

2. 关于新版本

   1. TensorFlow 提供多种API
      1. 最低级API 提供完整的编程控制
      2. tf.contrib.learn 高级API 管理 `数据集`，`估计器`，`培训`, `推理`
      3. 高级TensorFlow API（方法名称包含的那些）contrib仍在开发中
   2. TF 的 `操作` 在 `会话`（Session） 中进行, 设计都在 `图`（Graph）中产生

3. 图

   1. TensorFlow程序 由 构建图 和 执行图 组成
      1. 构建图, 用 `图` 描述op的执行步骤
      2. 执行图, 用 `会话` 执行图中的op
         1. tensorflow中 图形节点操作必须在 `会话` 中运行
   2. 构建简单图
      1. 每个节点 用零个或多个 `张量` 作为输入，并产生 `张量` 作为输出
      2. 常量节点
         1. `node1 = tf.constant(3.0, tf.float32)`
         2. `node2 = tf.constant(4.0)`

4. 构建图

   1. 第一步,  创建源 op (source op). 

      1. 源 op 不需要任何输入,  如 常量 (Constant). 
      2. 源 op 的输出被传递给其它 op 做运算.
      3. TensorFlow Python 库有一个默认图 (default graph), 
         1. op 构造器可为其增加节点. 
         2. 这个默认图对 许多程序已够用.
         3. 默认Graph 始终注册， 通过 `tf.get_default_graph()` 访问

   2. 示例

      1. ``` 
         import tensorflow as tf
         
         # 创建一个常量 op,1x2 矩阵. 这个 op 被作为一个节点,加到默认图中.构造器的返回值代表该常量 op 的返回值.
         matrix1 = tf.constant([[3., 3.]])
         
         # 创建另外一个常量 op,2x1 矩阵.
         matrix2 = tf.constant([[2.],[2.]])
         
         # 创建一个矩阵乘法 matmul op , 把 'matrix1' 和 'matrix2' 作为输入.返回值 'product' 代表矩阵乘法的结果.
         product = tf.matmul(matrix1, matrix2)
         
         #默认图
         tf.get_default_graph()
         matrix1.graph
         matrix2.graph
         ```

      2. 所有方法都不是线程安全的

5. 在会话中启动图

   1. 启动图

      1. 第一步: 创建一个`Session`对象
         1. 会话构造器 启动 `默认图`
      2. Session的 `run()` 方法 执行矩阵乘法op

   2. 会话作用: 

      1. 传递op全部输入. op通常是`并发`执行 

   3. 示例

      1. ```
         #会话构造器, 并启动默认图.
         sess = tf.Session()
         
         # 函数调用 'run(product)' 触发了图中三个 op (两个常量 op 和一个矩阵乘法 op) 的执行.返回值 'result' 是一个 numpy `ndarray` 对象.
         result = sess.run(product)
         
         # 任务完成, 关闭会话.
         sess.close()
         ```

      2. Session对象用完后 要关闭 释放资源

         1. 也可以用 `上下文管理器` 自动关闭动作

6. op

   1. 节点描述 运算操作（operation, op）, 是运算操作的 `实例化`（instance）

   2. tensorflow内建运算操作

      1. |      类型      |                             示例                             |
         | :------------: | :----------------------------------------------------------: |
         |    标量运算    | add、subtract、multiply、divide、floor_div 地板除法（//）、mod 取余（%） |
         |    向量运算    |     concat、slice、splot、constant、rank、shape、shuffle     |
         |    矩阵运算    |           matmul、matrixInverse、matrixDeterminant           |
         |  带状态的运算  |                 variable、assign、assignAdd                  |
         |  神经网络组件  |      SoftMax、Sigmoid、ReLU、Convolution2D、MaxPooling       |
         |   存储、恢复   |                        Save、Restore                         |
         | 队列及同步运算 |         Enqueue、Dequeue、MutexAcquire、MutexRelease         |
         |     控制流     |          Merge、Switch、Enter、Leave、NextIteration          |
         
      2. | 类型                            | 示例                                                         |
         | ------------------------------- | ------------------------------------------------------------ |
         | 基本运算：（+、-、*、/、//、%） | add、subtract、multiply、divide、floor_div 地板除法（//）、mod 取余（%） |
         | 张量运算                        | 求和：tf.add<br/>矩阵相乘：tf.matmul,<br/>求逆运算：tf.linalg.inv |
         | tf.linalg 模块, 线性代数模块    | 伴随矩阵（tf.linalg.adjoint）<br/>行列式的值（tf.linalg.det）<br/>转化为对角阵（tf.linalg.diag）<br/>求解特征值和特征向量（tf.linalg.eigh )<br/>行列式求逆（tf.linalg.inv）<br/>矩阵乘法（tf.linalg.matmul）<br/>矩阵转置（tf.linalg.matrix_transpose）<br/>QR分解（tf.linalg.qr）<br/>SVD奇异值分解（tf.linalg.svd）<br/>LU分解（tf.linalg.lu）<br/>行列式的迹（tf.linalg.trace）<br/> |
         | tf.math模块, 常用的代数运算函数 | 求绝对值( tf.math.abs )<br/>对给定的tensor列表中所有tensor的元素做element-wise的累加和（tf.accumulate_n）<br/>三角函数（tf.math.sin, tf.math.cos, tf.math.tan）<br/>反三角函数（tf.math.acos, tf.math.asin, tf.math.atan）<br/>双曲函数（tf.math.sinh, tf.math.cosh, tf.math.tanh）<br/>反双曲余弦函数（tf.math.asinh, tf.math.acosh）<br/>Bessel样条拟合（tf.math.bassel_i0, tf.bassel_i0e, tf.math.bessel_i1, tf.math.bassel_i1e）<br/>取整（tf.math.ceil, tf.math.floor）<br/>沿某个维度进行的运算（tf.math.reduce_any, tf.math.reduce_std, tf.math.reduce.mean, …）<br/>对数 tf.math.log , 指数 tf.pow(), 开方 tf.sqrt(), 开方 tf.exp() |
         | 深度学习相关的函数              | 混淆矩阵（tf.math.confusion_matrix）<br/>张量某段数据的运算（tf.math.segment_* ） |
      
         1. https://blog.csdn.net/qq_39241986/article/details/103556184?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_utm_term~default-0.base&spm=1001.2101.3001.4242

7. feed

   1. feed机制

      1. 临时替代图中的 `tensor`. 对图中任何操作提交补丁, 直接插入一个 tensor
      2. feed 只在调用它的方法内有效, 方法结束,feed消失

   2. 标记方法

      1. tf.placeholder() 为操作创建 `占位符`
      2. Session.run() 方法中增加`feed_dict` 参数

   3. 示例

      1. ```
         # 创建两个个浮点数占位符op
         input1 = tf.placeholder(tf.float32)
         input2 = tf.placeholder(tf.float32)
         
         #增加一个乘法op
         output = tf.mul(input1, input2)
         
         with tf.Session() as sess:
           # 替换input1和input2的值
           sess.run([output], feed_dict={input1:[7.], input2:[2.]}
         ```

      2. 没有正确提供feed, placeholder() 操作将会产生错误

### 2 Tensorflow进阶

1. 知识点：
   1. 张量
   2. 变量
   3. 名称域
   4. 图
   5. 会话

#### 2.1 张量的阶和数据类型

1. 张量: 

   1. TensorFlow `张量` 数据结构 表示所有的数据
   2. `张量`想象成 `n维的数组` 或 `n维列表`
   3. 张量有 一个静态类型和动态类型的 `维数`
   4. 张量在图的 节点 间流通

2. 阶

   1. 张量的维数 称为 阶, 张量的阶是张量的维数

      1. 张量的阶和矩阵的阶 不是同一个概念

   2. 二阶张量 是 矩阵，一阶张量 是 向量

      1. |  阶  |    数学实例    |    Python    |                             例子                             |
         | :--: | :------------: | :----------: | :----------------------------------------------------------: |
         |  0   |  标量 scalar   |  (只有大小)  |                           s = 483                            |
         |  1   |  向量 vector   | (大小和方向) |                     v = [1.1, 2.2, 3.3]                      |
         |  2   |  矩阵 matrix   |   (数据表)   |            m = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]             |
         |  3   | 3阶张量 tensor |  (数据立体)  | t = [[[2], [4], [6]], [[8], [10], [12]], [[14], [16], [18]]] |
         |  n   |      n阶       | (自己想想看) |                             ....                             |

3. 数据类型

   1. 类型
   
      1. Int, float, double
      2. bool
      3. string
   
   2. Tensors有 数据类型属性
   
      1. |   数据类型   | Python 类型  |                        描述                        |
         | :----------: | :----------: | :------------------------------------------------: |
         |   DT_FLOAT   |  tf.float32  |                    32 位浮点数.                    |
         |  DT_DOUBLE   |  tf.float64  |                    64 位浮点数.                    |
         |   DT_INT64   |   tf.int64   |                  64 位有符号整型.                  |
         |   DT_INT32   |   tf.int32   |                  32 位有符号整型.                  |
         |   DT_INT16   |   tf.int16   |                  16 位有符号整型.                  |
         |   DT_INT8    |   tf.int8    |                  8 位有符号整型.                   |
         |   DT_UINT8   |   tf.uint8   |                  8 位无符号整型.                   |
         |  DT_STRING   |  tf.string   | 可变长度的字节数组.每一个张量元素都是一个字节数组. |
         |   DT_BOOL    |   tf.bool    |                      布尔型.                       |
         | DT_COMPLEX64 | tf.complex64 |       由两个32位浮点数组成的复数:实数和虚数.       |
         |  DT_QINT32   |  tf.qint32   |            用于量化Ops的32位有符号整型.            |
         |   DT_QINT8   |   tf.qint8   |            用于量化Ops的8位有符号整型.             |
         |  DT_QUINT8   |  tf.quint8   |            用于量化Ops的8位无符号整型.             |
   
   

#### 2.2 张量操作

1. 操作张量的函数:

   1. 生成张量、创建随机张量、张量类型, 形状变换, 张量的切片与运算

2. 生成张量(固定值张量)

   1. 生成张量函数

      1. tf.zeros(shape, dtype=tf.float32, name=None)  # 创建元素为零的张量
      2. tf.zeros_like(tensor, dtype=None, name=None)  #指定张量形状, 类型, 
      3. tf.ones(shape, dtype=tf.float32, name=None)  #创建元素为1的张量
      4. tf.ones_like(tensor, dtype=None, name=None)  #指定张量 形状, 类型
      5. tf.fill(dims, value, name=None)  #创建填充了指定标量值的张量, 形状dims,填充value
      6. tf.constant(value, dtype=None, shape=None, name='Const')  #创建常数张量

   2. 示例

      1. ```
         t1 = tf.constant([1, 2, 3, 4, 5, 6, 7])  #用list创建张量
         
         t2 = tf.constant(-1.0, shape=[2, 3])  #用指定值创建指定形状的张量
         ```

   3. 张量包含了一下几个信息

      1. 名字，存储键值对，后续的检索：Const: 0
      2. 形状描述， 描述每一维度的元素个数：（2，3）
      3. 数据类型，如int32,float32

3. 创建随机张量

   1. normal(正态分布) 

      1. 随机数函数 Math.random()  #均匀分布的随机数
      2. tf.truncated_normal(shape, mean=0.0, stddev=1.0, dtype=tf.float32, seed=None, name=None)  #截断的正态分布中输出随机值
      3. tf.random_normal(shape, mean=0.0, stddev=1.0, dtype=tf.float32, seed=None, name=None)  #随机正态分布的数字组成的矩阵

   2. 示例

      1. ```
         #正态分布的 4X4X4 三维矩阵，平均值 0， 标准差 1
         normal = tf.truncated_normal([4, 4, 4], mean=0.0, stddev=1.0)
         
         a = tf.Variable(tf.random_normal([2,2],seed=1))
         b = tf.Variable(tf.truncated_normal([2,2],seed=2))
         init = tf.global_variables_initializer()
         with tf.Session() as sess:
             sess.run(init)
             print(sess.run(a))
             print(sess.run(b))
         ```

   3. 均匀分布

      1. tf.random_uniform(shape, minval=0.0, maxval=1.0, dtype=tf.float32, seed=None, name=None)  #从均匀分布输出随机值.

         1. 生成的值 均匀分布 [minval, maxval) 范围内。下限minval包含在范围内， maxval排除上限。

      2. 示例

         1. ```
            a = tf.random_uniform([2,3],1,10)
            
            with tf.Session() as sess:
              print(sess.run(a))
            ```

   4. tf.random_shuffle(value, seed=None, name=None)   #沿其第一维度随机打乱

      1. 洗牌

   5. tf.set_random_seed(seed)

      1. 设置 图级 随机种子

      2. 示例

         1. 跨会话生成不同的序列， 不设置图级别, op级别的种子

            1. ```
               a = tf.random_uniform([1])
               b = tf.random_normal([1])
               
               print("Session 1")   #Session 1
               with tf.Session() as sess1:
                  print(sess1.run(a))   #[0.5015043]
                  print(sess1.run(a))   #[0.35503483]
                  print(sess1.run(b))   #[0.1490678]
                  print(sess1.run(b))   #[0.04706627]
                  
               
               print("Session 2")  #Session 2
               with tf.Session() as sess2:
                    print(sess2.run(a))  #[0.61596787] 
                    print(sess2.run(a))  #[0.569373]
                    print(sess2.run(b))  #[-0.6041887]
                    print(sess2.run(b))  #[0.34696734]
               ```

         2. 跨会话生成一个可操作的序列， 设置 op 种子

            1. ```
               a = tf.random_uniform([1], seed=1)  #跨会话设置, op 种子
               b = tf.random_normal([1])
               
               
               print("Session 1")   #Session 1
               with tf.Session() as sess1:
                  print(sess1.run(a))   #[0.2390374]
                  print(sess1.run(a))   #[0.22267115]
                  print(sess1.run(b))   #[1.7657305]
                  print(sess1.run(b))   #[-0.9228078]
                  
               
               print("Session 2")  #Session 2
               with tf.Session() as sess2:
                    print(sess2.run(a))  #[0.2390374]
                    print(sess2.run(a))  #[0.22267115]
                    print(sess2.run(b))  #[-1.2054722]
                    print(sess2.run(b))  #[0.2121532]
               ```

         3. 使op产生的随机序列在会话 间 可重复, 设置 图级别 的种子

            1. ```
               tf.set_random_seed(1234)  #图级别 的种子, 也是跨会话的
               a = tf.random_uniform([1])
               b = tf.random_normal([1])
               
               print("Session 1")   #Session 1
               with tf.Session() as sess1:
                  print(sess1.run(a))   #[0.53202796]
                  print(sess1.run(a))   #[0.91749656]
                  print(sess1.run(b))   #[-1.3118125]
                  print(sess1.run(b))   #[-0.44506428]
                  
               
               print("Session 2")  #Session 2
               with tf.Session() as sess2:
                    print(sess2.run(a))  #[0.53202796]
                    print(sess2.run(a))  #[0.91749656]
                    print(sess2.run(b))  #[-1.3118125]
                    print(sess2.run(b))  #[-0.44506428]
               ```

4. 张量变换

   1. 用途: 在图形中改变张量数据类型

   2. 改变类型

      1. 改变张量中数值类型

         1. tf.string_to_number(string_tensor, out_type=None, name=None)
            1. Tensor中的每个字符串转换为指定的数字类型
            2. int32溢出导致错误，而浮点溢出导致舍入值
         2. tf.to_double(x, name='ToDouble')
         3. tf.to_float(x, name='ToFloat')
         4. tf.to_bfloat16(x, name='ToBFloat16')
         5. tf.to_int32(x, name='ToInt32')
         6. tf.to_int64(x, name='ToInt64')
         7. tf.cast(x, dtype, name=None)

      2. 示例

         1. ```
            n1 = tf.constant(["1234","6789"])
            n2 = tf.string_to_number(n1,out_type=tf.float32)
            
            sess = tf.Session()
            
            result = sess.run(n2)
            print(result)
            
            sess.close()
            ```

   3. 形状和变换

      1. 更改张量的形状

         1. tf.shape(input, name=None)  #返回张量的形状
         2. tf.size(input, name=None)
         3. tf.rank(input, name=None)
         4. tf.reshape(tensor, shape, name=None)
         5. tf.squeeze(input, squeeze_dims=None, name=None)
         6. tf.expand_dims(input, dim, name=None)

      2. 示例

         1. ```
            t = tf.constant([[[1, 1, 1], [2, 2, 2]], [[3, 3, 3], [4, 4, 4]]])
            
            sess = tf.Session()
            shape1 = sess.run(tf.shape(t))
            sess.close()
            ```

      3. 静态形状与动态形状

         1. 静态维度

            1. 特点

               1. 创建一个张量或 操作推导出一个张量时, 张量的维度是确定的
               2. 它是一个`元祖` 或 `列表`

            2. tf.Tensor.get_shape  #读取静态形状

               1. ```
                  t = tf.placeholder(tf.float32,[None,2])
                  
                  t.get_shape()  #TensorShape([Dimension(None), Dimension(2)])
                  ```

         2. 动态形状

            1. 特点

               1. 运行 图时，动态形状 用到
               2. None表示维度, 占位符

            2. tf.shape  #描述动态形状

               1. ```
                  t = tf.placeholder(tf.float32,[None,2])
                  print(tf.shape(t))  #Tensor("Shape_4:0", shape=(2,), dtype=int32)
                  ```

         3. tf.squeeze(input, squeeze_dims=None, name=None)

            1. 作用: 删除维度

               1. 将input中维度是1的那一维去掉。
               2. 用 `squeeze_dims` 参数, 指定去掉的维度

            2. 示例

               1. ```
                  import tensorflow as tf
                  
                  sess = tf.Session()
                  data = tf.constant([[1, 2, 1], [3, 1, 1]])
                  print(sess.run(tf.shape(data)))  #[2 3]
                  d_1 = tf.expand_dims(data, 0)
                  d_1 = tf.expand_dims(d_1, 2)
                  d_1 = tf.expand_dims(d_1, -1)
                  d_1 = tf.expand_dims(d_1, -1)
                  print(sess.run(tf.shape(d_1)))  #[1 2 1 3 1 1]
                  d_2 = d_1
                  print(sess.run(tf.shape(tf.squeeze(d_1))))
                  print(sess.run(tf.shape(tf.squeeze(d_2, [2, 4]))))
                  
                  sess.run(d_1)
                  ```
            
         4. tf.expand_dims(input, dim, name=None)
         
            1. 作用 : 添加 指定维度, 与squeeze相反
         
            2. 示例
         
               1. ```
                  import tensorflow as tf
                  import numpy as np
                  
                  sess = tf.Session()
                  data = tf.constant([[1, 2, 1], [3, 1, 1]])
                  sess.run(tf.shape(data))  #array([2, 3])
                  d_1 = tf.expand_dims(data, 0)
                  sess.run(tf.shape(d_1))
                  d_1 = tf.expand_dims(d_1, 2)
                  sess.run(tf.shape(d_1))
                  d_1 = tf.expand_dims(d_1, -1)
                  sess.run(tf.shape(d_1))
                  ```

5. 切片与扩展

   1. 切片或提取张量的部分, 或 多个张量加在一起
      1. tf.slice(input_, begin, size, name=None)
         1. 参数:  begin=[0, 1], size=[1, 2]
      2. tf.split(split_dim, num_split, value, name='split')
      3. tf.tile(input, multiples, name=None)
      4. tf.pad(input, paddings, name=None)
      5. tf.concat(concat_dim, values, name='concat')
      6. tf.pack(values, name='pack')
      7. tf.unpack(value, num=None, name='unpack')
      8. tf.reverse_sequence(input, seq_lengths, seq_dim, name=None)
      9. tf.reverse(tensor, dims, name=None)
      10. tf.transpose(a, perm=None, name='transpose')
      11. tf.gather(params, indices, name=None)
      12. tf.dynamic_partition(data, partitions, num_partitions, name=None)
      13. tf.dynamic_stitch(indices, data, name=None)

6. 其它一些张量运算（了解查阅）

   1. 张量复制与组合
      1. tf.identity(input, name=None)
      2. tf.tuple(tensors, name=None, control_inputs=None)
      3. tf.group(*inputs, **kwargs)
      4. tf.no_op(name=None)
      5. tf.count_up_to(ref, limit, name=None)
   2. 逻辑运算符
      1. tf.logical_and(x, y, name=None)
      2. tf.logical_not(x, name=None)
      3. tf.logical_or(x, y, name=None)
      4. tf.logical_xor(x, y, name='LogicalXor')
   3. 比较运算符
      1. tf.equal(x, y, name=None)
      2. tf.not_equal(x, y, name=None)
      3. tf.less(x, y, name=None)
      4. tf.less_equal(x, y, name=None)
      5. tf.greater(x, y, name=None)
      6. tf.greater_equal(x, y, name=None)
      7. tf.select(condition, t, e, name=None)
      8. tf.where(input, name=None)
   4. 判断检查
      1. tf.is_finite(x, name=None)
      2. tf.is_inf(x, name=None)
      3. tf.is_nan(x, name=None)
      4. tf.verify_tensor_all_finite(t, msg, name=None)   #断言张量不包含任何NaN或Inf
      5. tf.check_numerics(tensor, message, name=None)
      6. tf.add_check_numerics_ops()
      7. tf.Assert(condition, data, summarize=None, name=None)
      8. tf.Print(input_, data, message=None, first_n=None, summarize=None, name=None)

#### 2.3 变量的创建、初始化

1. 变量

   1. 变量的作用: 存储临时值 或 长久存储
   2. 张量（Tensor）存于内存的 `缓存区`
   3. 建模时 明确地初始化
   4. 模型训练后 被存储到磁盘

2. Variable类

   1. tf.Variable.init(initial_value, trainable=True, collections=None, validate_shape=True, name=None)
      1. 作用: 创建带initial_value值的新变量
      2. 参数: 
         1. initial_value: A Tensor或Python对象可转换为a Tensor. 变量的初始值.必须指定的形状, 除非 validate_shape设置为False.
         2. trainable:如果True，默认值将该变量添加到图形集合GraphKeys.TRAINABLE_VARIABLES, 该集合用作 Optimizer 类要使用的变量的默认列表
         3. collections: 图表集合 键列表, 新变量添加到这些集合中. 默认为[GraphKeys.VARIABLES]
         4. validate_shape:  False 允许使用未知形状的值初始化变量, True，默认形状initial_value必须提供.
         5. name: 变量的名称, 可选, 默认'Variable'并自动获取

3. 变量 的创建

   1. 创建变量. 将张量作为初始值传入构造函数 `Variable()`

      1. Tensorflow 提供了 一系列 `操作符` 初始化张量
      2. 变量Variable()可用作图中其他操作的输入, 像任何一样Tensor

   2. 示例

      1. ```
         x = tf.Variable(5.0,name="x")
         weights = tf.Variable(tf.random_normal([784, 200], stddev=0.35),name="weights")
         biases = tf.Variable(tf.zeros([200]), name="biases")
         ```

   3. tf.Variable()向图中添加了几个 操作

      1. 一个variable op, 保存变量值。
      2. 初始化器op. 将变量设置为 初始值. 实际是`tf.assign`操作。
      3. 初始值 ops. 示例中biases变量的zeros op 被添加到图中

4. 变量的初始化

   1. 初始化

      1. 变量的初始化 先完成. 在模型的其它操作运行之前
      2. 最简单的方法: 添加初始化所有变量的 操作, 且 使用模型前先运行那个操作
      3. 常见的初始化模式: `initialize_all_variables()` #将op添加到初始化所有变量的图形

   2. 初始化方法

      1. tf.global_variables_initializer()  初始化方法

         1. tf.global_variables_initializer()  #初始化所有变量

         2. 示例

            1. ```
               init_op = tf.global_variables_initializer()
               with tf.Session() as sess:
                 sess.run(init_op)
               ```

         1. 运行 初始化函数op 初始化变量

            1. 从保存文件还原变量，或 简单地运行assign向变量分配值的op

            2. 示例

               1. ```
                  with tf.Session() as sess:
                      sess.run(w.initializer)
                  ```

      2. initialized_value()

         1. 将已初始化的变量的值 赋值给另一个新变量

         2. 示例

            1. ```
               weights = tf.Variable(tf.random_normal([784, 200], stddev=0.35),name="weights")
               
               w2 = tf.Variable(weights.initialized_value(), name="w2")
               
               w_twice = tf.Variable(weights.initialized_value() * 0.2, name="w_twice")
               ```

      3. 所有变量 自动收集到创建它们的 图

         1. 默认情况, 构造函数将新变量添加到 图集合GraphKeys.GLOBAL_VARIABLES。方便函数 global_variables()返回该集合的内容

5. 张量的 属性

   1. name  #返回变量的名字

      1. ```
         weights = tf.Variable(tf.random_normal([784, 200], stddev=0.35),name="weights")
         weights.name
         ```

   2. op  #返回op操作

      1. ```
         weights = tf.Variable(tf.random_normal([784, 200], stddev=0.35),name="weights")
         weights.op
         ```

6. 张量的 方法

   1. assign()  #为变量分配一个新值

      1. ```
         w = tf.Variable(5.0,name="x")
         w.assign(w + 1.0)
         ```

   2. eval()  #会话中，计算并返回此变量的值. 不是 图构造方法， 不会向图 添加操作

      1. ```
         v = tf.Variable([1, 2])
         init = tf.global_variables_initializer()  # 必须先初始化变量
         
         with tf.Session() as sess:
             sess.run(init)
         
             #使用方法1: 指定会话, with结构中单独指定会话sess对象
             v.eval(sess)  
             
             #使用方法2: 使用默认会话, 使用完一次sess,v与sess就释放联系
             v.eval()
         ```

      2. 小结

         1. sess.run() 的参数必须是 string或Tensor
            1. sess.run(v.eval(sess))  #报错, v.eval(sess) 返回值是标量

7. 变量的形状: 静态形状与动态形状

   1. 张量 有`静态（推测）形状` 和 `动态（真实）形状`
   2. 静态形状
      1. 初始状态的形状, 创建 张量或 由操作推导出张量
      2. 函数
         1. tf.Tensor.get_shape  #获取静态形状
         2. tf.Tensor.set_shape()  #设置/更新Tensor对象的静态形状, 不能直接推断时
   3. 动态形状
      1. 描述原始张量执行过程中的形状
      2. 函数
         1. tf.shape(tf.Tensor)
            1. 运行的时候想知道None到底是多少,只能通过 `tf.shape(tensor)[0]` 获得
         2. tf.reshape()
            1. 创建 具有不同动态形状的新张量
   4. 要点
      1. 转换静态形状的时候，1-D到1-D，2-D到2-D，不能跨`阶数`改变形状
      2. 固定的 或 已设置静态形状的张量／变量，不能再设置静态形状
      3. tf.reshape() 动态创建新张量时，元素个数匹配
      4. 运行时, 获取张量的 **形状值**，只能通过 `tf.shape(tensor)[]`

8. 管理 图中收集的变量

   1. tf.global_variables()  #返回图中收集的所有变量

   2. ```
      weights = tf.Variable(tf.random_normal([784, 200], stddev=0.35))  #必须先定义, 在tf.global_variables_initializer().run()之前
      
      tf.global_variables_initializer().run() #注意顺序, 必须run
      tf.global_variables()
      ```
      
      1. 小结
         1. 必须先 `tf.global_variables_initializer()` 初始化全局变量
#### 2.4 名称域与共享变量

1. 变量作用域

   1. tensorflow的 `变量作用域` `共享变量` 概念, 让模型代码 清晰，作用分明

2. 变量作用域

   1. tf.variable_scope()  #创建指定名字的`变量作用域`

   2. 示例

      1. ```
         with tf.variable_scope("itcast") as scope:
           print("----")
         ```
         
      2. 小结
      
         1. with语句 在 `itcast变量作用域` 下操作
         2. `作用域` 的名字是 变量 itcast

   3. 变量作用域 嵌套

      1. ```
         with tf.variable_scope("itcast") as itcast:
             with tf.variable_scope("python") as python:
               print("----")
         ```
         
      2. with 中嵌套 with
      
   4. 变量作用域下的变量

      1. ```
         with tf.variable_scope("itcast") as scope:
             a = tf.Variable([1.0,2.0],name="a")
             b = tf.Variable([2.0,3.0],name="a")
         ```

      2. 小结

         1. 同一个变量作用域下, 同样名字的变量a b，tensorflow增加 名字`a_1` 表示 b图, 并没有当作同一个
         2. 变量, 与变量名不是一个概念

   5. 变量范围

      1. 示例

         1. ```
            with tf.variable_scope("itcast"):
                a = tf.Variable(1.0,name="a")
                b = tf.get_variable("b", [1])
                print(a.name,b.name)
            ```

         2. 结果: `(u'itcast/a:0', u'itcast/b:0')`

      2. 小结

         1. 在变量作用域中创建变量 时, 在变量的 `name` 前 加 `变量作用域`的名称

   6. 嵌套的变量作用域

      1. 示例

         1. ```
            with tf.variable_scope("itcast"):
                with tf.variable_scope("python"):
                    python3 = tf.get_variable("python3", [1])
            assert python3.name == "itcast/python/python3:0"
            ```

         2. `var2 = tf.get_variable("var",[3,4],initializer=tf.constant_initializer(0.0))`

#### 2.5 图与会话

1. 图

   1. TensorFlow计算，用 `图` 表示。

      1. 图 构成:  `tf.Operation`计算对象  `tf.Tensor`数据对象

   2. tf.get_default_graph 访问默认图

      1. 默认Graph值 注册，用 `tf.get_default_graph` 访问 默认图

      2. 示例

         1. ```
            a = tf.constant(1.0)
            assert a.graph is tf.get_default_graph()
            ```

   3. tf.Graph()

      1. 创建 `图`

      2. 示例

         1. ```
            g1= tf.Graph()
            g2= tf.Graph()
            
            with tf.Session() as sess:
                tf.global_variables_initializer().run()  #全局变量初始化,并运行
                print(g1,g2,tf.get_default_graph())
            ```

2. 图的其它属性和方法

   1. 图 是类, 有属性, 方法

   2. as_default()  

      1. 指定某个 `图` 为 Graph默认图, 返回 `上下文管理器`，
         2. 同一过程中创建多个图，用此方法 指定默认图
         2. 提供 全局默认图, 为了方便
            1. 不指定 图，所有操作都将添加到 默认图
            2. with指定 块范围内创建的操作添加到 默认图

      2. 示例

         1. ```
            g = tf.Graph()  #创建图g对象
            with g.as_default():  #指定g为默认图
              a = tf.constant(1.0)
              assert a.graph is g   #a的图是g
            ```

3. 会话

   1. tf.Session()

      1. 创建会话

         1. 意义: 运行 TensorFlow操作图的 `类`

      2. 示例

         1. ```
            a = tf.constant(5.0)
            b = tf.constant(6.0)
            c = a * b
            
            sess = tf.Session()
            
            print(sess.run(c))
            ```

   2. 开启会话时 指定图

      1. `with tf.Session(graph=g) as sess:`

4. 资源释放

   1. 资源
      1. 有很多, 如 tf.Variable，tf.QueueBase, tf.ReaderBase
      2. 不用资源时, 要释放资源
      
   2. 释放资源

      1. 调用 `tf.Session.close`
      2. 用 `会话` 作上下文管理器

   3. 示例

      1. ```
         # 使用 close 手动关闭
         sess = tf.Session()
         sess.run(...)
         sess.close()
         ```

      2. ```
         # 使用 上下文管理器
         with tf.Session() as sess:
           sess.run(...)
         ```

5. run())

   1. run(fetches, feed_dict=None, options=None, run_metadata=None)

      1. 运行ops和计算tensor
      2. 参数
         1. fetches : 单个图元素，列表/嵌套列表，元组，namedtuple，dict，OrderedDict
         2. feed_dict : 指定张量的值, 字典类型 {键值对}

   2. 示例

      1. ```
         a = tf.placeholder(tf.float32, shape=[])
         b = tf.placeholder(tf.float32, shape=[])
         c = tf.constant([1,2,3])
         
         with tf.Session() as sess:
             a,b,c = sess.run([a,b,c],feed_dict={a: 1, b: 2,c:[4,5,6]})
             print(a,b,c)
         ```

   3. 错误

      1. RuntimeError： Session无效状态（如已关闭）
      2. TypeError： fetches 或 feed_dict键 不合适的类型
      3. ValueError： fetches 或 feed_dict键 无效或引用 Tensor不存在

6. 其它属性和方法

   1. 属性

      1. graph  #返回本次会话中的图

   2. 方法

      1. as_default()   #指定 默认会话

      2. tf.get_default_session()  #获取 默认会话

      3. 示例

         1. ```
            c = tf.constant([1, 2, 4])
            sess = tf.Session()
            
            with sess.as_default():  #指定sess为默认会话
            	tf.get_default_session() #获取默认会话
            	assert tf.get_default_session() is sess
            	print(c.eval())
            ```

      4. 上下文管理器退出时 不会关闭会话， 要手动关闭

         1. ```
            c = tf.constant([1, 2, 4])
            sess = tf.Session()
            with sess.as_default():
            	print(c.eval())
            ```

         2. ```
            with sess.as_default():
            	print(c.eval())
            
            	sess.close()
            ```

#### 2.6 模型保存与恢复、自定义命令行参数

1. 模型保存

   1. 保存训练完成的模型， 从模型恢复继续 测试或 其它使用
   2. 用 `tf.train.Saver` 类实现，
      1. 保存 `Saver` 类到 OPS, 或 恢复变量到 checkpoint
   3. tf.train.Saver(var_list=None, reshape=False, sharded=False, max_to_keep=5, keep_checkpoint_every_n_hours=10000.0, name=None, restore_sequentially=False, saver_def=None, builder=None, defer_build=False, allow_empty=False, write_version=tf.SaverDef.V2, pad_step_number=False)
      1. 参数
         1. var_list, 指定要保存或要还原的 `变量`。一个dict或一个列表(传递)
         2. max_to_keep, 要保留的检查点文件的最大数量
            1. 创建新文件时，会删除较旧的文件
            2. 如果无或0，则保留所有检查点文件。默认 5（即保留最新的5个检查点文件。）
         3. keep_checkpoint_every_n_hours , 多久生成一个新的检查点文件。默认10,000小时

2. tf.train.Saver()

   1. 保存

      1. Saver.save()  #保存模型

         1. save(sess, save_path, global_step=None)

            1. 参数
               1. sess, 会话
               2. save_path, 保存 `目录/文件名`
               3. global_step , 两次保存的间隔

         2. checkpoint 专有格式的二进制文件 , 变量名称 映射到 张量值

         3. 示例

            1. ```
               import tensorflow as tf
               
               a = tf.Variable([[1.0,2.0]],name="a")
               b = tf.Variable([[3.0],[4.0]],name="b")
               c = tf.matmul(a,b)
               
               #创建储存对象saver
               saver=tf.train.Saver()
               with tf.Session() as sess:
                   tf.global_variables_initializer().run()
                   print(sess.run(c))
                   saver.save(sess, './test/matmul')  #目录要存在,否则报错
               ```

      2. 多次训练时, 指定 `多少间隔` 生成检查点文件

         1. 示例

            1. ```
               saver.save(sess, '/tmp/ckpt/test/matmu', global_step=0) ==> filename: 'matmu-0'
               
               saver.save(sess, '/tmp/ckpt/test/matmu', global_step=1000) ==> filename: 'matmu-1000'
               ```

   2. 恢复

      1. restore(sess, save_path)  #恢复模型

         1. save_path , 保存参数的路径

      2. tf.train.latest_checkpoint()  #获取最近的检查点文件(也恶意直接写文件目录)

         1. 参数, string类型, 保存参数的路径

      3. 示例

         1. ```
            import tensorflow as tf
            
            a = tf.Variable([[1.0,2.0]],name="a")
            b = tf.Variable([[3.0],[4.0]],name="b")
            c = tf.matmul(a,b)
            
            saver=tf.train.Saver(max_to_keep=1)
            with tf.Session() as sess:
                tf.global_variables_initializer().run()
                print(sess.run(c))
                saver.save(sess, './matmul')
            
                # 恢复模型
                model_file = tf.train.latest_checkpoint('./')
                saver.restore(sess, model_file)
                print(sess.run([c], feed_dict={a: [[5.0,6.0]], b: [[7.0],[8.0]]}))
            ```

3. tf.app.flags().DEFINE_xxx('参数名',值, '文档')  自定义命令行参数

   1. tf.app.run() 

      1. 调用main()函数，运行程序, `main(argv)` 必须传一个参数

   2. tf.app.flags()  

      1. 定义 命令行参数，指定集群配置。
      2. tf.app.flags 定义各种参数的类型
         1. `.DEFINE_string(flag_name, default_value, docstring)`
         2. `.DEFINE_integer(flag_name, default_value, docstring)`
         3. `.DEFINE_boolean(flag_name, default_value, docstring)`
         4. `.DEFINE_float(flag_name, default_value, docstring)`
         5. 参数:
            1. 第一个 参数的名字
            2. 第二个 参数提供具体的值
            3. 第三个 参数 说明文档

   3. tf.app.flags.FLAGS

      1. flags的 `FLAGS` 标志，它调用前面定义的 flag_name

   4. 示例

      1. ```
         import tensorflow as tf
         
         FLAGS = tf.app.flags.FLAGS
         
         #定义 命令行参数
         tf.app.flags.DEFINE_string('data_dir', '/tmp/tensorflow/mnist/input_data',
                                    """数据集目录""")
         tf.app.flags.DEFINE_integer('max_steps', 2000,
                                     """训练次数""")
         tf.app.flags.DEFINE_string('summary_dir', '/tmp/summary/mnist/convtrain',
                                    """事件文件目录""")
         
         
         def main(argv):
             print(FLAGS.data_dir)
             print(FLAGS.max_steps)
             print(FLAGS.summary_dir)
             print(argv)
         
         
         if __name__=="__main__":
             tf.app.run()
         ```

### 3 Tensorflow IO操作

1. Tensorflow的 `IO` 处理 知识点: 
   1. 线程与队列
   2. 数据读取
   3. 图像操作

#### 3.1 读取数据

1. 线程和队列
   1. TensorFlow 异步计算机制: 队列 
   2. `tf.Coordinator` 和 `tf.QueueRunner` 实现TensorFlow 多线程
      1. Coordinator 线程协调器类 , 同时停止多个线程并且向 等待所有工作线程终止的程序报告异常
      2. QueueRunner 队列管理器类,  协调多个线程 同时将多个 `张量` 推入同一个队列中
   
2. 队列概述

   1. 队列

      1. 队列类: FIFOQueue, RandomShuffleQueue， 张量异步计算中非常重要。

         1. tf.FIFOQueue

            1. 介绍

               1. 先进先出（FIFO）的原则创建一个队列
               2. 队列是Tensorflow的一种数据结构，元素是包含一个或多个张量的元组，元组有静态的类型和尺寸
               3. 入列和出列 支持一次一个元素，或一次一批元素
               4. 继承于Tensorflow 队列的基类 tf.QueueBase
               5. 队列是 Tensorflow 图异步处理张量 的重要对象

            2. 初始化

               1. ```
                  __init__(
                      capacity,
                      dtypes,
                      shapes=None,
                      names=None,
                      shared_name=None,
                      name='fifo_queue'
                  )
                  ```

            3. 属性

               1. capacity
                  整形数字, 队列存储的元素的最大数量
               2. dtypes
                  一个Dtype对象的列表，长度等于队列元素中的张量个数
               3. shapes
                  队列元素中的每个组成部分的尺寸对象组成的列表
               4. name
                  队列操作的命名
               5. shared_name
                  队列在不同session共享时使用的名称
               6. names
                  队列元素中的每个组成部分的命名组成的列表

            4. 方法

               1. close

                  1. ```
                     close(
                         cancel_pending_enqueues=False,
                         name=None
                     )
                     ```

                  2. 关闭队列，标示没有元素再入列，如果队列中还有元素，则出列操作可以执行，否则会失败

                  3. **参数：**
                     cancel_pending_enqueues: 可选，boolean类型，默认False，为True的时候，挂起的请求将被取消；
                     name：可选，队列操作的名称

                  4. **返回：**
                     返回队列的关闭操作

               2. dequeue

                  1. dequeue(name=None)
                  2. 从队列中移出一个元素。当队列为空的时候，将会阻止此操作，直到有一个元素可以出列。
                     队列关闭的情况下，操作会报错。如果队列为空且没有入列操作可执行，则报 tf.errors.OutOfRangeError错误。 如果执行此操作的session关闭, 将报tf.errors.CancelledError错误。
                  3. 参数：
                     name：可选，队列操作的名称
                  4. 返回：
                     返回包含出列张量的元组

               3. dequeue_many

                  1. ```
                      dequeue_many(
                         n,
                         name=None
                     )
                     ```

                     将n个元素连接到一起移出队列

                  2. **参数：**
                     n: 出列张量包含的元素个数
                     name：可选，队列操作的名称

                  3. **返回：**
                     一组连接在一起出列张量组成的列表

      2. 典型输入结构: 模型输入 RandomShuffleQueue
         1. 多个线程 准备 样本，并把样本推入 队列
         2. 一个线程 执行 一个操作

   2. 同步执行队列

      1. ```
         # 创建一个队列
         Q = tf.FIFOQueue(3, dtypes=tf.float32)
         
         # 数据进队列
         init = Q.enqueue_many(([0.1, 0.2, 0.3],))
         
         # 定义操作 op，
         #出队列，+1，进队列
         #返回的都是op
         out_q = Q.dequeue()
         data = out_q + 1
         en_q = Q.enqueue(data)
         
         #会话
         with tf.Session() as sess:
         
             # 初始化队列，是数据进入
             sess.run(init)
         
             # 执行两次入队加1
             for i in range(2):
                 sess.run(en_q)
         
             # 循环取队列
             for i in range(3):
                 print(sess.run(Q.dequeue()))
         ```

3. tf.QueueRunner 队列管理器类

   1. QueueRunner类  用途: 创建一组线程
      1. 这些线程 重复执行 `Enquene` 操作， 用同一个 `Coordinator` 处理线程同步终止。
      2. 一个QueueRunner 运行一个closer thread
      3. Coordinator收到异常报告时，closer thread会自动关闭队列
   2. 用queue runner 实现上述结构
      1. 先建一个 `TensorFlow图`, 图用 `队列` 输入样本
      2. 增加样本, 将样本推入队列中的操作
      3. 增加 training操作 移除队列中的 样本

4. tf.Coordinator 线程协调器类

   1. 作用: 帮助 多个线程`协同工作`，多个线程`同步终止`

   2. 方法
      1. should_stop()  #线程应该停止则返回True。
      2. request_stop()  #请求 线程停止
      3. join()  #等待被指定的线程终止
      
   3. 步骤
      1. 先创建`Coordinator`对象，然后建立使用Coordinator对象的 `线程`
         1. 这些线程通常一直循环运行，直到 `should_stop()` 返回True时停止
      2. 线程停止
         1. 线程调用request_stop()，同时其他线程的 `should_stop()` 返回True，线程都停下来。
      
   4. 示例

      1. ```
         #生成 线程协调器
         coord = tf.train.Coordinator()
         
         #启动线程, 执行线程操作
         threads_list = qr.create_threads(sess,coord=coord,start=True)
         
         #请求其它线程终止
         coord.request_stop()
         
         #关闭线程
         coord.join(threads_list)
         ```

         

5. 异步执行队列

   1. 过程

      1. 主线程 不断取数据，开启 `队列线程` 增加计数，入队
      2. 主线程结束, 队列线程没有结束，就会抛出异常
      3. 主线程没有结束，需要将队列线程关闭，防止主线程等待

   2. 示例

      1. ```
         #创建 队列
         Q = tf.FIFOQueue(1000,dtypes=tf.float32)
         
         #定义 操作op
         var = tf.Variable(0.0)
         increment_op = tf.assign_add(var,tf.constant(1.0))
         en_op = Q.enqueue(increment_op)
         
         #创建 队列管理器，指定 线程数3，指定 队列的操作
         qr = tf.train.QueueRunner(Q,enqueue_ops=[increment_op,en_op]*3)
         
         #会话
         with tf.Session() as sess:
             tf.global_variables_initializer().run()
         
             #生成 线程协调器
             coord = tf.train.Coordinator()
         
             #启动线程, 执行操作
             threads_list = qr.create_threads(sess,coord=coord,start=True)
         
             print(len(threads_list),"----------")
             #主线程去取数据
             for i in range(20):
                 print(sess.run(Q.dequeue()))
         
             #请求其它线程终止
             coord.request_stop()
         
             #关闭线程
             coord.join(threads_list)
         ```

#### 3.2 线程和队列

1. 读取数据

   1. 小数量数据读取, 可完全加载到内存中

      1. 两种方法

         1. 存储在常数中。更简单, 但用更多的内存
         2. 存储在变量中。初始化后，永远不要改变它的值

      2. 使用常量方式 示例

         1. ```
            training_data = [[1,2,3],[2,3,4]]
            training_labels = ['a', 'b']
            with tf.Session():
              input_data = tf.constant(training_data)
              input_labels = tf.constant(training_labels)
            ```

      3. 使用变量方式 示例

         1. 数据流图 建立后初始化这个 变量

         2. ```
            training_data = ...
            training_labels = ...
            with tf.Session() as sess:
            	data_initializer = tf.placeholder(dtype=training_data.dtype, shape=training_data.shape)
            	label_initializer = tf.placeholder(dtype=training_labels.dtype, shape=training_labels.shape)
            	input_data = tf.Variable(data_initalizer, trainable=False, collections=[])
            	input_labels = tf.Variable(label_initalizer, trainable=False, collections=[])
            	
            	sess.run(input_data.initializer, feed_dict={data_initializer: training_data})
            	sess.run(input_labels.initializer, feed_dict={label_initializer: training_lables})
            ```

         3. trainable=False

            1. 防止该变量被数据流图的GraphKeys.TRAINABLE_VARIABLES收集
            2. 不会在训练时, 尝试更新它的值;
            3. 设定`collections=[]` 可防止 `GraphKeys.VARIABLES` 收集后做为保存和恢复的中断点
            4. 设定 标志, 为了减少额外的开销

2. 文件读取

   1. 文件格式 种类
      1. 文本、excel, 图片. TensorFlow有对应的解析函数
      2. 标准TensorFlow格式: TFRecord
         1. TensorFlow内置文件格式 `TFRecord`
            1. 二进制 `数据` 和 类别`标签` 存储在同一文件
            2. 模型训练前, 图像等信息转换为 `TFRecord` 格式
            3. TFRecord文件是 `protobuf`格式。数据不压缩，可快速加载到内存
            4. TFRecords文件包含 tf.train.Example protobuf
               1. 将 `Example` 填充到 协议缓冲区
               2. 将 协议缓冲区 序列化 为 字符串
               3. 将 `字符串` 写入TFRecords文件

3. 数据读取实现

   1. 文件队列 生成函数
      1. tf.train.string_input_producer(string_tensor, num_epochs=None, shuffle=True, seed=None, capacity=32, name=None)  #产生指定的 `文件张量`
   2. 文件阅读器类
      1. tf.TextLineReader  #读取 文本文件, 逗号分隔值（CSV）格式
      2. tf.FixedLengthRecordReader  #读取 每个记录是固定数量字节的 二进制文件
      3. tf.TFRecordReader  #读取 TfRecords文件
   3. 解码
      1. 从文件中读取的是 `字符串`，函数解析 `字符串` 为 `张量`
      2. 解析函数
         1. tf.decode_csv（records，record_defaults,field_delim = None，name = None） 
            1. 将CSV转换为张量
            2. 与 `tf.TextLineReader` 搭配使用
         2. tf.decode_raw（bytes，out_type,little_endian = None，name = None） 
            1. 将 字节 转换为 数字向量
            2. 字节 是字符串类型的张量,
            3. 与函数 tf.FixedLengthRecordReader 搭配使用

4. 创建 文件队列

   1. tf.train.string_input_producer()

      1. 参数传入 `文件名列表`
      2. 构建 先入先出的 文件队列，文件阅读器用它们取数据。
      3. 参数 设置文件名乱序和最大的训练迭代数
         1. QueueRunner 为 迭代（epoch）将所有的文件名加入文件名队列中
         2. 如 shuffle=True,  对文件名乱序处理。
         3. 产生均衡的文件名队列。
      4. QueueRunner线程 独立于文件阅读器的线程
         1. 因此 乱序和将文件名推入到文件名队列 过程不会 阻塞文件阅读器运行
         2. 根据文件格式，选择文件阅读器，将文件名队列提供给阅读器的 `read` 方法
         3. 阅读器的read方法 输出一个键, 表征输入的文件和其中纪录（对于调试非常有用），同时得到一个 字符串标量
            1. 这个 `字符串标量` 可被一个或多个解析器或 转换操作将其 `解码` 为 张量并构造为样本。

   2. 示例

      1. 读取 CSV格式文件 流程

         1、构建 文件队列

         2、构建 读取器，读取内容

         3、解码 内容

         4、读取一个内容，如 有需要，批处理内容

      2. ```
         import tensorflow as tf
         import os
         def readcsv_decode(filelist):
             """
             读取并解析文件内容
             :param filelist: 文件列表
             :return: None
             """
         
             #目录和文件名 合并
             flist = [os.path.join("./csvdata/",file) for file in filelist]
         
             #构建 文件队列
             file_queue = tf.train.string_input_producer(flist,shuffle=False)
         
             #构建 阅读器，读取 文件内容
             reader = tf.TextLineReader()
         
             key,value = reader.read(file_queue)
         
             record_defaults = [["null"],["null"]] # [[0],[0],[0],[0]]
         
             #解码，按行解析，返回的是每行的列数据
             example,label = tf.decode_csv(value,record_defaults=record_defaults)
         
             #通过tf.train.batch 批处理数据
             example_batch,label_batch = tf.train.batch([example,label],batch_size=9,num_threads=1,capacity=9)
         
         
             with tf.Session() as sess:
         
                 #线程协调员
                 coord = tf.train.Coordinator()
         
                 #启动 工作线程
                 threads = tf.train.start_queue_runners(sess,coord=coord)
         
                 #这种方法不可取
                 #for i in range(9):
                 #    print(sess.run([example,label]))
         
                 #打印批处理的数据
                 print(sess.run([example_batch,label_batch]))
         
         
                 coord.request_stop()
         
                 coord.join(threads)
         
             return None
         
         
         if __name__=="__main__":
             filename_list = os.listdir("./csvdata")
             readcsv_decode(filename_list)
         ```

      3. 总结

         1. 每次 read 都会从文件中读取一行内容
            1. 注意，(这与后面的图片和TfRecords读取不一样)
         2. decode_csv操作 解析这一行内容并将其转为 张量列表
            1. 如 输入的参数有缺失，record_default 参数 根据张量的类型 设置默认值
         3. 调用 run 或 eval 执行 read 前, 先调用 `tf.train.start_queue_runners`  将文件名填充到队列。否则 read 操作会被阻塞到文件名队列中, 直到有值为止

#### 3.3 图像操作

1. 图像基本概念

   1. 数字化图片三要素 : 长, 宽, 颜色通道数
      1. 黑白图片 颜色通道数 1， 一个数字 一个像素位
      2. 彩色照片 三个颜色通道，RGB，三个数字 一个像素位
   2. TensorFlow支持 JPG、PNG图像格式，RGB、RGBA颜色空间
      1. 用与图像 尺寸相同 (height*width*chnanel) 张量表示
      2. 图像的像素于磁盘,  要加载到内存

2. 图像大小压缩

   1. 大量无关本征属性信息，影响模型泛化能力
   2. Python图像处理框架 `PIL`、`OpenCV`。
   3. TensorFlow 图像处理方法
      1. `tf.image.resize_images`  #缩小图片大小

3. 图像数据读取实例

   1. 图像加载 与二进制文件相同, 图像 要解码
   2. tf.train.string_input_producer()   #文件队列生成器, 加载文件到队列
   3. tf.WholeFileReader()  #图片阅读器, 加载图像文件到内存
      1. WholeFileReader.read()   #读取图像, 返回一个图片的值
      2. tf.image.decode_jpeg  #解码JPEG格式图像
   4. 图像是 三阶张量, RGB值是 一阶张量
      1. 加载 图像格式 为 `[batch_size,image_height,image_width,channels]`
      2. 批数据图像过大过多，占用内存过高，系统会停止响应。
      3. 加载 `TFRecord` 文件，可节省训练时间。支持写入多个样本

4. 读取图片数据到Tensor

   1. 多文件内容处理

      1. 出队列: 用特定函数 获取 内容队列
         1. tf.train.batch()   #读取 指定大小（个数）的张量
         2. tf.train.shuffle_batch   #乱序读取 指定大小（个数）的张量

   2. 示例

      1. ```
         def readpic_decode(file_list):
             """
             批量读取图片并转换成张量格式
             :param file_list: 文件名目录列表
             :return: None
             """
         
             #构造 文件队列
             file_queue = tf.train.string_input_producer(file_list)
         
             #图片阅读器和读取数据
             reader = tf.WholeFileReader()
             key,value = reader.read(file_queue)
         
             #解码 成张量形式
             image_first = tf.image.decode_jpeg(value)
         
             print(image_first)
         
             #缩小图片到指定长宽，不用指定通道数
             image = tf.image.resize_images(image_first,[256,256])
         
             #设置 图片的静态形状
             image.set_shape([256,256,3])
         
             print(image)
         
             #批处理图片数据，tensors是需要具体的形状大小
             image_batch = tf.train.batch([image],batch_size=100,num_threads=1,capacity=100)
         
             tf.summary.image("pic",image_batch)
         
             with tf.Session() as sess:
         
                 merged = tf.summary.merge_all()
         
                 filewriter = tf.summary.FileWriter("/tmp/summary/dog/",graph=sess.graph)
         
                 #线程协调器
                 coord = tf.train.Coordinator()
         
                 #开启线程
                 threads = tf.train.start_queue_runners(sess=sess,coord=coord)
         
                 print(sess.run(image_batch))
         
                 summary = sess.run(merged)
         
                 filewriter.add_summary(summary)
         
                 #等待线程回收
                 coord.request_stop()
                 coord.join(threads)
         
         
             return None
         
         
         if __name__=="__main__":
         
             #获取 文件列表
             filename = os.listdir("./dog/")
         
             #组合 文件目录和文件名
             file_list = [os.path.join("./dog/",file) for file in filename]
         
             #调用函数读取
             readpic_decode(file_list)
         ```
   
2. 小结
   
   1. 开启线程 `threads = tf.train.start_queue_runners`
         2. 用 线程协调器操作 实现对线程的操作
   
5. 读取 TfRecords文件数据

   1. 示例

      1. CIFAR-10的数据读取以及转换成TFRecordsg格式

         1. tf.app.flags.DEFINE_xxx()  就是添加命令行的 optional argument（可选参数），而tf.app.flags.FLAGS 从对应的命令行参数取出参数
         2. tensorflow 使用tf.app.flags 定义参数，能够使得程序在使用命令行运行程序时，可以通过命令行添加程序参数
      
      2. ```
         #1、数据的读取
         
         FLAGS = tf.app.flags.FLAGS
         
         #定义常量FLAGS
         tf.app.flags.DEFINE_string("data_dir","./cifar10/cifar-10-batches-bin/","CIFAR数据目录")
         tf.app.flags.DEFINE_integer("batch_size",50000,"样本个数")
         tf.app.flags.DEFINE_string("records_file","./cifar10/cifar.tfrecords","tfrecords文件位置")
         
         class CifarRead(object):
         
             def __init__(self,filename):
                 self.filelist = filename
         
                 #定义 图片的长、宽、深度，标签字节，图像字节，总字节数
                 self.height = 32
                 self.width = 32
                 self.depth = 3
                 self.label_bytes = 1
                 self.image_bytes = self.height*self.width*self.depth
                 self.bytes = self.label_bytes + self.image_bytes
         
         
             def readcifar_decode(self):
                 """
                 读取数据，进行转换
                 :return: 批处理的图片和标签
                 """
         
                 #1、构造 文件队列
                 file_queue = tf.train.string_input_producer(self.filelist)
         
                 #2、构造 读取器，读取内容
                 reader = tf.FixedLengthRecordReader(self.bytes)
                 key,value = reader.read(file_queue)
         
                 #3、文件内容 解码
                 image_label = tf.decode_raw(value,tf.uint8)
         
                 #分割 标签与图像张量，转换成相应的格式
         
                 label = tf.cast(tf.slice(image_label,[0],[self.label_bytes]),tf.int32)
         
                 image = tf.slice(image_label,[self.label_bytes],[self.image_bytes])
         
                 print(image)
         
                 #给image设置形状，防止批处理出错
                 image_tensor = tf.reshape(image,[self.height,self.width,self.depth])
         
                 print(image_tensor.eval())
                 #depth_major = tf.reshape(image, [self.depth,self.height, self.width])
                 #image_tensor = tf.transpose(depth_major, [1, 2, 0])
         
                 #4、处理流程
                 image_batch,label_batch = tf.train.batch([image_tensor,label],batch_size=10,num_threads=1,capacity=10)
         
         
                 return image_batch,label_batch
         
         
             def convert_to_tfrecords(self,image_batch,label_batch):
                 """
                 转换成TFRecords文件
                 :param image_batch: 图片数据Tensor
                 :param label_batch: 标签数据Tensor
                 :param sess: 会话
                 :return: None
                 """
         
                 #创建 TFRecord存储器
                 writer = tf.python_io.TFRecordWriter(FLAGS.records_file)
         
                 #构造 每个样本的Example
                 for i in range(10):
                     print("---------")
                     image = image_batch[i]
                     #将单个图片张量转换为字符串，以可以存进二进制文件
                     image_string = image.eval().tostring()
         
                     #使用eval需要注意的是，必须存在会话上下文环境
                     label = int(label_batch[i].eval()[0])
         
                     #构造 协议块
                     example = tf.train.Example(features=tf.train.Features(feature={
                         "image": tf.train.Feature(bytes_list=tf.train.BytesList(value=[image_string])),
                         "label": tf.train.Feature(int64_list=tf.train.Int64List(value=[label]))
                     })
                     )
         
                     #写进文件
                     writer.write(example.SerializeToString())
         
                 writer.close()
         
                 return None
         
             def read_from_tfrecords(self):
                 """
                 读取tfrecords
                 :return: None
                 """
                 file_queue = tf.train.string_input_producer(["./cifar10/cifar.tfrecords"])
         
                 reader = tf.TFRecordReader()
         
                 key, value = reader.read(file_queue)
         
                 features = tf.parse_single_example(value, features={
                     "image":tf.FixedLenFeature([], tf.string),
                     "label":tf.FixedLenFeature([], tf.int64),
                 })
         
                 image = tf.decode_raw(features["image"], tf.uint8)
         
                 #设置静态形状，可用于转换动态形状
                 image.set_shape([self.image_bytes])
         
                 print(image)
         
                 image_tensor = tf.reshape(image,[self.height,self.width,self.depth])
         
                 print(image_tensor)
         
                 label = tf.cast(features["label"], tf.int32)
         
                 print(label)
         
                 image_batch, label_batch = tf.train.batch([image_tensor, label],batch_size=10,num_threads=1,capacity=10)
                 print(image_batch)
                 print(label_batch)
         
                 with tf.Session() as sess:
                     coord = tf.train.Coordinator()
         
                     threads = tf.train.start_queue_runners(sess=sess,coord=coord)
         
                     print(sess.run([image_batch, label_batch]))
         
                     coord.request_stop()
                     coord.join(threads)
         
                 return None
         
         
         if __name__=="__main__":
             #构造文件名字的列表
             filename = os.listdir(FLAGS.data_dir)
             file_list = [os.path.join(FLAGS.data_dir, file) for file in filename if file[-3:] == "bin"]
         
             cfar = CifarRead(file_list)
             #image_batch,label_batch = cfar.readcifar_decode()
             cfar.read_from_tfrecords()
         
             with tf.Session() as sess:
         
         
                 #构建线程协调器
                 coord = tf.train.Coordinator()
         
                 #开启线程
                 threads = tf.train.start_queue_runners(sess=sess, coord=coord)
         
                 #print(sess.run(image_batch))
         
                 #存进文件
                 #cfar.convert_to_tfrecords(image_batch, label_batch)
         
         
           coord.request_stop()
                 coord.join(threads)
         ```
      
         

### 4  可视化学习Tensorboard

1. TensorBoard
   1. 方便 TensorFlow 程序的理解、调试、优化. TensorBoard 可视化工具。
   2. 展现 TensorFlow 图像
   3. 绘制 指标图 以及附加数据
   
2. 数据序列化 -- events文件(事件文件)
   1. TensorBoard 读取 TensorFlow 事件文件
      1. 事件文件包括 TensorFlow的主要数据
      2. 事件文件通过tf.summary.FileWriter()生成
      3. tf.summary.FileWriter() 指定 存储的目录,以及要运行的图
   2. 生成 事件文件 (数据序列化代码)
      1. `tf.summary.FileWriter('/tmp/summary/test/', graph=default_graph)`
   3. 事件文件 的写入方法
      1. add_summary(summary, i)
   
3. 启动TensorBoard
   1. `tensorboard --logdir=path/to/log-directory`
      1. 参数
         1. logdir : FileWriter序列化的数据的 目录
   2. 流程
      1. 运行 :  `Tensorboard --logdir=path/to/log-directory`
      2. 浏览器 `localhost:6006` 查看TensorBoard
   
4. 节点符号

   1. | 符号               | 意义                                   |
      | ------------------ | -------------------------------------- |
      | 高层节点           | 代表 名称域                            |
      | 彼此不连接结点序列 |                                        |
      | 彼此连接节点序列   |                                        |
      | 单独的操作节点     |                                        |
      | 常量节点           |                                        |
      | 摘要节点           |                                        |
      | 数据流边           | 各操作间的数据流边                     |
      | 控制依赖边         | 各操作间的控制依赖边                   |
      | 引用边             | 出度操作节点, 可以使入度tensor发生变化 |

   2. 主图的结构

      1. ```
         import tensorflow as tf
         
         graph = tf.Graph()
         
         with graph.as_default():
             with tf.name_scope("name1") as scope:
                 a = tf.Variable([1.0,2.0],name="a")
             with tf.name_scope("name2") as scope:
                 b = tf.Variable(tf.zeros([20]),name="b")
                 c = tf.Variable(tf.ones([20]),name="c")
             with tf.name_scope("cal") as scope:
                 d = tf.concat([b,c],0)
                 e = tf.add(a,5)
         
         with tf.Session(graph=graph) as sess:
             tf.global_variables_initializer().run()
             # merged = tf.summary.merge_all()
             summary_writer = tf.summary.FileWriter('./', graph=sess.graph)
             sess.run([d,e])
         ```

      2. 图

         1. ```
                      ┌──────────┐ 
                      │    cal   │ 
                      │          │ 
                      │          │ 
                      └──────────┘
                      
                      
            ┌──────────┐         ┌──────────┐   
            │   name2  │         │  name1   │
            │          │         │          │
            │ b    c   │         │ a        │
            └──────────┘         └──────────┘
            ```

         2. 所有的常数、变量、操作 都在对应的 名称域中

            1. name1中，有变量a，依赖于初始化，有对应的张量的阶
            2. name2中，有变量b和c。实线是数据流向边
            3. cal名称域，两个运算操作，数据从前面两个名称域 流入

         3. 名称域使主图 结构 清晰

5. 添加节点汇总操作

   1. tensorboard 功能2,追踪程序中的变量, 图片相关信息 , 流程
   
      1. 收集 变量
      2. 将 变量合并,  训练过程中 写入到 `事件文件`
   
   2. 收集操作 函数
      1. tf.summary.scalar()  收集 损失函数和准确率等 单值变量
      2. tf.summary.histogram()  收集 高维度的 变量参数
      3. tf.summary.image()  收集 输入的图片张量 显示图片
   
   3. 合并变量 函数
   
      1. tf.summary.merge_all()
   
   4. 示例 : 收集变量 并写入 事件文件
   
      1. 收集 变量
   
         1. ```
            cross_entropy = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(labels=y_label, logits=y))
            train_step = tf.train.GradientDescentOptimizer(0.5).minimize(cross_entropy)
            correct_prediction = tf.equal(tf.argmax(y, 1), tf.argmax(y_label, 1))
            accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
            
            tf.summary.scalar("loss",cross_entropy)
            
            tf.summary.scalar("accuracy", accuracy)
            
            tf.summary.histogram("W",W)
            ```
   
      2. 合并 变量，写入事件文件
   
         1. ```
            # 合并
            merged = tf.summary.merge_all()
            
            #生成 事件文件
            summary_writer = tf.summary.FileWriter(FLAGS.summary_dir, graph=sess.graph)
            
            # 运行
            summary = sess.run(merged)
            
            #写入
            summary_writer.add_summary(summary,i)
            ```
   
      3. 运行 `tensorboard`, 直观查看结果
   
      

### 5 神经网络与深度学习

1. 深度学习（deep learning）
   1. 机器学习 的分支
   2. 基于数据 表征学习的方法
   3. 观测值（如图像） 多种方式表示
      1. 如像素强度值的向量，或一系列边、特定形状的区域等
   4. 好处: 非监督特征学习 和 分层特征提取算法 替代手工获取特征
   5. 数种 深度学习框架:
      1. 深度神经网络
      2. 卷积神经网络
      3. 深度置信网络
      4. 递归神经网络
   6. 应用: 计算机视觉、语音识别、自然语言处理、音频识别与生物信息学
   7. 神经网络，启发自生物学的 编程范式， 从观测到的数据中 学习
2. 图像分类
   1. 为输入图像分配一个标签的任务
   2. 示例
      1. 猫图像 248像素宽，400像素高， 三个颜色通道红色，绿色，蓝色（或简称RGB）。
      2. 图像由248 x 400 x 3数字组成，共297,600个数字。
         1. 每个数字是一个整数，0（黑色）--255（白色）
      3. 任务是 297,600个数字 `转成` 一个标签，如“猫”
3. 数据驱动的方法
   1. 先为计算机提供许多示例，然后学习算法。这种方法被称为数据驱动方法，
   2. 它依赖于 标记图像的 训练数据集

#### 5.1 神经网络基础之人工神经网络

1. 神经网络

   1. 出现很早
   2. 最基本的成分是 神经元模型
      1. layer1 --> layer2 --> layer3 --$h_{w,b}(x)$-->
      2. 每个圆圈 是一个神经元
      3. 每条线 是神经元之间的连接
   3. 特征
      1. 神经元 分成了多层
      2. 层与层间的神经元有连接
      3. 层内的神经元没有连接

2. 感知器 perceptron

   1. 神经元 也叫 感知器

      1. 即 神经网络的组成单元

      2. **感知器**，也可翻译为**感知机**

         1. Frank Rosenblatt在1957年在Cornell航空实验室(Cornell Aeronautical Laboratory)时所发明的一种 人工神经网络

      3. 结构

         1. input --weight--> weighted sum --> step function--> output

         2. 结构图

            >  $  1  $  --> $ω_0$ ───┐
            >
            > $x_1$ --> $ω_1$ ───┐
            >
            > ​                         ├-->$\sum$  --> step function --> output
            >
            > $x_2$ --> $ω_2$ ───┘
            >
            > $x_n$ --> $ω_n$───┘

         3. 感知器 组成

            1. 输入权值
               1. 一个感知器有多个输入$x_1, x_2,x_3...x_n$, 每个输入有一个权值$w_i$
            2. 激活函数
               1. 激活函数有许多选择，
                  1. 阶跃函数
                  2. sigmoid函数 $\frac{1}{1+e^{-w*x}}$
                     1. z = $\sum{w_i*x_i}$ 权重数据积之和
                     2. 线性回归中, sigmoid函数 擅长 二分类问题
            3. 输出 $y{=}f\left({w*x}{+}{b}\right)$

3. 神经网络

   1. 神经网络 是按规则连接起来的 多个神经元
      1. 神经网络结构
         1. input layer --> hidden layer --> output layer
         2. input layer $X$ : $x_1$ --> ①, $x_2$ --> ②, $x_3$ --> ③, .....
         3. hidden layer : $a_4$ -- ④  , $a_5 $-- ⑤, $a_6$ -- ⑥, $a_7$ -- ⑦,....
         4. output layer   $Y$ : ⑧ --> $y_1$, ⑨ --> $ y_2$
         5. 小结
            1. ① 到 ⑨ 是 9 个神经元
      2. 结构分析
         1. 每一个样本$X$ ,  得到输入值 $x_1,x_2,x_3$, 是节点1，2，3的输入值
         2. 隐层每一个神经元 有一个偏置项b，和权重 线性组合
            1. *a*4=*s**i**g**m**o**i**d*(*W*41∗*x*1+*W*42∗*x*2+*W*43∗*x*3+*W*4*b*)
            2. *a*5=*s**i**g**m**o**i**d*(*W*51∗*x*1+*W*52∗*x*2+*W*53∗*x*3+*W*5*b*)
            3. *a*6=*s**i**g**m**o**i**d*(*W*61∗*x*1+*W*62∗*x*2+*W*63∗*x*3+*W*6*b*)
            4. *a*7=*s**i**g**m**o**i**d*(*W*71∗*x*1+*W*72∗*x*2+*W*73∗*x*3+*W*7*b*)
         3. 矩阵表示
            1. $\vec a = \begin{bmatrix}   a_4 \\ a_5\\ a_6 \\ a_7\end{bmatrix} $  , $W = \begin{bmatrix} \vec{w_4}\\\vec{w_5} \\\vec{w_6}\\\vec{w_7}   \end{bmatrix} = \begin{bmatrix}   w_{41}, w_{42}, w_{43}, w_{4b}\\w_{51}, w_{52}, w_{53}, w_{5b}\\ w_{61}, w_{62}, w_{63}, w_{6b}\\w_{71}, w_{72}, w_{73}, w_{7b}  \end{bmatrix} $ , $f(\begin{bmatrix}   x_1\\x_2\\x_3\\.\\.\\. \end{bmatrix}) = \begin{bmatrix} f(x_1)\\ f(x_2)\\f(x_3)\\.\\.\\. \end{bmatrix}$
            2. 小结
               1. 矢量 是一列
               2. 二维矩阵的 一行 是一个 矢量
      3. 分类问题的 `类别个数`  等于 输出层的 `神经元个数`

4. 神经网络的训练

   1. 神经网络
      1. 是 模型
      2. 权值 是模型的参数, 是模型要学习的 东西
      3. 超参数 :神经网络的连接方式、层数、每层的节点数 是人为事先设置的。人为设置的参数，是 超参数
   2. 前向传播
      1. 神经网络的训练 类似于 线性回归的 训练优化
      2. 梯度下降 步骤
         1. 计算结果误差
         2. 通过梯度下降找 误差最小
         3. 更新 权重 和 偏置项
         4. 得出每个参数再进行一次计算结果，通过 数学理论优化误差 得变化率$\alpha$
   3. 反向传播
      1. 通过误差最小得到新的 权重, 更新 整个网络参数

5. tensorflow神经网络接口的实现

   1. tf.train.GradientDescentOptimizer(0.5)

      1. 使用 梯度下降 要指定 `学习速率`

   2. 方法

      1. init

         1. 构造 新的梯度下降优化器

         2. ```
            __init__(
                learning_rate,
                use_locking=False,
                name='GradientDescent'
            )
            ```

            1. learning_rate :  tensor或 浮点值，学习速率

      2. minimize

         1. 作用

            1. 添加操作, 更新最小化 loss
            2. 简单结合调用 compute_gradients() 和 apply_gradients()

         2. ```
            minimize(
                loss,
                global_step=None,
                var_list=None,
                gate_gradients=GATE_OP,
                aggregation_method=None,
                colocate_gradients_with_ops=False,
                name=None,
                grad_loss=None
            )
            ```

            1. loss 损失值,变量值
            2. global_step 变量， 每次更新之后加1

#### 5.2 ANN网络分析 (人工神经网络)

1. Mnist手写数字识别
   1. Mnist数据集
      1.  http://yann.lecun.com/exdb/mnist/
      2. 55000行 训练数据集（mnist.train）和10000行的测试数据集（mnist.test）
      3. 每个MNIST数据单元 两部分组成
         1. 一张手写数字的图片, 一个对应的标签
         2. 图片设为“xs”，标签设为“ys”
            1. 训练数据集 图片`mnist.train.images` ，
            2. 训练数据集的标签是 `mnist.train.labels`
         3. 图片是 `黑白图片`，每张 含28像素X28像素 (数组)
            1. 数组已 展开为一个向量，长度 28x28 = 784
            2. MNIST训练数据集中，mnist.train.images 是一个形状为 [60000, 784] 的张量
         4. MNIST 每个图像有 标签，0到9的数字, 表示图像的数字。用one-hot编码
   2. 每个图 是0到9。图像有十个可能。希望看到一个图，给出它是每个数字的概率
   
2. one-hot编码
   1. one-hot编码
      1. 每个类别生成一个布尔列
      2. 每行对应一个样本标签, 每行各个列中只有一列 取值1
      3. 示例
         1. $ \begin{bmatrix}  [ 1 & 0 & 0] \\   [0 & 1 & 0] \\   [0 & 0 & 1]  \end{bmatrix}  $
      
   2. tf.one_hot(indices, depth, on_value=None, off_value=None, axis=None, dtype=None, name=None)
   
      1. 参数
   
         1. indices 在独热编码中位置，即数据集标签
         2. depth 张量的深度，即类别数
   
      2. 示例
   
         1. ```
            indices = [0, 1, 2, 1]
            depth = 3
            
            one = tf.one_hot(indices, depth, on_value=None, off_value=None, axis=None, dtype=None, name=None)
            
            print(one.eval(session = sess))
            ```
   
         2. 输出
   
            1. ```
               [[1. 0. 0.]
                [0. 1. 0.]
                [0. 0. 1.]
                [0. 1. 0.]]
               ```
   
3. SoftMax回归

   1. softmax
      1. 给出了 [0,1]概率值 和为1的列表
      2. 激活函数, 或 激励（activation）函数
      3. 模型最后一步用 softmax 分配概率
         1. 吻合度 被softmax函数转换为 概率值
   2. softmax回归 两步骤
      1. 首先 将输入的数据加载到某些类中
      2. 然后将数据转换成概率。
         1. 每个概率 对应着 独热编码 的类别
   3. softmax 公式
      1. $softmax(x_i) = \frac {exp(x_i)}{∑_j exp(x_j)}$
   4. softmax模型
      1. *y*=softmax(*Wx*+*b*)   # y是概率值的矢量, $y = \begin{bmatrix} y_1 \\ y_2 \\ y_3\end {bmatrix}$

4. 损失计算-交叉熵损失

   1. 误差损失 方法
      1. 预测值与标准值差的平方和
      2. 交叉熵损失 -- 统计, 概率模型中用
   2. 交叉熵
      1. 定义式
         1. $H_{y^′}(y)=−∑_i y_i^′ log(y_i)$
         2. 意义:目标标签值与权值求和后的对应类别输出值
   3. tf.nn.softmax_cross_entropy_with_logits(_sentinel=None, labels=None, logits=None, dim=-1, name=None)
      1. 参数:
         1. labels  经`独热编码`的 标签值
         2. logits  没有被`log`调用的 输入值
      2. 返回 :  交叉熵损失列表
      3. 意义: logits 与 labels 间的softmax交叉熵损失
         1. 函数 包含了softmax功能
         2. logits 和 labels 必须有相同的形状 [batch_size, num_classes] 和相同的类型 (float16, float32, or float64)
      4. 示例
         1. `tf.nn.softmax_cross_entropy_with_logits(labels=y_label, logits=y))`

5. 实现神经网络模型
   1. 获取数据

      1. tensorflow提供下载mnist数据集的接口

      2. ```
         from tensorflow.examples.tutorials.mnist import input_data
         
         # 输入数据, 要指定下载目录
         mnist = input_data.read_data_sets(FLAGS.data_dir, one_hot=True)
         ```

   2. 计算数据

      1. 生成权重 偏置初始值, 为随时接收输入值, 要建立输入 `占位符`

      2. ```
         # 建立输入数据 占位符. 参数: 类型, 形状 None行 784列
         x = tf.placeholder(tf.float32, [None, 784])
         
         #权重 偏置 的初始化 
         W = tf.Variable(tf.zeros([784, 10]))
         b = tf.Variable(tf.zeros([10]))
         
         # 输出结果
         y = tf.matmul(x, W) + b
         ```

   3. 梯度下降优化与训练

      1. TensorFlow知道计算图，**自动**用 `反向传播算法` 确定变量 最小化损失, 用 选定的 `优化算法` 修改变量并减少损失

         1. `train_step = tf.train.GradientDescentOptimizer(0.5).minimize(cross_entropy)`

      2. 每次提供部分值，多次训练 `train_step`，使之不断更新

         1. ```
            # 训练
            for i in range(1000):
                print("第%d次训练"%(i))
                batch_xs, batch_ys = mnist.train.next_batch(100)
                sess.run(train_step, feed_dict={x: batch_xs, y_label: batch_ys})
            ```
   
   4. 模型正确率评估
   
      1. 比较 目标值标签和模型的输出值 相同率，用tf.argmax.它给出某个轴的张量中最大值的 索引 
         1. `correct_prediction = tf.equal(tf.argmax(y, 1), tf.argmax(y_label, 1))`  #两个张量元素比较相同不同
            1. 返回 布尔型列表
      2. 为确定哪个部分是正确的, 转换为浮点数，然后取平均值。如， [True, False, True, True] 变成[1,0,1,1], 哪一个0.75
         1. `accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))`  #聚合函数(类型转换函数())
            1. tf.reduce_mean  聚合函数
            2. tf.cast  类型转换函数
   
   5. 跟踪变量
   
      1. tensorboard 观察变量, 节点汇总操作, 观察损失、准确率以及高维变量W的变化
   
      2. ```
         tf.summary.scalar("loss",cross_entropy)
         
         tf.summary.scalar("accuracy", accuracy)
         
         tf.summary.histogram("W",W)
         ```
   
6. 完整代码

   1. ```
      from __future__ import absolute_import
      from tensorflow.examples.tutorials.mnist import input_data
      
      import tensorflow as tf
      
      FLAGS = tf.app.flags.FLAGS
      
      tf.app.flags.DEFINE_string('data_dir', '/tmp/tensorflow/mnist/input_data',
                                 """数据集目录""")
      tf.app.flags.DEFINE_integer('max_steps', 2000,
                                  """训练次数""")
      tf.app.flags.DEFINE_string('summary_dir', '/tmp/summary/mnist/train',
                                 """事件文件目录""")
      
      def main(sess):
          # 输入数据
          mnist = input_data.read_data_sets(FLAGS.data_dir, one_hot=True)
      
      
          # 建立 输入数据占位符. 参数: 类型, 形状[行, 列]
          x = tf.placeholder(tf.float32, [None, 784])
      
          # 权重和偏置 初始化
          W = tf.Variable(tf.zeros([784, 10]))
          b = tf.Variable(tf.zeros([10]))
      
          # 定义输出结果y
          y = tf.matmul(x, W) + b
      
      
          # 建立类别占位符
          y_label = tf.placeholder(tf.float32, [None, 10])
      
          # 计算交叉熵损失平均值
          cross_entropy = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(labels=y_label, logits=y))
          # 生成优化损失操作
          train_step = tf.train.GradientDescentOptimizer(0.5).minimize(cross_entropy)
          # 比较结果
          correct_prediction = tf.equal(tf.argmax(y, 1), tf.argmax(y_label, 1))
          # 计算正确率平均值
          accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
      
          tf.summary.scalar("loss",cross_entropy)
      
          tf.summary.scalar("accuracy", accuracy)
      
          tf.summary.histogram("W",W)
      
          tf.global_variables_initializer().run()
      
          # 合并所有摘要
          merged = tf.summary.merge_all()
          summary_writer = tf.summary.FileWriter(FLAGS.summary_dir, graph=sess.graph)
      
          # 训练
          for i in range(1000):
      
              print("第%d次训练"%(i))
              #取下一批数据
              batch_xs, batch_ys = mnist.train.next_batch(100)
      
              sess.run(train_step, feed_dict={x: batch_xs, y_label: batch_ys})
      
              print(sess.run(accuracy,feed_dict={x: batch_xs, y_label: batch_ys}))
      
              summary = sess.run(merged,feed_dict={x: batch_xs, y_label: batch_ys})
      
              summary_writer.add_summary(summary,i)
      
          # 模型在测试数据的准确率, y预测值, y_label真实值
          correct_prediction = tf.equal(tf.argmax(y, 1), tf.argmax(y_label, 1))
          test_accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
          print("测试数据准确率：")
          print(sess.run(test_accuracy, feed_dict={x: mnist.test.images,y_label: mnist.test.labels}))
      
      if __name__ == '__main__':
          with tf.Session() as sess:
              main(sess)
      ```
   
   2. 小结
   
      1. 在sess.run() 外单独使用 张量变量报错, 
      2. sess.run() 外的张量变量只是个运算记录, 没有实际值
      3. ANN网络 对图像识别不适合

#### 5.3 卷积神经网络与图像识别

1. 卷积神经网络(Convolutional Neural Network, CNN)
   1. 重要的一种神经网络
   2. 几乎所有图像、语音识别领域的 重要突破 都是卷积神经网络取得的
   3. 谷歌的GoogleNet、微软的ResNet等，打败李世石的AlphaGo也用了这种网络
2. 人工神经网络网络VS卷积神经网络
   1. 人工神经网络 问题
      1. 参数 多
         1. 如 CIFAR-10（一个比赛数据集）中，图像 32x32x3（32宽，32高，3色通道），人工神经网络的第一隐藏层中的单个神经元将具有`32*32* 3 = 3072`个权重
         2. 参数增多 , 完全连接是浪费
      2. 没有利用像素间的位置信息
         1. 图像 的每个像素和周围像素的联系大, 和离得很远的像素的联系小
         2. 值 很小, 这些连接无关紧要
      3. 网络层数限制
         1. 层数越多其表达能力越强
         2. 全连接神经网络的梯度很难传递超过 `3` 层, 不能得到很深的 全连接神经网络
   2. 卷积神经网络 解决思路
      1. 局部连接
         1. 每个神经元 只和一小部分神经元相连。减少了很多参数
      2. 权值共享
         1. 一组连接 共享同一个权重, 不是每个连接有一个不同的权重. 这样减少了很多参数
      3. 下采样
         1. Pooling 减少每层的样本数
            1. 减少参数数量
            2. 提升模型的鲁棒性
      4. 小结
         1. 图像识别任务, 卷积神经网络 尽可能保留重要的参数，去掉大量不重要的参数， 达到更好的学习效果
3. 卷积神经网络CNN
   1. ANN与CNN
      1. 联系
         1. 它们由具有学习权重和偏差的 `神经元` 组成
         2. 每个神经元接收一些输入，执行点积，可选地以非线性跟随它
         3. 整个网络 表现出单一的 `可微分` 评分功能
            1. 从一端的原始图像像素到另一个类的分数
            2. 最后层（完全连接）上有 `损失函数`（如SVM / Softmax）
      2. CNN 定义
         1. CNN结构
            1. 卷积层，池化层，完全连接层FC（如常规神经网络所见）
               1. conv, pool, fc
               2. fc层就是 car, trunck, airplane, ship, horse等具体的类别
            2. 池化层 前一般有 激活函数
               1. RELU 激活函数
            3. 堆叠这些层，形成一个架构
         2. 特点
            1. CNN : 输入3D体积, 输出3D体积
            2. CNN每一层 有三维 神经元：宽度，高度，深度 . (width, height, depth), 
   2. 卷积层
      1. 参数及结构
         1. 四个超参数
            1. 控制输出体积的大小：过滤器大小，深度，步幅, 零填充
            2. 得到的每一个深度也叫 一个Feature Map
      2. 卷积层的处理
         1. 卷积层有 过滤器
            1. 示例1
               1. 若输入值 [32x32x3]的大小。如 每个过滤器（Filter）大小5×5，则CNN中的每个Filter有对应输入体积[5x5x3]区域的权重，`5 *5* 3 = 75`个权重（ 再加1偏置参数）
               2. 输入图像的3个深度分别与 Filter的3个深度 运算
            2. 示例2
               1. 如输入卷的大小为[16x16x20]。用3x3的过滤器，CNN中的每个神经元有`3 *3* 20 = 180` 连接到输入层
      3. 卷积层的输出深度
         1. 卷积层的输出深度 可以指定
         2. 输出深度 等于 本次卷积中 `Filter` 的个数
         3. 假若 用 `64` 个Filter，即 [5,5,3,64]，则得到 64个 `Feature Map`，这64个Feature Map可作下一次操作的输入值
      4. 卷积层的输出宽度
         1. 输出宽度 用`算数公式` 得出
   3. 卷积输出值的计算
      1. 例子
         1. `5*5`的图像，用`3*3`的 filter 卷积，得到一个`3*3`的Feature Map
         2. 计算公式
            1. $a_{i,j} = f(\sum_{m=0}^{2}\sum_{n=0}^{2}w_{m,n}x_{i+m,j+n} + w_b)$
            2. 依次计算可得到 feature map
      2. 步长 
         1. Filter移动的间隔大小, 又称 步幅
   4. 外围填充与多Filter
      1. 每个卷积层 有多个 filter. 即卷积层是很多 filter. 
      2. 卷积后Feature Map的深度(个数)和卷积层的filter个数 相同
   5. 总结输出大小
      1. 输入体积大小$H_1*W_1*D_1$
      2. 四个超参数
         1. Filter数量K
         2. Filter大小F
         3. 步长S
         4. 零填充大小 P
      3. 输出体积大小 $H_2*W_2*D_2$
         1. $H_2=(H_1−F +2 P )/ S + 1$
         2. $W_2 = (W_1 - F + 2P)/S + 1$
         3. $D_2 = K$
4. 新的激活函数-Relu
   1. 卷积后 激活函数 Relu
      1. `f(x) = max(0, x)`
      2. 特点
         1. 速度快 
            1. relu函数 是 max(0,x)，计算代价小.  sigmoid函数 要计算指数和倒数
         2. 稀疏性
            1. 大脑工作时 约5%的神经元是激活的
5. Pooling层
   1. Pooling层 作用 下采样, 去掉Feature Map中不重要的样本，进一步减少参数数量
   2. Pooling的方法很多
      1.  `Max Pooling` :  取最大值，作为采样后的样本值
      2. `Mean Pooling` :取各样本的平均值
   3. 深度为D的Feature Map，各层独立做Pooling， Pooling后的深度仍为D
6. 过拟合解决办法
   1. Dropout
      1. 减少过拟合, 输出层之前加入dropout
      2. 方法
         1. 用一个 `placeholder` 代表一个神经元的输出在dropout中保持不变的 `概率`。
         2. 在训练过程中启用 `dropout`，在测试过程中关闭 dropout。 
      3. TensorFlow的 `tf.nn.dropout` 作用
         1. 屏蔽神经元的输出
         2. 自动处理神经元输出值的 scale
            1. 所以用 `dropout` 时 , 不用考虑scale
      4. 在全连接层 后进行 Dropout
   2. `x= tf.nn.dropout(x_in, 1.0)`
7. FC层
   1. 卷积 池化相当于 特征工程
   2. 全连接 相当于 特征加权
      1. 全连接层 “分类器”作用
8. 实例探究 -- 卷积网络 几种架构
   1.  LeNet
      1. 卷积网络 第一个成功应用, Yann LeCun 1990年 开发
      2. 读取邮政编码，数字
   2. AlexNet
      1. 第一个, 卷积网络计算机视觉
      2. 历克斯·克里维斯基，伊利亚·萨茨基弗，吉奥夫·欣顿发展
   3.  ZFNet
       1.  ILSVRC 2013获奖者,Matthew Zeiler和Rob Fergus的卷积网络。 Zeiler＆Fergus Net的缩写） 
       2.  调整架构超参数，扩展中间卷积层的大小，使第一层的步幅和过滤器尺寸更小，对AlexNet的改进
   4.  GoogleNet
       1.  ILSVRC 2014获奖者, Szegedy等人的卷积网络
       2.  贡献 开发一个初始模块, 大大减少了网络中的参数数量（4M，与AlexNet的60M相比）
       3.  GoogLeNet还有几个后续版本，最近的是Inception-v4
   5.  VGGNet
       1.  2011 ILSVRC 亚军, Karen Simonyan和Andrew Zisserman的网络
       2.  意义:  网络的深度是良好性能的关键组成部分
       3.  特点
           1.  最佳网络 含16个CONV / FC层， 有非常均匀的架构
           2.  从始至终只能执行 `3x3` 卷积和 `2x2` 池
           3.  预先训练的模型可用于Caffe的即插即用
           4.  缺点: 评估和使用更多的内存和参数（140M）, 更昂贵
   6.  ResNet
       1.  2015 ILSVRC 获胜者, Kaiming He 残留网络
       2.  有特殊的 `跳过连接` 和 `批量归一化` 大量使用
       3.  该架构在网络末端 没有 `完全连接层`
       4.  迄今为止最先进的 卷积神经网络模型, 实际使用ConvNets的默认选择（截至2016年5月10日）
   7.  小结
       1.  卷积网络一样
           1.  大多数内存（以及计算时间）在早期的 `CONV` 层 使用
           2.  大多数参数 在最后的`FC` 层
               1.  特殊情况, 第一个FC层 100M的权重，总共140M

#### 5.4 图像识别卷积网络实现案例

1. Mnist数据集卷积网络实现

   1. ANN与CNN
      1. ANN神经网络, MNIST 准确率 92%
      2. CNN神经网络, MNIST 准确率更高
   2. Tensorflow中， `tf.nn` 神经网络 操作模块，包含 `卷积`、`池化`、 `损失`

2. 准备基础函数

   1. 卷积层权重 初始化

      1. 权重 初始化时 加入少量的噪声, 打破对称性 和 避免0梯度

      2. `ReLU`神经元,  较小的 `正数` 初始化偏置项, 避免神经元节点输出恒为0（dead neurons）

      3. 定义两个函数用于初始化, 避免建立模型时 反复 初始化

         1. ```
            def weight_variable(shape):
              initial = tf.truncated_normal(shape, stddev=0.1)  #tf常量
              return tf.Variable(initial)   #tf变量, 用
            ```

         2. ```
            def bias_variable(shape):
              initial = tf.constant(0.1, shape=shape)  #较小的 正数 初始化偏置项
              return tf.Variable(initial)
            ```

   2. 卷积和池化

      1. tf.nn.conv2d(input, filter, strides, padding, use_cudnn_on_gpu=None, name=None)

         1. 参数
            1. input：A Tensor。必须是以下类型之一：float32，float64。
            2. filter：A Tensor。必须与input类型相同
            3. strides：列表ints。1-D长度4. 每个尺寸的滑动窗口的步幅input。
            4. padding A string来自："SAME", "VALID"。填充算法的类型。
            5. use_cudnn_on_gpu：可选bool。默认为True。
            6. name：操作的名称（可选）

      2. tf.nn.max_pool(value, ksize, strides, padding, name=None)

         1. value：A 4-D Tensor, 具有形状[batch, height, width, channels]和类型float32，float64，qint8，quint8，qint32。
         2. ksize：长度> = 4的int列表。输入 张量的每个维度的 窗口大小。
         3. strides：长度> = 4的int列表。输入 张量的每个维度的滑动 窗口的跨度。
         4. padding：一个字符串，或者'VALID'或'SAME'。填补算法。
         5. name：操作的可选名称

      3. 小结

         1. TensorFlow 卷积和池化 很强的灵活性

         2. 步长为2，1个零填充边距的模版。池化选择2*2大小。

            1. ```
               def conv2d(x, W):
               	return tf.nn.conv2d(x, W, strides=[1, 1, 1, 1], padding='SAME')
               ```

            2. ```
               def max_pool_2x2(x):
               	return tf.nn.max_pool(x, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding='SAME')
               ```

3. CNN实现

   * 案例: 两层卷积池化，两个全连接层 及一个过拟合方法

   1. 输入数据占位符准备

      1. 训练要一直提供数据, 准备 占位符，以备训练时 填充

      2. x 转变为 一个`4d`向量

         1. 第2、第3维 图片的宽、高
         2. 第4维 颜色通道数(灰度图的通道数为1，rgb彩色图，为3)

      3. ```
         with tf.variable_scope("data") as scope:
         	y_label = tf.placeholder(tf.float32, [None, 10])  #形状2D
         	x = tf.placeholder(tf.float32, [None, 784])  #形状2D
         	x_image = tf.reshape(x, [-1, 28, 28, 1])   #形状改为4D
         ```

   2. 第一层卷积加池化

      1. 卷积的权重张量形状是[5, 5, 1, 32]

         1. 前两个维度是 patch 的大小
         2. 第三维度输入的 通道数目
         3. 第四维度 输出的通道数目
            1. 每一个输出通道 有一个偏置量

      2. x_image和权值向量 卷积，加上偏置项，然后用 ReLU 激活函数，最后 max pooling。为观察 将高位变量W_con1、b_con1添加到事件文件

         1. ```
            with tf.variable_scope("conv1") as scope: #卷积层1
            	W_conv1 = weight_variable([5, 5, 1, 32])  #权值
                b_conv1 = bias_variable([32])  #偏置项
            
                h_conv1 = tf.nn.relu(conv2d(x_image, W_conv1) + b_conv1)
            
                h_pool1 = max_pool_2x2(h_conv1)
            
                tf.summary.histogram('W_con1', W_conv1)
                tf.summary.histogram('b_con1', b_conv1)
            ```

   3. 第二层卷积加池化

      1. 几个类似的层堆叠 构建 深网络

      2. 第二层 , 接受上一层的输出 `32` 个通道, 用5x5的过滤器，输出64个通道。高位变量 W_con2、b_con2添加到 事件文件

         1. ```
            with tf.variable_scope("conv2") as scope:  #卷积层2
                W_conv2 = weight_variable([5, 5, 32, 64])  #权重
                b_conv2 = bias_variable([64])  #偏置值
            	
            	#先卷积,在激活
                h_conv2 = tf.nn.relu(conv2d(h_pool1, W_conv2) + b_conv2)
                h_pool2 = max_pool_2x2(h_conv2)
            
                tf.summary.histogram('W_con2', W_conv2)
                tf.summary.histogram('b_con2', b_conv2)
            ```
   ```
            
   
   4. 两个全连接层
   
      1. 过程
   
         1. 全连接层: 所有数据 权重乘积求和，特征加权，得 输出结果，也可交叉熵进行优化器优化
         2. 高位变量W_fc1、b_fc1、W_fc2、b_fc2添加到事件文件
   
      2. 示例
   
         1. 第一个全连接层(激活函数层作用)
   
            1. ```
               with tf.variable_scope("fc1") as scope:
                   W_fc1 = weight_variable([7 * 7 * 64, 1024])
                   b_fc1 = bias_variable([1024])
               
                   h_pool2_flat = tf.reshape(h_pool2, [-1, 7 * 7 * 64])
                   h_fc1 = tf.nn.relu(tf.matmul(h_pool2_flat, W_fc1) + b_fc1)
               
                   tf.summary.histogram('W_fc1', W_fc1)
                   tf.summary.histogram('b_fc1', b_fc1)
   ```

         2. 第二个全连接层(dropout层作用)
       
            1. ```
               with tf.variable_scope("fc2") as scope:
                   keep_prob = tf.placeholder(tf.float32)
                   h_fc1_drop = tf.nn.dropout(h_fc1, keep_prob)
               
                   W_fc2 = weight_variable([1024, 10])
                   b_fc2 = bias_variable([10])
               
                   y_conv = tf.matmul(h_fc1_drop, W_fc2) + b_fc2
                   tf.summary.histogram('W_fc2', W_fc2)
                   tf.summary.histogram('b_fc2', b_fc2)
               ```

   5. 计算损失

      1. 交叉熵计算, 损失添加到 事件文件

      2. 示例, 计算损失

         1. ```
            def total_loss(y_conv,y_label):
                """
                计算损失
                :param y_conv: 模型输出
                :param y_label: 数据标签
                :return: 返回损失
                """
                with tf.variable_scope("train") as scope:
                	#y_label真实标签值, logits计算标签值
                	#交叉熵计算
                    cross_entropy = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(labels=y_label, logits=y_conv))
                    
                    #损失添加到 事件文件
                    tf.summary.scalar("loss", cross_entropy)
            
                return cross_entropy
            ```

   6. 训练

      1. mnist数据 稀疏的数据矩阵，所以选优化器AdamOptimizer, 指定学习率。`tf.equal`和 `tf.argmax` 求准确率

      2. 示例

         1. ```
            def train(loss,sess,placeholder):
                """
                训练模型
                :param loss: 损失
                :param sess: 会话
                :param placeholder: 占位符，用于填充数据
                :return: None
                """
                #生成梯度下降优化器, 优化策略
                train_step = tf.train.AdamOptimizer(1e-4).minimize(loss)
            
                #计算准确率
                correct_prediction = tf.equal(tf.argmax(y_conv, 1), tf.argmax(y_label, 1))
                #类型转换cast, 聚合reduce_mean
                accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
                tf.summary.scalar("accuracy", accuracy)
            
                #初始化变量
                tf.global_variables_initializer().run()
            
                #合并所有摘要
                merged = tf.summary.merge_all()
                summary_writer = tf.summary.FileWriter(FLAGS.summary_dir, graph=sess.graph)
            
                #循环训练
                for i in range(FLAGS.max_steps):
                	#next_batch取下批次数据
                    batch_xs,batch_ys = mnist.train.next_batch(50)
            
                    #每100次一次输出
                    if i % 100 == 0:
                        train_accuracy = accuracy.eval(feed_dict={placeholder[0]: batch_xs, placeholder[1]: batch_ys, placeholder[2]: 1.0})
                        print("第%d轮，准确率：%f" % (i, train_accuracy))
            
                        summary = sess.run(merged, feed_dict={placeholder[0]: batch_xs, placeholder[1]: batch_ys, placeholder[2]: 1.0})
                        
                        #添加合并
                        summary_writer.add_summary(summary,i)
            
                    train_step.run(feed_dict={placeholder[0]: batch_xs, placeholder[1]: batch_ys, placeholder[2]: 1.0})
            
                print("测试数据准确率：%g" % accuracy.eval(feed_dict={placeholder[0]: mnist.test.images, placeholder[1]: mnist.test.labels, placeholder[2]: 1.0}))
            ```

   7. 输出结果以及显示

      1. pycharm中编程

      2. 启用 tensorboard 查看训练过程以及结果

         1. 损失和准确率

         2. 卷积参数

         3. 全连接参数

         4. 计算图

            

4. 完整代码如下

   1. ```
      from __future__ import absolute_import
      from tensorflow.examples.tutorials.mnist import input_data
      
      import tensorflow as tf
      
      FLAGS = tf.app.flags.FLAGS
      
      tf.app.flags.DEFINE_string('data_dir', './input_data',
                                 """数据集目录""")
      tf.app.flags.DEFINE_integer('max_steps', 2000,
                                  """训练次数""")
      tf.app.flags.DEFINE_string('summary_dir', './convtrain',
                                 """事件文件目录""")
      
      mnist = input_data.read_data_sets(FLAGS.data_dir, one_hot=True)
      
      
      #权重
      def weight_variable(shape):
        initial = tf.truncated_normal(shape, stddev=0.1)  #tf初始化
        return tf.Variable(initial)  #返回生成的张量
      
      #偏置
      def bias_variable(shape):
        initial = tf.constant(0.1, shape=shape)  #tf初始化
        return tf.Variable(initial)  #返回生成的张量
      
      #卷积
      def conv2d(x, W):
        return tf.nn.conv2d(x, W, strides=[1, 1, 1, 1], padding='SAME')
      
      #池化
      def max_pool_2x2(x):
        return tf.nn.max_pool(x, ksize=[1, 2, 2, 1],strides=[1, 2, 2, 1], padding='SAME')
      
      
      def reference():
          """
          得到模型输出
          :return: 模型输出与数据标签
          """
      
          with tf.variable_scope("data") as scope:
              y_label = tf.placeholder(tf.float32, [None, 10])  #形状[None, 10], None行 10列
              x = tf.placeholder(tf.float32, [None, 784])   #形状[None, 784], None行 784列, 1行是一个样本
              x_image = tf.reshape(x, [-1, 28, 28, 1])  #形状[-1, 28, 28, 1], -1行,28*28行宽, 1通道
      
          with tf.variable_scope("conv1") as scope:
              W_conv1 = weight_variable([5, 5, 1, 32])  #filter5*5, 输入1深度, 输出32深度
              b_conv1 = bias_variable([32])
      		
      		#卷积+偏置, 激活
              h_conv1 = tf.nn.relu(conv2d(x_image, W_conv1) + b_conv1)
      
              h_pool1 = max_pool_2x2(h_conv1)
      
              tf.summary.histogram('W_con1', W_conv1)
              tf.summary.histogram('b_con1', b_conv1)
      
          with tf.variable_scope("conv2") as scope:
              W_conv2 = weight_variable([5, 5, 32, 64])  #filter5*5, 输入32深度, 输出64深度
              b_conv2 = bias_variable([64])
      		
      		#卷积+偏置, 激活
              h_conv2 = tf.nn.relu(conv2d(h_pool1, W_conv2) + b_conv2)
              h_pool2 = max_pool_2x2(h_conv2)
      
              tf.summary.histogram('W_con2', W_conv2)
              tf.summary.histogram('b_con2', b_conv2)
      
          with tf.variable_scope("fc1") as scope:
              W_fc1 = weight_variable([7 * 7 * 64, 1024])  #7*7*64行, 1024列
              b_fc1 = bias_variable([1024])
      
              h_pool2_flat = tf.reshape(h_pool2, [-1, 7 * 7 * 64])   #-1行, 7*7*64列
              #矩阵运算,激活函数
              h_fc1 = tf.nn.relu(tf.matmul(h_pool2_flat, W_fc1) + b_fc1)
      
              tf.summary.histogram('W_fc1', W_fc1)
              tf.summary.histogram('b_fc1', b_fc1)
      
          with tf.variable_scope("fc2") as scope:
              keep_prob = tf.placeholder(tf.float32)
              h_fc1_drop = tf.nn.dropout(h_fc1, keep_prob)
      
              W_fc2 = weight_variable([1024, 10])
              b_fc2 = bias_variable([10])
      
              tf.summary.histogram('W_fc2', W_fc2)
              tf.summary.histogram('b_fc2', b_fc2)
      
              y_conv = tf.matmul(h_fc1_drop, W_fc2) + b_fc2
          return y_conv,y_label,(x,y_label,keep_prob)
      
      
      def total_loss(y_conv,y_label):
          """
          计算损失
          :param y_conv: 模型输出
          :param y_label: 数据标签
          :return: 返回损失
          """
          with tf.variable_scope("train") as scope:
              cross_entropy = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(labels=y_label, logits=y_conv))
              tf.summary.scalar("loss", cross_entropy)
      
          return cross_entropy
      
      
      def train(loss,sess,placeholder):
          """
          训练模型
          :param loss: 损失
          :param sess: 会话
          :param placeholder: 占位符，用于填充数据
          :return: None
          """
          # 生成梯度下降优化器
          train_step = tf.train.AdamOptimizer(1e-4).minimize(loss)
      
          # 计算准确率
          correct_prediction = tf.equal(tf.argmax(y_conv, 1), tf.argmax(y_label, 1))
          accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
          tf.summary.scalar("accuracy", accuracy)
      
          # 初始化变量
          tf.global_variables_initializer().run()
      
          # 合并所有摘要
          merged = tf.summary.merge_all()
          summary_writer = tf.summary.FileWriter(FLAGS.summary_dir, graph=sess.graph)
      
          # 循环训练
          for i in range(FLAGS.max_steps):
              batch_xs,batch_ys = mnist.train.next_batch(50)
      
              # 每过100次进行一次输出
              if i % 100 == 0:
                  train_accuracy = accuracy.eval(feed_dict={placeholder[0]: batch_xs, placeholder[1]: batch_ys, placeholder[2]: 1.0})
                  print("第%d轮，准确率：%f" % (i, train_accuracy))
      
                  summary = sess.run(merged, feed_dict={placeholder[0]: batch_xs, placeholder[1]: batch_ys, placeholder[2]: 1.0})
                  summary_writer.add_summary(summary,i)
      
              train_step.run(feed_dict={placeholder[0]: batch_xs, placeholder[1]: batch_ys, placeholder[2]: 1.0})
      
          print("测试数据准确率：%g" % accuracy.eval(feed_dict={placeholder[0]: mnist.test.images, placeholder[1]: mnist.test.labels, placeholder[2]: 1.0}))
      
      
      if __name__ == '__main__':
          with tf.Session() as sess:
      
              y_conv,y_label,placeholder = reference()
      
              loss = total_loss(y_conv,y_label)
      
              train(loss,sess,placeholder)
      ```

#### 5.5 网络优化改进

1. 学习率: 

   1. 自动更新学习率的函数

2. 指数衰减学习率exponential_decay

   1. ```
      class tf.train.exponential_decay(learning_rate, global_step, decay_steps, decay_rate, staircase=False, name=None)
        """
        实现学习率指数衰减
      
        :param learning_rate:初始的学习率
      
        :param global_step: 全局的训练步数（样本学习了多少次）
      
        :param decay_steps: 衰减系数（每多少步衰减decay_rate）
      
        :param decay_rate: 衰减的速度
      
        :param staircase: 默认False,如设为True,将(global_step / decay_ steps)转换成整数
        """
      ```

   2. 衰减的公式 $learningrate * decay\_rate ^ { global\_step / decay steps}$

3. 使用(可变学习率)

   1. ```
      #学习率根据步伐自动变换，指定每10步衰减速度为0.99，0.001 初始的学习率
      lr = tf.train.exponential_decay(0.001,   #初始的学习率
                                      global_step,   #全局的训练步数
                                      10,   #每10步衰减速度为0.99
                                      0.99,  #衰减的速度
                                      staircase=True)  #(global_step / decay_ steps)转换成整数
      
      # 优化器
      train_op = tf.train.GradientDescentOptimizer(lr).minimize(loss, global_step=global_step)
      ```

      

### 6 多分类图像识别案例

1. CIFAR-10

   1. 10个类别, 60000张 32x32彩色图，每个类别6000张。50000个训练图和10000个测试图像
      1. 10个类别: 飞机, 汽车, 鸟, 猫, 鹿, 狗, 青蛙, 马, 船, 卡车
   2. 分五个训练集和一个测试集，每个集有10000个图像
      1. 测试集每个类 1000 个随机图。训练集 每个类 500 个图像
      2. 

2. 下载数据集

   1. https://www.cs.toronto.edu/~kriz/cifar.html

3. 二进制 版本格式

   1. 含文件data_batch_1.bin，data_batch_2.bin，data_batch_4.bin, data_batch_5.bin, 及test_batch.bin

   2. 文件的格式

      1. ```
         <1 x label> <3072 x 像素>  #一行是一个图像
         ...
         <1 x label> <3072 x 像素>
         ```

         1. 第一个字节 图标签， 0-9 的数字
         2. 3072个字节是图像
            1. 前 1024 个字节 红色通道值
            2. 接者 1024 个绿色
            3. 最后 1024 个是蓝色

      2. 每个文件 含10000个 3073字节的“行”的图像

      3. batches.meta.txt 文件, ASCII文件, 0-9的数字标签映射到有意义的类名

#### 6.1 图片 读取与写入

1. 二进制文件 读取

   1. `tf.FixedLengthRecordReader`  读取, 保存到TFRecords文件

   2. 设计 CifarRead类

      1. 初始化每个图片的大小数据

         1. ```
            def __init__(self, filelist=None):
                # 文件列表
                self.filelist = filelist
                # 每张图片大小 初始化
                self.height = 32
                self.width = 32
                self.channel = 3
                self.label_bytes = 1
                self.image_bytes = self.height * self.width * self.channel
                self.bytes = self.image_bytes + self.label_bytes
            ```

      2. 读取代码

         1. ```
            def read_decode(self):
                """
                读取数据并转换成张量
                :return: 图片数据，标签值
                """
            
                #1、构造文件队列
                file_queue = tf.train.string_input_producer(self.filelist)
            
                #2、构造二进制文件阅读器，读取内容解码成张量
                #构建二进制文件阅读器
                reader = tf.FixedLengthRecordReader(self.bytes)
            	
            	#阅读器读取文件队列, 返回key, value
                key, value = reader.read(file_queue)
            
                #value解码成张量
                image_label = tf.decode_raw(value, tf.uint8)
            
                #tf.slice 分割标签与图片数据
                #标签
                label_tensor = tf.cast(tf.slice(image_label, [0], [self.label_bytes]), tf.int32)
            	
            	#图片数据
                image = tf.slice(image_label, [self.label_bytes], [self.image_bytes])
            
                print(image)
            
                #3、图片数据形状转换
                image_tensor = tf.reshape(image, [self.height, self.width, self.channel])
            
                #4、图片数据批处理,一次从二进制文件读取多少数据出来
            
                image_batch, label_batch = tf.train.batch([image_tensor, label_tensor], batch_size=5000, num_threads=1, capacity=50000)
            
                return image_batch, label_batch
            ```

   3. 保存和读取 `TFRecords` 文件

      1. 两个接口实现

      2. ```
         #保存TFRecords文件
         def write_to_tfrecords(self, image_batch, label_batch):
             """
             读取的数据存储(tfrecords)
             :param image_batch: 图片RGB值
             :param label_batch: 图片标签
             :return:
             """
         
             #1、构造存储器, 参数存储地址字符串
             writer = tf.python_io.TFRecordWriter(FLAGS.image_dir)
         
             #2、每张图片 example协议化，存储
             for i in range(5000):
                 print(i)
         
                 #图片的张量转换成字符串才能写入，否则大小格式不对
                 #图片的张量转换成字符串
                 image = image_batch[i].eval().tostring()
         
                 #标签值
                 label = int(label_batch[i].eval())
         
                 #构造 example协议块，名字 提取时使用
                 #tf.train.Example(features), tf.train.Features(feature), tf.train.Feature(bytes_list),tf.train.BytesList()
                 #Example(Features(Feature(BytesList(value))))
                 #Example(Features(Feature(Int64List(value))))
                 example = tf.train.Example(features=tf.train.Features(feature={
                     "image": tf.train.Feature(bytes_list=tf.train.BytesList(value=[image])),
                     "label": tf.train.Feature(int64_list=tf.train.Int64List(value=[label]))
                 }))
         		
         		#存储器写入数据
                 writer.write(example.SerializeToString())
         
             return None
         
         #读取TFRecords文件
         def read_tfrecords(self):
         
             #1、构建文件的队列
             file_queue = tf.train.string_input_producer([FLAGS.image_dir])
         
             #2、构建tfrecords文件阅读器
             reader = tf.TFRecordReader()
         	
         	#阅读器读取文件队列
             key, value = reader.read(file_queue)
         
             #3、解析 example协议块，返回字典类型数据:feature["image"],feature["label"]
             #解析 tf.parse_single_example(), 参数 value, features
             feature = tf.parse_single_example(value, features={
                 "image": tf.FixedLenFeature([], tf.string),
                 "label": tf.FixedLenFeature([], tf.int64)
             })
         
             #4、数据处理, 图片数据解码，标签数据不用解码
             #4.1 图片数据处理: 
             #图片数据解码
             image = tf.decode_raw(feature["image"], tf.uint8)
         
             #图片数据转换形状
             image_reshape = tf.reshape(image, [self.height, self.width, self.channel])
         
             #改变图片数据类型
             image_tensor = tf.cast(image_reshape, tf.float32)
         
             #4.2 标签数据处理
             #改变数据类型
             label_tensor = tf.cast(feature["label"], tf.int32)
         
             #批处理 图片标签数据, 指定训练数据每批次数量
             image_batch, label_batch = tf.train.batch([image_tensor, label_tensor], batch_size=10, num_threads=1, capacity=10)
         
             return image_batch, label_batch
         ```

2. cifar_data.py文件 : 数据读取 ，当作原始数据

   1. ```
      import tensorflow as tf
      import os
      
      """获取Cifar TFRecords数据文件的程序"""
      
      FLAGS = tf.app.flags.FLAGS
      
      tf.app.flags.DEFINE_string("image_dir", "./cifar10.tfrecords","数据文件目录")
      
      
      class CifarRead(object):
          def __init__(self, filelist=None):
              # 文件列表
              self.filelist = filelist
      
              # 每张图片大小数据初始化
              self.height = 32
              self.width = 32
              self.channel = 3
              self.label_bytes = 1
              self.image_bytes = self.height * self.width * self.channel
              self.bytes = self.image_bytes + self.label_bytes
      	
          def read_decode(self):
              """
              读取数据并转换成张量
              :return: 图片数据，标签值
              """
      
              #1、构造 文件队列
              file_queue = tf.train.string_input_producer(self.filelist)
      
              #2、构造二进制文件的阅读器，解码成张量
              #构造二进制文件的阅读器
              reader = tf.FixedLengthRecordReader(self.bytes)
      		
      		#阅读器读取
              key, value = reader.read(file_queue)
      
              #图像数据 解码成张量
              image_label = tf.decode_raw(value, tf.uint8)
      
              #数据分割出标签与数据
              #分割出标签
              label_tensor = tf.cast(tf.slice(image_label, [0], [self.label_bytes]), tf.int32)
      		#分割出图片
              image = tf.slice(image_label, [self.label_bytes], [self.image_bytes])
      
              print(image)
      
              #3、图片数据形状转换
              image_tensor = tf.reshape(image, [self.height, self.width, self.channel])
      
              #4、图片数据批处理
              image_batch, label_batch = tf.train.batch([image_tensor, label_tensor], batch_size=5000, num_threads=1, capacity=50000)
      
              return image_batch, label_batch
      
          def write_to_tfrecords(self, image_batch, label_batch):
              """
              把读取出来的数据进行存储(tfrecords)
              :param image_batch: 图片RGB值
              :param label_batch: 图片标签
              :return:
              """
      
              #1、构造存储器 tf.python_io.TFRecordWriter()
              writer = tf.python_io.TFRecordWriter(FLAGS.image_dir)
      
              #2、每张图片 example协议化，存储
              for i in range(5000):
                  print(i)
      
                  #图片的张量 转换成字符串才能写进去，否则大小格式不对
                  #图片的张量 转换成字符串
                  image = image_batch[i].eval().tostring()
      
                  #标签值
                  label = int(label_batch[i].eval())
      
                  #构造example协议块，名字在读取时用
                  #Example(Features(Features(BytesList))), Example(Features(Features(Int64List)))
                  example = tf.train.Example(features=tf.train.Features(feature={
                      "image": tf.train.Feature(bytes_list=tf.train.BytesList(value=[image])),
                      "label": tf.train.Feature(int64_list=tf.train.Int64List(value=[label]))
                  }))
      
                  writer.write(example.SerializeToString())
      
              return None
      
          def read_tfrecords(self):
      
              #1、构建文件的队列
              file_queue = tf.train.string_input_producer([FLAGS.image_dir])
      
              #2、构建tfrecords文件阅读器
              reader = tf.TFRecordReader()
      
              key, value = reader.read(file_queue)
      
              #3、解析example协议块，返回字典数据，feature["image"],feature["label"]
              feature = tf.parse_single_example(value, features={
                  "image": tf.FixedLenFeature([], tf.string),
                  "label": tf.FixedLenFeature([], tf.int64)
              })
      
              #4、解码图片数据，标签数据不用
              # 图片数据处理
              image = tf.decode_raw(feature["image"], tf.uint8)
      
              #处理一下形状
              image_reshape = tf.reshape(image, [self.height, self.width, self.channel])
      
              #改变数据类型
              image_tensor = tf.cast(image_reshape, tf.float32)
      
              #标签数据处理
              label_tensor = tf.cast(feature["label"], tf.int32)
      
              #批处理图片数据
              image_batch, label_batch = tf.train.batch([image_tensor, label_tensor], batch_size=10, num_threads=1, capacity=10)
      
              return image_batch, label_batch
      
      
      if __name__ == "__main__":
      
          #获取文件名列表(路径+文件名)
          filename = os.listdir("./cifar10/cifar-10-batches-bin")
      
          filelist = [os.path.join("./cifar10/cifar-10-batches-bin", file) for file in filename if file[-3:] == "bin"]
      
          #实例化
          cfr = CifarRead(filelist)
      
          #读取数据,并生成张量
          image_batch, label_batch = cfr.read_decode()
      
          # image_batch, label_batch = cfr.read_tfrecords()
      
          #会话
          with tf.Session() as sess:
          	#生成线程管理器
              coord = tf.train.Coordinator()
      		
              threads = tf.train.start_queue_runners(sess=sess, coord=coord, start=True)
      
              print(sess.run([image_batch, label_batch]))
      
              #写进tfrecords文件
              cfr.write_to_tfrecords(image_batch, label_batch)
   
              coord.request_stop()
   
              coord.join(threads)
      ```
   ```
   
   ```

#### 6.2 模型接口建立

1. 模型接口的建立

   1. 模型接口 放在cifar_model.py
   2. 四个函数
      1. input()  从cifar_data文件获取数据
      2. inference() 建立神经网络模型的
      3. total_loss()计算模型的损失
      4. train() 梯度下降训练减少损失

2. input代码

   1. ```
      def input():
          """
          获取输入数据
          :return: image,label
          """
      
          #实例化
          cfr = cifar_data.CifarRead()
      
          #读取数据, 生成张量
          image_batch, label_batch = cfr.read_tfrecords()
      
          #目标标签转换为one-hot编码格式
          label = tf.one_hot(label_batch, depth=10, on_value=1.0)
      
          return image_batch, label, label_batch
      ```

3. inference代码

   1. 卷积神经网络模型, 修改图像的通道数 及 两次卷积池化变换后的图像大小

   2. ```
      def inference(image_batch):
          """
          得到模型的输出
          :return: 预测概率输出以及占位符
          """
          #1、数据占位符建立
          with tf.variable_scope("data"):
              #样本标签值
              #y_label = tf.placeholder(tf.float32, [None, 10])  #类型, 形状
      
              #样本特征值
              #x = tf.placeholder(tf.float32, [None, IMAGE_HEIGHT * IMAGE_WIDTH * IMAGE_DEPTH])
      
              #改变形状，供给卷积层使用
              x_image = tf.reshape(image_batch, [-1, 32, 32, 3])
      
          #2、卷积池化第一层
          with tf.variable_scope("conv1"):
              #构建权重， filter: 5*5， 3个输入通道，32个输出通道
              w_conv1 = weight_variable([5, 5, 3, 32])
      
              #构建偏置, 个数位输出通道数
              b_conv1 = bias_variable([32])
      
              #卷积，激活,滑动窗口步长，填充类型
              y_relu1 = tf.nn.relu(tf.nn.conv2d(x_image, w_conv1, strides=[1, 1, 1, 1], padding="SAME") + b_conv1)
      		
      		#池化
              y_conv1 = tf.nn.max_pool(y_relu1, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding='SAME')
      
          #3、卷积池化第二层
          with tf.variable_scope("conv_pool2"):
              #权重， filter: 5*5， 1个输入通道，32个输出通道
              w_conv2 = weight_variable([5, 5, 32, 64])
      
              #偏置, 64个输出通道数
              b_conv2 = bias_variable([64])
      
              #进行卷积，激活,滑动窗口步长，填充类型
              y_relu2 = tf.nn.relu(tf.nn.conv2d(y_conv1, w_conv2, strides=[1, 1, 1, 1], padding="SAME") + b_conv2)
      		
      		#池化
              y_conv2 = tf.nn.max_pool(y_relu2, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding='SAME')
      
          #4、全连接第一层
          with tf.variable_scope("FC1"):
              #构建权重,[7*7*64, 1024]. 卷积池化大小变换32->16->8
              w_fc1 = weight_variable([8 * 8 * 64, 1024])
      
              #构建偏置，1024个数位第一次全连接层输出个数
              b_fc1 = bias_variable([1024])
      		
      		#改变图片数据形状
              y_reshape = tf.reshape(y_conv2, [-1, 8 * 8 * 64])
      
              #矩阵运算,全连接后激活
              y_fc1 = tf.nn.relu(tf.matmul(y_reshape, w_fc1) + b_fc1)
      
          # 5、全连接第二层 dropout后在全连接
          with tf.variable_scope("FC2"):
      
              # dropout层
              drop = tf.nn.dropout(y_fc1, 1.0)
      
              # 构建权重,[1024, 10]
              w_fc2 = weight_variable([1024, 10])
      
              # 构建偏置 [10]
              b_fc2 = bias_variable([10])
      
              # 最后的全连接层
              y_logit = tf.matmul(drop, w_fc2) + b_fc2
      
          return y_logit
      ```

4. total_loss代码

   1. ```
      def total_loss(y_label, y_logit):
          """
          计算训练损失
          :param y_label: 目标值
          :param y_logit: 计算值
          :return: 损失
          """
          with tf.variable_scope("loss"):
      
              #softmax回归, 交叉损失熵
              cross_entropy = tf.nn.softmax_cross_entropy_with_logits(labels=y_label, logits=y_logit)
      
              #损失平均值
              loss = tf.reduce_mean(cross_entropy)
      
          return loss
      ```

5. train代码

   1. ```
      def train(loss, y_label, y_logit, global_step):
          """
          训练数据得出准确率
          :param loss: 损失大小
          :return:
          """
          with tf.variable_scope("train"):
              #自动变换学习率，指定每10步衰减基数为0.99，0.001初始的学习率
              lr = tf.train.exponential_decay(0.001,
                                              global_step,
                                              10,
                                              0.99,
                                              staircase=True)
      
              #优化器创建, 优化
              train_op = tf.train.GradientDescentOptimizer(lr).minimize(loss, global_step=global_step)
      
              #计算准确率
              equal_list = tf.equal(tf.argmax(y_logit, 1), tf.argmax(y_label, 1))
      
              accuracy = tf.reduce_mean(tf.cast(equal_list, tf.float32))
      
          return train_op, accuracy
      ```

6. 完整代码

   1. ```
      import tensorflow as tf
      import os
      import cifar_data
      
      
      from tensorflow.examples.tutorials.mnist import input_data
      
      IMAGE_HEIGHT = 32
      IMAGE_WIDTH = 32
      IMAGE_DEPTH = 3
      
      
      #按照指定形状构建权重变量
      def weight_variable(shape):
          init = tf.truncated_normal(shape=shape, mean=0.0, stddev=1.0, dtype=tf.float32)
          weight = tf.Variable(init)
          return weight
      
      
      #按照制定形状构建偏置变量
      def bias_variable(shape):
          bias = tf.constant([1.0], shape=shape)
          return tf.Variable(bias)
      
      
      def inference(image_batch):
          """
          得到模型的输出
          :return: 预测概率输出以及占位符
          """
          #1、数据占位符建立
          with tf.variable_scope("data"):
              #样本标签值
              #y_label = tf.placeholder(tf.float32, [None, 10])
      
              #样本特征值
              #x = tf.placeholder(tf.float32, [None, IMAGE_HEIGHT * IMAGE_WIDTH * IMAGE_DEPTH])
      
              #改变形状，以提供给卷积层使用
              x_image = tf.reshape(image_batch, [-1, 32, 32, 3])
      
          #2、卷积池化第一层
          with tf.variable_scope("conv1"):
              #构建权重， 5*5， 3个输入通道，32个输出通道
              w_conv1 = weight_variable([5, 5, 3, 32])
      
              #构建偏置, 个数位输出通道数
              b_conv1 = bias_variable([32])
      
              #进行卷积，激活,指定滑动窗口，填充类型
              y_relu1 = tf.nn.relu(tf.nn.conv2d(x_image, w_conv1, strides=[1, 1, 1, 1], padding="SAME") + b_conv1)
      
              y_conv1 = tf.nn.max_pool(y_relu1, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding='SAME')
      
          #3、卷积池化第二层
          with tf.variable_scope("conv_pool2"):
              #构建权重， 5*5， 一个输入通道，32个输出通道
              w_conv2 = weight_variable([5, 5, 32, 64])
      
              #构建偏置, 个数位输出通道数
              b_conv2 = bias_variable([64])
      
              #卷积，激活,指定滑动窗口，填充类型
              y_relu2 = tf.nn.relu(tf.nn.conv2d(y_conv1, w_conv2, strides=[1, 1, 1, 1], padding="SAME") + b_conv2)
      
              y_conv2 = tf.nn.max_pool(y_relu2, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1], padding='SAME')
      
          #4、全连接第一层
          with tf.variable_scope("FC1"):
              #构建权重,[7*7*64, 1024],根据前面的卷积池化后一步步计算的大小变换是32->16->8
              w_fc1 = weight_variable([8 * 8 * 64, 1024])
      
              #构建偏置，个数位第一次全连接层输出个数
              b_fc1 = bias_variable([1024])
      
              y_reshape = tf.reshape(y_conv2, [-1, 8 * 8 * 64])
      
              #全连接结果激活
              y_fc1 = tf.nn.relu(tf.matmul(y_reshape, w_fc1) + b_fc1)
      
          #5、全连接第二层
          with tf.variable_scope("FC2"):
      
              #dropout层
              droup = tf.nn.dropout(y_fc1, 1.0)
      
              #构建权重,[1024, 10]
              w_fc2 = weight_variable([1024, 10])
      
              #构建偏置 [10]
              b_fc2 = bias_variable([10])
      
              #最后的全连接层
              y_logit = tf.matmul(droup, w_fc2) + b_fc2
      
          return y_logit
      
      
      def total_loss(y_label, y_logit):
          """
          计算训练损失
          :param y_label: 目标值
          :param y_logit: 计算值
          :return: 损失
          """
          with tf.variable_scope("loss"):
              #将y_label转换为one-hot编码形式
              #y_onehot = tf.one_hot(y_label, depth=10, on_value=1.0)
      
              #softmax回归，以及计算交叉损失熵
              cross_entropy = tf.nn.softmax_cross_entropy_with_logits(labels=y_label, logits=y_logit)
      
              #计算损失平均值
              loss = tf.reduce_mean(cross_entropy)
      
          return loss
      
      
      def train(loss, y_label, y_logit, global_step):
          """
          训练数据得出准确率
          :param loss: 损失大小
          :return:
          """
          with tf.variable_scope("train"):
              #自动变换学习率，指定每10步衰减基数为0.99，0.001为初始的学习率
              lr = tf.train.exponential_decay(0.001,
                                              global_step,
                                              10,
                                              0.99,
                                              staircase=True)
      
              #优化器
              train_op = tf.train.GradientDescentOptimizer(lr).minimize(loss, global_step=global_step)
      
              #计算准确率
              equal_list = tf.equal(tf.argmax(y_logit, 1), tf.argmax(y_label, 1))
      
              accuracy = tf.reduce_mean(tf.cast(equal_list, tf.float32))
      
          return train_op, accuracy
      
      
      def input():
          """
          获取输入数据
          :return: image,label
          """
      
          #实例化
          cfr = cifar_data.CifarRead()
      
          #生成张量
          image_batch, lab_batch = cfr.read_tfrecords()
      
          #将目标值转换为one-hot编码格式
          label = tf.one_hot(label_batch, depth=10, on_value=1.0)
      
          return image_batch, label, label_batch
      ```

#### 6.3 训练以及高级会话函数

1. 主训练逻辑

   1. cifar_train.py文件实现 训练逻辑
   2. 新 会话函数 tf.train.MonitoredTrainingSession()
      1. 自动建立 `events`文件、`checkpoint`文件，记录重要的信息。
      2. 定义钩子函数，自定义每批次的训练信息，训练的限制
      3. 全局步数，每批次训练 +1计数，内部使用

2. 代码

   1. ```
      import tensorflow as tf
      import cifar_model
      import time
      from datetime import datetime
      
      
      
      def train():
          # 在图中进行训练
          with tf.Graph().as_default():
              # 定义全局步数,必须得使用这个，否则会出现StopCounterHook错误
              global_step = tf.contrib.framework.get_or_create_global_step()
      
              # 获取数据
              image, label, label_1 = cifar_model.input()
      
              # 通过模型进行类别预测
              y_logit = cifar_model.inference(image)
      
              # 计算损失
              loss = cifar_model.total_loss(label, y_logit)
      
              # 进行优化器减少损失
              train_op, accuracy = cifar_model.train(loss, label, y_logit, global_step)
      
              # 通过钩子定义模型输出
              class _LoggerHook(tf.train.SessionRunHook):
                  """Logs loss and runtime."""
                  def begin(self):
                      self._step = -1
                      self._start_time = time.time()
      
                  def before_run(self, run_context):
                      self._step += 1
                      return tf.train.SessionRunArgs(loss, float(accuracy.eval()))  # Asks for loss value.
      
                  def after_run(self, run_context, run_values):
                      if self._step % 10 == 0:
                          current_time = time.time()
                          duration = current_time - self._start_time
                          self._start_time = current_time
                          loss_value = run_values.results
                          examples_per_sec = 10 * 10 / duration
                          sec_per_batch = float(duration / 10)
      
                          format_str = ('%s: step %d, loss = %.2f (%.1f examples/sec; %.3f '
                                        'sec/batch)')
                          print(format_str % (datetime.now(), self._step, loss_value,
                                              examples_per_sec, sec_per_batch))
      
              with tf.train.MonitoredTrainingSession(
                      checkpoint_dir="./cifartrain/train",
                      hooks=[tf.train.StopAtStepHook(last_step=500),# 定义执行的训练轮数也就是max_step，超过了就会报错
                             tf.train.NanTensorHook(loss),
                             _LoggerHook()],
                      config=tf.ConfigProto(
                          log_device_placement=False)) as mon_sess:
                  while not mon_sess.should_stop():
                      mon_sess.run(train_op)
      
      
      def main(argv):
          train()
      
      
      if __name__ == "__main__":
          tf.app.run()
      ```

### 7 分布式Tensorflow

1. Tensorflow特色 分布式计算
   1. 分布式Tensorflow, 高性能的gRPC框架作 底层技术
   2. gRPC(google remote procedure call)
      1. 通信框架
      2. 高性能、跨平台的RPC框架
   3. RPC协议
      1. 远程过程调用协议
      2. 通过网络从远程计算机程序 请求服务。
2. 分布式原理
   1. Tensorflow分布式组成
      1. 多个服务器进程 客户端进程
   2. 几种部署方式:  单机多卡 多机多卡（分布式）
      1. 单机多卡
         1. 单台服务器 多GPU设备
            1. 如, 1台机器 4块GPU
         2. 单机多GPU 训练过程
            1. 单机多GPU
               1. 一次处理4个batch数据(假设 4个GPU训练）， 每个GPU处理一个batch的数据
               2. 单机单GPU 训练, 数据 一个batch一个batch的训练
            2. 变量或参数 保存在CPU
               1. CPU分发数据 给 4个GPU,  GPU计算，得到每个批次 更新的梯度
            3. CPU收集4个GPU要更新的梯度，计算 平均梯度，然后更新。
            4. 循环进行上面步骤
      2. 多机多卡（分布式）
         1. 分布式指多台计算机
3. 分布式的架构
   1. Tensorflow分布式 建 集群
      1. 分布式 作业集合
      2. 一个作业有很多的任务（工作结点），一个工作进程执行一个任务
   2. 节点之间的关系
      1. 分布式机器学习框架中, 作业: 参数作业（parameter job）, 工作结点作业（worker job） 
         1. 参数服务器（parameter server,PS）, 参数作业服务器
            1. 参数的存储和更新
            2. 多台机器组成的集群
         2. 工作结点作业
            1. 计算, 如运行操作
      2. Tensorflow分布式 实现作业间的数据传输, 即:
         1. 参数作业 --> 工作结点作业的 `前向传播`
         2. 工作节点作业 --> 参数作业的 `反向传播`
4. 分布式的模式
   1. 数据并行
      1. CPU负责 梯度平均 参数更新, GPU负责 训练模型副本
      2. 过程
         1. 模型副本 在GPU 
         2. 每一个GPU从CPU获得数据，前向传播( 计算)，得损失, 计算梯度
         3. CPU接收GPU的梯度，取梯度平均值，更新梯度
   2. 参数更新方式:同步 异步
      1. 即: 异步随机梯度下降法（Async-SGD）, 同步随机梯度下降法（Sync-SGD)
      2. 同步随即梯度下降法:
         1. 训练时，每个节点读入共享参数，计算梯度，等所每个工作节点算好 局部的梯度，然后将所有共享参数合并、累加，`一次更新`到模型的参数. 下一个批次，每个节点拿到模型更新后的参数再训练
         2. 特点
            1. 优势: 每个训练批次 考虑了所有工作节点的训练情况，损失下降稳定
            2. 劣势: 性能瓶颈, 最慢的工作结点
      3. 异步随机梯度下降法:
         1. 每个工作结点 独立计算局部梯度， 异步更新到模型的参数中，不需要协调等待
         2. 特点
            1. 优势: 不存在性能瓶颈
            2. 劣势: 每个工作节点计算的梯度值发回参数服务器有 `参数更新的冲突`，影响算法的收敛速度，损失下降 `抖动`大

#### 7.1 分布式接口

1. 集群创建方法: 一个任务启动一个服务

   1. 这些任务可分布在不同的机器. 也可一台机器启动多个任务，用不同的GPU运行
   2. 每个任务 要创建完成的工作
      1. 创建一个tf.train.ClusterSpec
         1. 描述集群的所有任务
         2. 该描述内容对所有任务 是相同的
      2. 创建一个tf.train.Server
         1. 创建一个任务
         2. 运行计算任务。

2. Tensorflow的分布式API使用

   1. tf.train.ClusterSpec()

      1. 创建任务描述

         1. 创建ClusterSpec,  分布式TensorFlow计算的一组进程

      2. ```
         #创建ClusterSpec任务描述对象, ps worker是任务类型名称
         cluster = tf.train.ClusterSpec({"worker": ["worker0.example.com:2222", /job:worker/task:0
                                                    "worker1.example.com:2222", /job:worker/task:1
                                                    "worker2.example.com:2222"], /job:worker/task:2
                                         "ps": ["ps0.example.com:2222", /job:ps/task:0
                                                "ps1.example.com:2222"]}) /job:ps/task:1
         ```

         1. 创建Tensorflow集群描述信息，ps和worker 作业名称，指定 `ip地址和端口` 创建

   2. tf.train.Server(server_or_cluster_def, job_name=None, task_index=None, protocol=None, config=None, start=True)

      1. 参数

         1. server_or_cluster_def: 集群描述ClusterSpec对象名
         2. job_name: 任务类型名称
         3. task_index: 任务数

      2. 作用(创建服务, 启动运行任务)

         1. 创建一个服务（主节点或者工作节点服务）
         2. 运行相应作业上的计算任务
            1. task_index 指定机器启动任务

      3. 示例 : 不同的 `ip+端口` 启动两个工作任务

         1. ```
            #第一个任务
            cluster = tf.train.ClusterSpec({"worker": ["localhost:2222","localhost:2223"]})
            server = tf.train.Server(cluster, job_name="worker", task_index=0)
            ```

         2. ```
            #第二个任务
            cluster = tf.train.ClusterSpec({"worker": ["localhost:2222","localhost:2223"]})
            server = tf.train.Server(cluster, job_name="worker", task_index=1)
            ```

      4. 属性：target

         1. 返回tf.Session连接到此服务器的目标

      5. 方法：join()

         1. 参数服务器端 `等待` 接受参数任务，直到服务器关闭

      6. `tf.device(device_name_or_function)`  #指定设备 执行张量运算

         1. 在指定设备上 工作任务端的代码执行张量运算, 即 指定CPU或者GPU运行代码

         2. ```
            with tf.device("/job:ps/task:0"):
              weights = tf.Variable(...)
            ```

#### 7.2 图片识别分布式案例改进

1. 空

----

结束





----

10米跳台决赛,日程训练

早操 1.5h

上午: 8:30 - 12:00

下午: 2:45 - 8点多

训练超过 10个小时

`硬扛`着进行下去

需要控制, 练很简单, 要到更大一点的层面, 在这条路上, 要应对伤病, 克服自己心里, 控制自己的欲望

-----

不给自己任何的理由, 任何消极的没有, 就是`一条路`: 冲, 杀, 拼, 搏, 

最后要出手, 要`搏`

主动权掌握在自己手上, 得个对手失误, 我得上

怕搏不着, 博不着就搏不着, 搏不着就输, 打不着就死, 输了就输了

那真输了他们三年以后再干

就非`拼`, 不行吗?

孙颖莎那场球好就好在就 `搏` , 对不对?

第一 摆正位置去拼对手

第二接发球 出头 出头就拉他, 出完就顶他. 我们敢不敢亮剑, 敢不敢千里绝杀. 这是关键. 那你再有实力, 如果你没有这样的信心, 没有这样的决心, 没有这样的勇气, 没有这样的无所畏惧的精神, 那你的技术都会打折扣. 日本队是一只什么队, 他们是一支攻强受弱的队伍, 进攻让他们搏出来了, 打出来, 让他们会有一些亮点, 会给我们带来很大的压力, 但是他们的防守, 他们的实力都还比较单薄, 大家可以通过备战的过程当中一点 打出我们的信心, 是我们的勇气, 是我们的气势, 让我们就是进攻, 坚持进攻, 疯狂进攻. 就这三个阶段, 只不过在进攻当中你采样什么样的方式, 是发球抢攻进攻,还是接发球直接进攻, 还是相持当中进攻, 还是攻防转换当中进攻, 但是不管怎样, 进攻将是我们今晚上的主题. 就在今夜, 就在东京, 就在这个奥运赛场, 这个时代是属于你们三个人共同来扛起的. 一个新的女乒时代

总结: 拼,搏. 实力+信心+决心+勇气+精神.

















