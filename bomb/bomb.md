##bomb

题目设定感觉还是挺有趣的，这个题目需要静下心来看汇编，分析分支结构，前面5个题比较基础，看懂代码就行，后面两个题目需要结合特定的数据结构。后面2个函数稍微有点长，于是用了IDA查看汇编流程图（疯狂按住要F5的手，锻炼锻炼自己的汇编阅读能力。objdump和gdb命令还是用的不是很熟悉，有一些指令都是临时学的，还有待加强。

### phase1

![截屏2021-03-19 上午9.28.27](/Users/wangxiaocheng/Library/Application Support/typora-user-images/截屏2021-03-19 上午9.28.27.png)

0x402400 : Border relations with Canada have never been better.

###phase2

![截屏2021-03-19 上午9.56.42](/Users/wangxiaocheng/Library/Application Support/typora-user-images/截屏2021-03-19 上午9.56.42.png)

读输入放栈上，然后和1 2 4 8 16 32 逐次比较

###phase3

![截屏2021-03-19 上午10.24.20](/Users/wangxiaocheng/Library/Application Support/typora-user-images/截屏2021-03-19 上午10.24.20.png)

switch跳转，随便达成一个组合就好了

###phase3

![截屏2021-03-19 上午11.00.45](/Users/wangxiaocheng/Library/Application Support/typora-user-images/截屏2021-03-19 上午11.00.45.png)

令fun返回0即可，反推得到第一个参数为7，输入7 0

###phase5

![截屏2021-03-19 上午11.34.03](/Users/wangxiaocheng/Library/Application Support/typora-user-images/截屏2021-03-19 上午11.34.03.png)

![截屏2021-03-19 上午11.22.17](/Users/wangxiaocheng/Library/Application Support/typora-user-images/截屏2021-03-19 上午11.24.09.png)

输入的串和0x0f取与运算，然后作为array的下标取字符，与flyers相比，输入ionefg

###phase6

![截屏2021-03-19 上午11.45.02](/Users/wangxiaocheng/Library/Application Support/typora-user-images/截屏2021-03-19 上午11.45.02.png)

直接IDA了，觉得可以更直观的看到数据结构吧，用第一个数据进行排序，从大向小，输入的6个数为第二个数据id。

###phase secret

意外在function里发现一个fun7，于是查看了一下字符串，交叉引用发现phase_defused中发现隐藏关卡。

![截屏2021-03-19 下午2.34.12](/Users/wangxiaocheng/Library/Application Support/typora-user-images/截屏2021-03-19 下午2.34.12.png)

需要fun7返回2，观测fun7函数结构与n1数据分布猜测此处的数据结构类型为二叉树，返回2的方式为2*（1+（2 * 0）），输入节点n32的值即可达成。

![截屏2021-03-19 下午3.03.04](/Users/wangxiaocheng/Library/Application Support/typora-user-images/截屏2021-03-19 下午3.03.04.png)

![截屏2021-03-19 下午3.02.25](/Users/wangxiaocheng/Library/Application Support/typora-user-images/截屏2021-03-19 下午3.02.25.png)