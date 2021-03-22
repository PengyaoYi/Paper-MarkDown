# LSTM RNN CNN

#### RNN

$$
h' = \sigma(W^hh+w^ix)\\
y=\sigma(w^oh')(softmax)
$$

#### LSTM

解决RNN中长序列训练过程中梯度爆炸和梯度消失问题

![](https://github.com/PengyaoYi/Mardown-photograph/blob/main/lstm.png?raw=true)
$$
C_t = f_t*C_{t-1}+i_t*\widetilde{C}_t\\
 =\sigma(W_f[h_{t-1},x_t]+b_f)*C_{t-1}+\sigma(W_i[h_{t-1},x_t]+b_i)*tanh(W_c[h_{t-1},x_t]+b_c)
 \\
 h_t=o_t*tanh(C_t)\\
 =\sigma(Wo[h_{t-1},x_t]+b_o)*tanh(C_t)
$$

#### GRU

![](https://github.com/PengyaoYi/Mardown-photograph/blob/main/gru.png/?raw=true)

reset gate :
$$
r=\sigma(W^r[x_t,h_{t-1}])
$$
update gate:
$$
z=\sigma(W^z[x_t,h_{t-1})
$$
通过重置门获得重置后的数据：
$$
h^{t-1'}=h^{t-1}\bigodot r\\
h'=tanh(W[h^{t-1'},x_t])
$$
最后更新记忆
$$
h^t=(1-z)\bigodot h^{t-1}+z\bigodot h'
$$

#### CNN

##### 稀疏交互性

卷积核尺度小，仅与输出层部分连接并产生交互。可以学习到局部特征

##### 参数共享

同一个模型的不同模块中使用相同的参数。卷积运算中的参数共享让网络只需要学一个参数集合，而不是对于每一位置都需要学习一个单独的参数集合。

参数共享的物理意义：使得卷积层具有平移等变性。在第三个特点中会谈到。

##### 等变表示

任何平移变换不会改变输出结果

#### TextCNN

卷积神经网络的核心思想是**捕捉局部特征**，对于文本来说，局部特征就是**由若干单词组成的滑动窗口**，类似于N-gram。卷积神经网络的优势在于能够**自动地对N-gram特征进行组合和筛选，获得不同抽象层次的语义信息**。

在NLP中输入层的"image"是一个由词向量拼成的词矩阵，且卷积核的宽和该词矩阵的宽相同，该宽度即为词向量大小，且卷积核只会在高度方向移动。