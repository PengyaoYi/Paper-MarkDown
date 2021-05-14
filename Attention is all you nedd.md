# Attention is all you need
![figure 1](https://github.com/PengyaoYi/Mardown-photograph/blob/main/transformer.png?raw=true)
### 基本结构
encoder-decoder结构。模型层面是一个seq2seq with Attention的模型
#### encoder端decoder端总览
+ Encoder：由N=6个相同模块组成，每个模块由两部分组成，分别是Multi-Head Self-Attention和前馈神经网络
+ Decoder：由N=6个相同模块组成，每个模块由三个部分组成，分别是Multi-Head Self-Attention；前馈神经网络和多头Encoder-Decoder Attention 

**其中最底层的模块训练时和测试时接受的数据是不同的** 

底层训练和测试时的输入为：
+ 训练：每次的输入为上次的输入序列加上输入序列向后移一位的 ground truth(例如每向后移一位就是一个新的单词，那么则加上其对应的 embedding)，
特别地，当 decoder 的 time step 为 1 时(也就是第一次接收输入)，其输入为一个特殊的 token，例如</s>起始符，也可能是其它视任务而定的输入等等，不同源码中可能有微小的差异，
其目标则是预测输出序列第一个位置的单词(token)是什么，Shifted right的原因；解码过程中第一个是</s>用于初始化输入，整体序列相当于整体右移一位。实际过程中不会动态输入二十使用mask机制
+ 测试：先生成第一个位置的输出。然后在第二次预测时将其加入到输入序列，以此类推直至结束。
### Encoder子模块
#### Multi-Head Self-Attention
![figure 2](https://github.com/PengyaoYi/Mardown-photograph/blob/main/Scaled%20Dot-Product%20Attention.png?raw=true)


$$
Attention\_Output=Attention (Q,K,V)  \\
Attention(Q,K,V) = softmax(\frac{QK^T}{\sqrt{d_k}})V
$$

##### Attention 小结

1. Attention分类：
   + 根据计算区域：
     + Soft Attention:对所有key求权重概率，每个key都有一个对应的权重，是一种全局的计算方式（也可以叫Global Attention）。这种方式比较理性，参考了所有key的内容，再进行加权。但是计算量可能会比较大一些。
     + Hard Attention: 这种方式是直接精准定位到某个key，其余key就都不管了，相当于这个key的概率是1，其余key的概率全部是0。因此这种对齐方式要求很高，要求一步到位，如果没有正确对齐，会带来很大的影响。另一方面，因为不可导，一般需要用强化学习的方法进行训练。（或者使用gumbel softmax之类的）
     + Local Atteniton: 以上两种方式的一个折中，对一个窗口区域进行计算。先用Hard方式定位到某个地方，以这个点为中心可以得到一个窗口区域，在这个小区域内用Soft方式来算Attention。
   + 根据所用信息：
     + General Attention：利用外部信息，常用于需要构建两段文本关系的任务，query一般包含了额外信息，根据外部query对原文进行对齐。比如在阅读理解任务中，需要构建问题和文章的关联，假设现在baseline是，对问题计算出一个问题[向量](https://easyai.tech/ai-definition/vector/)q，把这个q和所有的文章词向量拼接起来，输入到[LSTM](https://easyai.tech/ai-definition/lstm/)中进行建模。那么在这个模型中，文章所有词向量共享同一个问题向量，现在我们想让文章每一步的词向量都有一个不同的问题向量，也就是，在每一步使用文章在该步下的词向量对问题来算attention，这里问题属于原文，文章词向量就属于外部信息。
     + Local Attention：这种方式只使用内部信息，key和value以及query只和输入原文有关，在self attention中，key=value=query。既然没有外部信息，那么在原文中的每个词可以跟该句子中的所有词进行Attention计算，相当于寻找原文内部的关系。

##### Multihead

使用h个不同线性变换对Q,K,V进行投影，然后把结果拼接起来。
$$
MultiHead(Q,K,V)=Concat(head_1,...,head_h)W^O
$$

##### Scaled dot-product

即$ \frac{QK^T}{\sqrt{d_k}} $  ，其中点积运算会随着向量维度增加，导致权重增加，

> 极大的点积值将整个 softmax 推向梯度平缓区，使得收敛困难。  

而softmax函数对元素数量级很敏感，如果数量级都很大，softmax结果会接近1 导致梯度消失，为了提升计算效率，防止数据上溢，对其进行Scaling。除了点积，还可以通过多层感知机、或权重矩阵$q^TWk$ 来计算注意力权重



#### Position-wise feed-forward networks

提供非线性变换



### Decoder模块

Decoder和Encoder的结构差不多，但是多了一个attention的sub-layer，这里先明确一下decoder的输入输出和解码过程：

+ 输出：对应i位置的输出词的概率分布
+ 输入：encoder的输出 & 对应i-1位置decoder的输出。所以中间的attention不是self-attention，它的K，V来自encoder，Q来自上一位置decoder的输出
+ 解码：这里要注意一下，训练和预测是不一样的。在训练时，解码是一次全部decode出来，用上一步的ground truth来预测（mask矩阵也会改动，让解码时看不到未来的token）；而预测时，因为没有ground truth了，需要一个个预测。



#### Positional Encoding

$$
PE_{(pos,2i)}=sin(pos/10000^{2i/d_{model}}) \\
PE_{(pos,2i+1)}=cos(pos/10000^{2i/d_{model}})
$$

如果直接pos=encoding则编码会随着位置增大而增大，导致词向量本身丢失意义，因此需要把pos限定在一定范围内。并且不同长度文本，其pos位置差值应相同，所以使用周期函数

