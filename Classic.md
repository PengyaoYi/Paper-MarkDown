# Attention is all you need
![figure 1](https://github.com/PengyaoYi/Mardown-photograph/blob/main/transformer.png)
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
![figure 2](https://github.com/PengyaoYi/Mardown-photograph/blob/main/Scaled%20Dot-Product%20Attention.png)

$e^{i \pi} + 1 = 0$
$$ f(x)= a + b \tage{2.1}$$
