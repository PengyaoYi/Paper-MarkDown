# MEDIASUM: A Large-scale Media Interview Dataset for Dialogue Summarization
提出了MEDIASUM数据集，基于NPR和CNN媒体采访数据，对话形式，使用指针网络，UniLM和BART进行实验，并研究了在AMI，ICSI，SAMSum上及逆行迁移学习的效果
# Evaluation of BERT and ALBERT Sentence Embedding Performance on Downstream NLP Tasks
探究BERT和ALBERT中句子表征的效果；结合了SENT-BERT and ALBert， 并提出了SALBERT；引入一个CNN句子表征的外部结构；上述工作在语义相似度和自然语言推理数据集实验。

1. [CLS] token
2. Pooled token 
3. Sentence-BERT
4. Sentence-ALBERT
5. CNN-SENTBERT
6. CNN-SALBERT

### 结果

[CLS]的皮尔森系数和斯皮尔曼系数<pooled<sentence bert

孪生网络结构在sentence embedding上有很好的效果

CNN结构提升了8个点（斯皮尔曼系数）

# ALBERT: A LITE BERT FOR SELF-SUPERVISED LEARNING OF LANGUAGE REPRESENTATIONS
提出两种减少内存占用和增加训练速度的改进方法；提出自监督损失函数，增强句子内部的一致性，提升多句子输入的指标。
tricks：

1. 使用与NSP任务中不同的目标函数，用textual segments 代替了sentences，并且任务变为SOP sentence order prediction
2. 跨层参数共享
3. 因式分解embedding参数
其他：模型不会过拟合时 不用dropout 可以提升指标一点点；warm-start可以加快复杂网络收敛

# Entity-level Factual Consistency of Abstractive Text Summarization
提出了一种在生成式摘要中衡量实体层面事实一致性的评估方法；提出一个针对摘要的实体分类任务作为一个额外的任务以及摘要生产的方法

刚读完摘要

# Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks



# RoBERTa: A Robustly Optimized BERT Pretraining Approach

研究BERT中超参数的影响，认为BERT训练还不够充分。用了一个新的数据集CC-News;证明了MLM足够厉害

改进：

1. 用更大的batches和更多数据训练更长的时间
2. 取消NSP任务
3. 用更长的序列进行训练
4. 动态调整mask机制

