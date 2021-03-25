# XLNet

**自回归语言模型**

例如ELMO/GPT

优点：适合生成类NLP任务，如摘要生成，机器翻译。

缺点：不能同时利用上下文信息

**自编码语言模型**

例如Bert 降噪自编码器

优点：融入双向语言模型，同时看到被预测单词的上下文信息

缺点：由于训练阶段引入了[mask]标记，导致预训练和Fine tuning阶段不一致，产生了预训练-微调差异

## XLNet模型

排列语言模型

XLNet看上去仍然是从左到右的语言模型，但是通过对句子中的单词排列组合，把一部分预测词下文的单词排列在预测词的上文位置中，就同时看到了上文和下文的信息，但是形式上仍然是从左到右预测一个单词。XLNet还解决了Bert中由于[Mask]的词是条件独立的，而实际上并不是，XLNet考虑了这种关系

从序列x的所有排列序列采样一个z，然后根据这个排列序列来分解联合概率成条件概率的乘积，然后加起来。

**结构**

1. 双流注意力

   内容隐状态h，标准transformer

   查询隐状态g，不包含当前时刻的内容信息

2. Transformer-XL的相对编码方案和分段递归机制

   transformer-XL

   vanilla model：将长序列分段处理，放弃段和段之间的上下文信息，缺点是文本信息受限于segment的长度，另外segments破坏句子的完整性导致预测前几个符号时缺少足够的信息，被称为context fragmentation

   分段递归：针对普通版Transformer语言模型在训练阶段，各个segment缺乏联系的缺点，Transformer-XL给出的解决方法是在segment之间引入RNN机制，具体而言，就是把上一个segment计算好的hidden state进行存储，然后在计算下一个segment的时候，将上一个segment的这些信息作为一个context融入到当前segment的计算当中。下式中的n属于Transformer层数，\tau*τ*表示上一个segment，简单而言就是会把上一个segment的hidden state沿着句子长度的方向与当前segment的hidden state进行concat，因此比如每个segment长度为4，那么concat之后长度就为8，然后再在这个concat过后的长度上进行Transformer Decoder操作，因此如果没有这种操作的话，那么每一个segment的开始第一个词实际上它的context信息只有第一个时刻的输入，而此时则能带上上一个segment的后三个词。用这种方法能够建模得到的最长依赖为N * L，N是Transformer的层数，L是每个segment的长度。这里的原因是每一层都可以往前建模L的依赖，累积N层后，最长的依赖关系便是N * L，这体现在上图中的右半部分。

   实际上，因为上一个segment的hidden state是会存储起来复用的，因此这部分就不再需要重复计算，论文中提到通过这种办法可以加速1800倍，此外，还有一点是因为是存储的，理论上可以存储到更多segment的hidden state从而加以利用，所以如果是这样的话，能够建模到更长的依赖关系。

   相对编码：对于普通的Transformer而言，引入了一个positional embedding，如果在recurrence-level segement中也同样使用这样的absolute position embedding的话可能会有个问题，如下式所示，因为这两个segment使用的是同一个，那么可能会导致模型无法区分这两个segment的相对位置。