---
title: CPU、GPU学习
date: 2023-02-12
type: 系统资源
---

# CPU、GPU学习

# CPU

​	CPU (central processing unit) 是计算机的大脑，它提供了一套指令集，我们写的程序最终会通过 cpu 指令来控制的计算机的运行。

## 指令周期

​	CPU会对指令进行译码，然后通过逻辑电路执行该指令。整个阶段分为5个步骤：

- 取指令

  从 PC 寄存器里找到对应的指令地址，根据指令地址从内存里把具体的指令，加载到指令寄存器中，然后把 PC 寄存器自增，好在未来执行下一条指令。

- 译码

  根据指令寄存器里面的指令，解析成要进行什么样的操作，是 R、I、J 中的哪一种指令，具体要操作哪些寄存器、数据或者内存地址。

- 执行

  实际运行对应的 R、I、J 这些特定的指令，进行算术逻辑操作、数据传输或者直接的地址跳转。

- 取数

- 写回

  

  以上5个步骤为一个指令周期，CPU会不断重复当前周期以完成各种任务。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=MzM1MjA4Y2I2ZDAwNWNjMDBkMjExNzRiOGU4NWVmYjlfMkxTWlMzQ3NFV21ySDdvaFBpVnlaUHFOV1h2eFV2dkRfVG9rZW46Ym94azRUaXk0Y25lRzM0RFV4dW5LcGVzenVkXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

## CPU缓存

### 读取缓存

​	指令和数据都会首先加载到内存中，在程序运行时依次取到cpu里。cpu访问内存虽然比较快，但比起 cpu 执行速度来说还是比较慢的，为了避免出现CPU的计算因为上一个结果未输出而出现滞留，在CPU内部开辟空间用于存储运算结果，这片空间叫做缓存。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=YmI0ZjlmNjBjZTk0ZDE1OTgwNTViYjExZWM2ODQ1NDNfRGJNbnFCZnNoM1NUQlhnaDJhVVpmQm5YYTd4elBOd2JfVG9rZW46Ym94azRNUDFkR3hCWTBjSU1PWWdqWUJPMzlmXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

​	CPU设计了 3 级缓存，也就是 L1、L2、L3 的缓存（查询优先级由高到低），缓存未命中时才会在内存查询；但其实查到内存并未结束，内存也是一层cache，是磁盘的cache，如果内存无法命中，就需要到磁盘中查询。

​	多核 cpu 各核心都有自有独立的 L1、L2 缓存，然后共享 L3 缓存，这 3 级缓存容量是逐渐递增的，但是速度是逐渐下降的，但是也会比访问内存快一些。有了这 3 级缓存以后，cpu 执行速度和访问内存速度的矛盾就可以得到缓解，不需要一直访问内存，cpu 每次会加载一个缓存行，也就是 64 字节大小的数据到缓存中；这样访问临近的数据的时候就可以直接访问缓存。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=MTkzMGRmMDQ1ZDlkODhhOGQ1M2JjNDhkYTBiY2RjNzdfV1VwaTM1ckV1b0JiWnZ6QUxOMEFGUlhya3pOSjZKRjZfVG9rZW46Ym94azRBN3hYVmpkSGVLdlhXT0twQ1o3aG1kXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

​	这里提到一个词:“临近访问的数据”，这就涉及到程序的局部性原理，优质代码往往都具备良好的局部性。

### 程序局部性原理

​	这个原理是说程序在执行时呈现出局部性规律，即在一段时间内，整个程序的执行仅限于程序中的某一部分；相应地，执行所访问的存储空间也局限于某个内存区域，具体来说，局部性通常有两种形式：时间局部性和空间局部性。

时间局部性：被引用过一次的存储器位置在未来会被多次引用（通常在循环中）。

空间局部性：如果一个存储器的位置被引用，那么将来他附近的位置也会被引用。

​	我们再次回到CPU三级缓存，越靠近CPU核心的缓存，其访问速度越快。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=NjRkYjBiZGVhMjM2ZjgwMDkxYzJmODM4NDdjOTY1NTRfeHRwTmY4aHVqWTE2SldTMGNkdjRXeE15NHByMDZ4SU9fVG9rZW46Ym94azRQRE1JeVV2Rlh6TmpHdWQzTkVCTzZmXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

