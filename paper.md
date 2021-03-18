# MEDIASUM: A Large-scale Media Interview Dataset for Dialogue Summarization
提出了MEDIASUM数据集，基于NPR和CNN媒体采访数据，对话形式，使用指针网络，UniLM和BART进行实验，并研究了在AMI，ICSI，SAMSum上及逆行迁移学习的效果
# Evaluation of BERT and ALBERT Sentence Embedding Performance on Downstream NLP Tasks
探究BERT和ALBERT中句子表征的效果；结合了SENT-BERT and ALBert， 并提出了SALBERT；引入一个CNN句子表征的外部结构；上述工作在句子相似度和自然语言推理数据集实验。
# ALBERT: A LITE BERT FOR SELF-SUPERVISED LEARNING OF LANGUAGE REPRESENTATIONS
提出两种减少内存占用和增加训练速度的改进方法；提出自监督损失函数，增强句子内部的一致性，提升多句子输入的指标。
tricks：
1. 使用与NSP任务中不同的目标函数，用textual segments 代替了sentences，并且任务变为SOP sentence order prediction
2. 跨层参数共享
3. 因式分解embedding参数
其他：模型不会过拟合时 不用dropout 可以提升指标一点点；warm-start可以加快复杂网络收敛
