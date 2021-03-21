# LSTM RNN

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

