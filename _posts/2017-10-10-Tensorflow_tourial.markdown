##tensorflow简明教程

----------
####tensorflow是一个采用数据流图(data flow graphs)，用于数值计算的数学开源软件库
**一般而言，使用Tensorflow程序的流程是先创建一个图，然后在session中启动它**  

- Graph: 表示计算任务   
- Node:  表示在图中的数学操作  
- tensors:表示数据（多维数组）  
- edges:  表示在节点之间相互联系的多维数据数组  
- Variable: 表示变量（可以通过变量来维护状态）
- feed&fetch: 可以为任意的操作赋值或者从中获取数据 

一个tensor看做是一个n维的数组或者列表， 一个tensor包含一个静态类型rank，和一个shape， 在计算图中，操作间传递的数据都是tensor。

    rank:在Tensorflow系统中tensor的维数被描述为阶(rank)，但是tensor的rank和矩阵的rank不是一个概念。Tensor Rank的意义看起来更像是维度，比如Rank = 1就是向量，Rank = 2就是矩阵了。（Rank = 0就是一个值了）---- 一阶张量可以认为是一个向量.对于一个二阶张量你可以用语句t[i, j]来访问其中的任何元素.而对于三阶张量你可以用't[i, j, k]'来访问其中的任何元素. 2阶张量(矩阵) m = [[1, 2, 3], [4, 5, 6], [7, 8, 9]] 3阶张量(立体)t = [[[2], [4], [6]], [[8], [10], [12]], [[14], [16], [18]]] 一个中括号代表一个维度
    Shape:就是Tensor在各个维度上的长度组成的数组，譬如Rank = 0的Tensor Shape = []（因为没有维度嘛），Rank = 1的Tensor Shape = [ a ]，Rank = 2的Tensor Shape = [ a, b ]这样。  
	  type:Type就是Tensor中每一个元素的数据类型了，基本上就是不同精度的整型、浮点型以及布尔型等。


> 综上所述，`Tensorflow` 是一个编程系统，使用图来表示计算任务，图中的节点称之为`op`（operation的缩写）一个`op`获得0个或者多个`Tensor`,每一个`Tensor` 是一个类型化的多维数组，例如，你可以将一小组图像表示为一个四维浮点数数组，这四个维度分别是[batch, height, width, channels].  
>一个`Tensorflow`图描述了计算的过程，为了进行计算，图必须在`会话(session)` 中被启动， 会话会将图中的op分发到诸如CPU或者GPU之类的设备上，同时提供了执行op的方法，这些方法执行后，将产生的tensor返回，在Python中，返回的tensor是numpy的ndarray对象。


**In tensorflow computations are done with data flow graphs. In these graphs, Nodes represent mathematical operations, while the edges represent the data**