> 时钟周期：这里是指CPU主频的倒数，比如2GHZ主频的CPU，它的一个时钟周期为0.5ns

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=OTAwZjYxOWQ3NTE3NGFiYWE1MGE0ODg4OThkNWIyZTlfVnphN0R1UzBGRkt1YmptcEdzOWx5ZEI5TnpEdDIwZHlfVG9rZW46Ym94azRtWmxCTWxrU2pGZWJxYU9sT1FRT3loXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

​	这里从上到下依次为L1数据缓存、L1指令缓存、L2数据缓存和L3数据缓存。CPU缓存是从内存中读取的，它是按照固定大小读取数据的，这样固定大小的数据，称为cache_line（缓存块），是CPU从内存读取数据的基本单位。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=NDZlMThiYzU4YzRlYzhhYzJiZjhhZWI4MTgxOGNlZmJfYWFrMkVLeUlWR1VRbDdxS00zbU02NUxYZXJvQVNyNXlfVG9rZW46Ym94azRkZGYxTzJIUm9UZ0NtdmVBeFNkZG9nXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

> 举个例子：
>
> 有一个 int array[100] 的数组，当载入 array[0] 时，由于这个数组元素的大小在内存只占4字节，不足 64 字节，CPU就会顺序加载数组元素到 array[15] ，意味着 array[0]~array[15] 数组元素都会 被缓存在 CPU Cache 中了，因此当下次访问这些数组元素时，会直接从 CPU Cache 读取，而不用再从内 存中读取，大大提高了 CPU 读取数据的性能。

​	CPU cache由多个cache_line组成，每个cache_line则是由各种标志（Tag）+数据块（Data block）组成。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=YWU3Mjc2MjVlM2MxYzY3ZTQwYjgzNDU4NGRjMzEzNjdfRzJEaGlCQ1QzSUZsUGJCMElueHlFVHRoOHJEM0dzWDNfVG9rZW46Ym94azRkV3ZFN2M5TkRrR0lrcU5XOFZwd2dmXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

### 写入缓存

​	当cpu将数据写入cache后，cache和内存中的数据就不同了，此时需要将cache中的数据同步到内存中去；前面已经说过，“cpu执行速度比起访问内存的速度要慢”，那么cpu需要将数据从缓存写入到内存中的时机就显得很重要了；一般有两种：

- 写直达-Write Through

  保证缓存和内存中数据一致最简单的方式就是：同时写入；将数据同时写入cache和内存中，这种方式就叫“写直达”（Write Through）。

![image-20230212202342690](C:\Users\Mithrandir\AppData\Roaming\Typora\typora-user-images\image-20230212202342690.png)

​	这种方式比较直观，但是从CPU性能的角度来看，这种方式避开了缓存对写入操作的优化，对CPU性能影响较大。

- 写回

  当CPU发生“写”的操作时，新数据仅仅被写入到cache block中；只有当修改过的cache block再次被修改时，才将数据写入到内存中。这样的好处是，如果我们大量的操作都能够命中缓存，那么大部分时间里 CPU 都不需要读写内存，自然性能相比写直达会高很多。

暂时无法在飞书文档外展示此内容

### 缓存一致性

TODO：补充

总线嗅探

MESI

## MMU

# GPU

图形处理器，GPU的出现在于解决3D图形渲染，3D画面是由多边形组合而来的，这些画面的变化需要计算机根据图形学的各种计算实时渲染。图形渲染的整个过程可被分为5个步骤：

## 图形渲染过程

### 顶点处理（Vertex Processing）

图形渲染的第一步是顶点处理。构成多边形建模的每一个多边形呢，都有多个顶点（Vertex）。这些顶点都有一个在三维空间里的坐标。但是我们的屏幕是二维的，所以在确定当前视角的时候，我们需要把这些顶点在三维空间里面的位置，转化到屏幕这个二维空间里面。这个转换的操作，就被叫作顶点处理，这样的转化都是通过线性代数的计算来进行的。可想而知，建模越精细，需要转换的顶点数量就越多，计算量就越大。而且，这里面**每一个顶点位置的转换，互相之间没有依赖，是可以并行独立计算的**。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=MWE1NWYxNjUwYTU1YTJjZmY4M2RkY2E1N2UyZTYwZThfR0RjTHd1WjBxSGNLQnozakowNTFJZlJwOVplUWNWWm1fVG9rZW46Ym94azQydW9KVlROOUJRRk11dW81WjJGaG1kXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

