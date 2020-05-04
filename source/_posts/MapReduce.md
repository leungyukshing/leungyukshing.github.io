# MapReduce实验

&emsp;&emsp;本实验是MIT6.824课程的第一个实验，要求实现一个分布式的MapReduce算法。基于学术诚信理由，在这里我不会公布完整的代码，只会讲解思路。如果涉及到代码细节的讨论，请使用邮件联系本人。

<!-- more -->

实验网址：[lab website](https://pdos.csail.mit.edu/6.824/labs/lab-mr.html)

MapReduce论文：[点这里](/download/mapreduce.pdf)

---

## 实验介绍

&emsp;&emsp;实验要求实现一个分布式的MapReduce，务求达到**并发处理**task的效果。这里的并发体现在，在进行map任务和reduce任务的过程，多个worker可以并行处理。需要注意的是，**在完成所有的map任务之前，不能开始reduce的任务**。

&emsp;&emsp;实验提供了基本的框架，包括`master`、`worker`的启动程序和RPC调用的例子。从代码层面上看，我们需要实现的就是`mr/worker.go`、`mr/master.go`和`mr/rpc.go`这三个文件。

&emsp;&emsp;master主要负责的是任务的调度，包括分派任务，管理任务的状态等；worker主要负责的是完成任务（map任务或reduce任务）。master和worker直接通过RPC沟通。

---

## 实验思路

&emsp;&emsp;在这个实验中，coding并不是最重要的部分，我们应该把重点放在设计上，由于本身提供的代码很少，所以自我发挥的空间很大。这里我给出的思路是我从0开始实现的思路，不一定是最合理的思考方式，但是是我整个的思考过程。

1. 先整理出来一个完成单个task最基本的流程，暂不考虑并发、状态管理、容错的问题。

   a. worker启动，向master注册，获得一个workerID；

   b. worker向master请求一个task，master分配一个task给worker做；

   c. worker做完task后，向master汇报任务完成。

2. 通过上面这个流程，就可以整理出来几个比较重要的RPC。刚好每一个步骤对应一个调用，这些都是在master中需要实现的接口。

   a. `RegisterWorker()`：worker向master注册，获得master分配的一个workerID，用于全局识别某一个worker。

   b. `GetTask()`：worker向master请求一个task，master会分配task给worker去完成。

   c. `UpdateTask()`：worker在task完成（包括成功和失败）后，向master汇报完成情况。

3. 定义Task类型。把task定义成一个类型的好处是使得RPC和master的管理都更加方便，虽然这个是在代码层级上的内容，但是会涉及到后面对master的设计。在`GetTask()`的时候worker收到的是一个完整的Task结构，包含了完成task所需要的所有信息。

4. Worker设计。worker相对来说比较简单，因为只需要完成单个task，所以先考虑worker。

   a. worker启动后，需要先向master注册自己，只有成功注册的worker，才能继续和master联系。

   b. worker不停地向master要task来做，这里使用轮询的方法，只要worker不在处理task，就轮询。

   c. worker收到task之后，判断是map类型还是reduce类型，分开处理。处理的细节在后面会详细展开。

   d. worker完成task之后，无论成功与否，都需要向master汇报情况（RPC）。

   e. 当所有task都完成后，worker是否退出都可以，这个不是设计的重点，这里不cover。

5. Master设计。Master设计比较负责，主要涵盖这几个内容：任务状态管理、任务调度、超时检查等。

   a. 任务状态管理：任务的状态是master必须维护的信息，因为master需要知道任务做到哪一步，有谁在做，做完了没有这些信息。这类型信息我称为meta，包括状态、分配的workerID和任务开始的时间。在master里面应该有一个数组或map来记录，然后定时去更新这些meta信息。任务的状态有5个：**ready**、**queue**

   **running**、**finish**，**error**。初始化之前默认是0（ready状态），初始化之后就进入queue。在分配给worker后就改为running状态，在worker完成task后向master汇报，成功则改为finish，失败就改为error。

   b. 任务调度：任务调度是master的核心功能，目的就是把需要完成的task，逐一分发给worker。task的调度有一个不可避免的问题就是顺序问题，因为这里没有特殊的需要，所以就采取一般的FIFO队列就可以了，具体实现利用go的channel来做就行了，不需要另外使用消息队列。

   c. 超时检查：master必须要处理worker崩溃的场景。也就是说，当一个worker一段时间内没有应答时，我们就认为这个worker已经掉线，需要把这个worker正在做的任务移交给其余可用的worker。实验指导中建议是超时设置为10秒，这就需要我们不停地去检查每个任务已经进行的时间，超出10秒的就认为是失败，需要重新排队，等待分配。

## Map实现

&emsp;&emsp;实验中我们只需要实现worker中的map函数，具体的逻辑是调用方提供的，这里我们要做的就是根据task的文件名，读入文件内容，然后调用map函数后得出kv键值对，存储到中间文件中。为了读写统一，中间文件名的命名为`mr-X-Y`，X是maptask的编号，Y是reducetask的编号。

## Reduce实现

&emsp;&emsp;同样地，reduce函数的具体逻辑也是调用方提供的，我们要做的是读入中间文件，然后调用reduce方法，得出一个字符串，然后我们需要把所有的结果都连接起来，输入到一个文件中。每一个reducetask都输入一个文件`mr-out-Y`，Y是reducetask的编号。

---

## Tips

1. RPC的失败重试。在单机上做这个实验不会有问题，但如果是在真实场景下，就需要考虑网络问题带来的RPC失败问题，一般的方法是进行快重试，这里不深入讨论。

2. master处理请求的并发问题。同时可能会有多个worker向master发起请求，而任务的状态都是存放在运行时内存的，对这些变量的并发访问会带来问题。同时我们也会有轮询的函数去检查task的时间、状态等，所以我建议每一个涉及到状态信息及每一个RPC请求都加上并发锁来控制。也需要注意避免死锁。

3. 在mac上运行的话，可能需要缺少`timeout`命令。执行如下命令即可：

   ```bash
   brew install coreutils
   sudo ln -s /usr/local/bin/gtimeout /usr/local/bin/timeout
   ```

---

## 小结

&emsp;&emsp;分布式的实验更加讲求的是设计，其次才是coding，在设计过程中要尽可能地想多各种的问题，然后再结合coding的技巧去解决。在架构设计中要特别注意并发问题，因为并发带来的问题是比较难debug的。同时还要多问几个“如果这个失败了会怎么样”，这就是容错。一开始会比较难，最好不要局限自己的思维，设计好之后coding就会快速很多。这一个小实验其实细究的话，会有很多很多的技术细节可以探讨，也欢迎大家来与我探讨，谢谢！