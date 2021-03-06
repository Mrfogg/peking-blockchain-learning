## btc数据结构

### 哈希指针

区块链和普通链表的区别在于，用哈希指针代替了普通的指针。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200214173245117.png)

后边的区块，保存上一个区块整体作为输入的哈希值。

这种数据结构可以实现**tamper evident log**

```
如图中所示，如果我们想要破坏区块链完整性。篡改B的内容，而C中保存有B的哈希值，所以C也得进行修改。而同样C后区块也得修改。而用户只需要记住最后一个区块链的哈希地址，就可以检测区块链上内容是否被篡改。
在实际应用中，一整条链可能会被切断分开保存在多个地方。若用户仅仅具有其中一段，当用到前面部分区块数据时，直接问系统中其他节点要即可，当要到之后，仅仅通过计算要到的最后一个哈希值和自己保存哈希值是否一致可以判断所给内容是否确实为区块链上真实的内容。

```

### merkle tree

每个区块的交易组织成merkle tree的形式，叶子节点ABCD代表一笔交易
![image](https://user-images.githubusercontent.com/7734816/149287495-d39112dd-ab0e-41be-9079-f74dc0ca724f.png)

上图即为一个简单的Markle Tree，其中A、B、C、D为数据块。可见，A和B各有一个哈希值，将其合并放在一个节点中，C和D同样操作，而后，针对得到的两个节点分别取哈希，又可以得到两个新的哈希值，即为图中根节点。实际中，在区块块头中存储的是根节点的哈希值（对其再取一次哈希）。

该数据结构的优点在于：只需要记住Root Hash（根哈希值），存储在block header里，便可以检测出对树中任何部位的修改.
例如，所绘制Markle Tree中节点B发生了改变，则对应的第二层第一个节点中第二个哈希值便也会发生改变，进而根节点中第一个哈希值也会发生改变，从而导致根哈希值也发生了改变。



BTC一个区块有block header和block body。

**比特币中节点分为**轻节点**和**全节点**。全节点保存整个区块的所有内容，而轻节点仅仅保存区块的块头信息**。手机上的BTC钱包，就是轻节点，只保存block header.

### Markle Proof

那么如何向轻节点证明某个交易已经写入到区块链，这用到

![image-20220113114757985](/Users/trh/Library/Application Support/typora-user-images/image-20220113114757985.png)

轻节点向某个全节点发送请求，全节点将标为红色的哈希值发给轻节点，然后轻节点在本地计算图中标为绿色的哈希值。轻节点把根哈希值与block header中的哈希值进行比较，就可以确认tx是否在区块链里。

那么如何证明某个交易不在区块链里呢？即**proof of non-membership**