### 图元处理（Primitive Processing）

图元处理是把顶点处理完成之后的各个顶点连起来，变成多边形。其实转化后的顶点，仍然是在一个三维空间里，只是第三维的 Z 轴，是正对屏幕的“深度”。所以我们针对这些多边形，需要做一个操作，叫剔除和裁剪（Cull and Clip），也就是把不在屏幕里面，或者一部分不在屏幕里面的内容给去掉，减少接下来流程的工作量。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=ODUxYjQyN2JlZTM5YWZlNmZiOTk5NTk0OWExZGEzMzRfODlrUTJVRXZhMEpZUXF0TnBUMEFkWDBuWTB0eEt3S1VfVG9rZW46Ym94azRjb1JoV3FXa0JxSDhMbW1zSFpWbHN4XzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

### 栅格化（Rasterization）

因为我们使用的屏幕分辨率是有限的：它一般是通过一个个“像素（Pixel）”来显示出内容。所以，需要把多边形转换成屏幕里面的一个个像素点，这个操作叫作栅格化，这里**每一个图元都可以并行独立地栅格化**。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=NWYwMDcyNGE2OGNhZTAyZGQ2NzE1YzVjM2QzMjE3ZmRfbFdYb3IxaUlMc2JBaWxMUHNtTFJ4QTFoM0NJZmk3NWJfVG9rZW46Ym94azRzVzJ5WE4yTEljMGY0d09MNXVMdVpkXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

### 片段处理（Fragment Processing）

在栅格化变成了像素点之后，此时图还是“黑白”的。我们还需要计算每一个像素的颜色、透明度等信息，给像素点上色。这步操作叫做片段处理，同样也可以**每个片段并行、独立进行**，和上面的顶点处理和栅格化一样。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=YmFmMmUzZjYxYjMyMTMxMjA0MTBjZjA4NjdhYmFlY2RfMnN5ckxlZ0ZOeW5BUHlQS1o4MHFTRlpQU1JLbkZ2R2hfVG9rZW46Ym94azRwcGVIdG81WGxQdWpRQ0RnTjhHVnBmXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

### 像素操作（Pixel Operations）

最后一步把不同的多边形的像素点“混合（Blending）”到一起。可能前面的多边形可能是半透明的，那么前后的颜色就要混合在一起变成一个新的颜色；或者前面的多边形遮挡住了后面的多边形，那么只要显示前面多边形的颜色就好了。最终，输出到显示设备。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=ZjE2N2IxYWNhNWM2ZmU4NzAzMGI0N2M0MzY0N2ZiMWJfaUt6bXZ4emREQWg2ZXpKT01HSWFYMFdVZ29vWHFDaFNfVG9rZW46Ym94azQyQ3dJNzh0eGgxSk00RmVSdXRBcFVmXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

经过这完整的 5 个步骤之后，就完成了从三维空间里的数据的渲染，变成屏幕上可以看到的 3D 动画了。这样 5 个步骤的渲染流程一般也被称之为图形流水线（Graphic Pipeline）。

**GPU****的出现是因为****CPU****性能不足以实时计算出****3D****画面的数据。**在一块分辨率为640×480的屏幕上（也就是说有30w像素），如果按照画面为60帧进行计算的话，也就是说每秒需要渲染1800w次单个像素的渲染。从栅格化开始，每个像素有 3 个流水线步骤，即使每次步骤只有 1 个指令，那我们也需要 5400 万条指令，也就是 54M 条指令。93 年出货的第一代 Pentium 处理器，主频是 60MHz，后续逐步推出了 66MHz、75MHz、100MHz 的处理器。以这个性能来看，用 CPU 来渲染 3D 图形，基本上就要把 CPU 的性能用完了。因为实际的每一个渲染步骤可能不止一个指令，换言之，CPU 可能根本就跑不动这样的三维图形渲染。

> [CPU频率](https://xiaomi.f.mioffice.cn/docx/doxk46a0pLdeLKtYrx1dwhPjcGd) 

既然图形渲染的流程是固定的，那可以直接用硬件来处理这部分过程，不用 CPU 来计算，这样的硬件会比制造有同样计算性能的 CPU 要便宜得多。因为整个计算流程是完全固定的，不需要流水线停顿、乱序执行等等的各类导致 CPU 计算变得复杂的问题；同样不需要有什么可编程能力，只要让硬件按照写好的逻辑进行运算就好了。

## 图形渲染过程主体变更史

- Voodoo FX这样的图形加速卡登上了历史舞台。那个时候，整个顶点处理的过程还是都由 CPU 进行的，不过后续所有到图元和像素级别的处理都是通过 Voodoo FX 或者 TNT 这样的显卡去处理的。不过，无论是 Voodoo FX 还是 NVidia TNT。整个显卡的架构还不同于我们现代的显卡，也没有现代显卡去进行各种加速深度学习的能力。这个能力，要到 NVidia 提出 Unified Shader Archicture 才开始具备。

- 1999 年 NVidia 推出的 GeForce 256 显卡，就把顶点处理的计算能力，也从 CPU 里挪到了显卡里。不过，这对于想要做好 3D 游戏的程序员们还不够，即使到了 GeForce 256，整个图形渲染过程都是在硬件里面固定的管线来完成的。

- 从2001年的Direct3D 8.0开始，微软第一次引入了可编程管线（Programable Function Pipeline）的概念。一开始的可编程管线，仅限于顶点处理（Vertex Processing）和片段处理（Fragment Processing）部分。这个时候的 GPU，有两类 Shader，也就是 Vertex Shader 和 Fragment Shader。在进行顶点处理的时候，操作的对象是多边形的顶点；在片段操作的时候，操作的对象是屏幕上的像素点。对于顶点的操作，通常比片段要复杂一些。所以一开始，这两类 Shader 都是独立的硬件电路，也各自有独立的编程接口。因为这么做，硬件设计起来更加简单，一块 GPU 上也能容纳下更多的 Shader。虽然顶点处理和片段处理上的具体逻辑不太一样，但是里面用到的指令集可以用同一套。而且，虽然把 Vertex Shader 和 Fragment Shader 分开，可以减少硬件设计的复杂程度，但是也带来了一种浪费，有一半 Shader 始终没有被使用。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=YjQwNWQzYTA3MjY3MjBkYjBiNDIwYzMxODY1Zjg0N2JfMWtSbTVnYms0TnVWdnIzc2Zud3V1b0x5dnpNV0RHc0xfVG9rZW46Ym94azRaQmxCSnA4aVBUOVJQMU1lVlJia3ZmXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

- 既然顶点逻辑和片段处理的指令集可以使用同一套，那不如就在 GPU 里面放很多个一样的 Shader 硬件电路，然后通过统一调度，把顶点处理、图元处理、片段处理这些任务，都交给这些 Shader 去处理，让整个 GPU 尽可能地忙起来；在这种情况下，统一着色器架构（Unified Shader Architecture）就应运而生。

![img](https://xiaomi.f.mioffice.cn/space/api/box/stream/download/asynccode/?code=MTc5MjQ0NGNmOGU1NTdiOTdmMzA4OThmMjBiNGMxYzZfYWw5Yk5sdjBKN3dwRzA4Y2JwNUpLRFJZeVBUN2tKT2xfVG9rZW46Ym94azRzbXlCVVlKbEZ5RnR5TjJ6ZkkxdVVkXzE2NzYyMDQ1Mzk6MTY3NjIwODEzOV9WNA)

正是因为Shader 变成一个“通用”的模块，才有了把 GPU 拿来做各种通用计算的用法，也就是 GPGPU（General-Purpose Computing on Graphics Processing Units，通用图形处理器），而正是因为 GPU 可以拿来做各种通用的计算，才有了深度学习的火热。

## 现代GPU的三个核心创意

### 芯片瘦身

### 多核并行和 SIMT

### GPU 里的“超线程”

> 参考资料：
>
> https://www.zhihu.com/question/55854401/answer/2292909472
>
> https://juejin.cn/post/7001634685927292936
>
> 
>
> https://time.geekbang.org/column/article/104747
>
> https://zhuanlan.zhihu.com/p/34675934
>
> https://imgtec.eetrend.com/d6-imgtec/blog/2018-07/16908.html
>
> https://taaang.github.io/blog/cpu/2020/11/12/cpu-1-freq/
>
> http://kernel.meizu.com/cpufreq-sched.html
>
> 
>
> http://tianyu-code.top/Linux%E5%86%85%E6%A0%B8/cpufreq/