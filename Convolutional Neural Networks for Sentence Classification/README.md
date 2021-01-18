# Convolutional Neural Networks for Sentence Classification

## 目的

基于一个无监督模型词向量后使用CNN提取文本特征及构建文本分类器。  

## 贡献

首次将CNN应用于Text Classification任务上。  

尝试两种模型变化：维持原先的word vectors以及通过反向传播finetune word vectors。

正则化：使用基于白努力分布为概率的Dropout与L2正则

## 步骤

1. 基于预训练向量模型（在本文实验中使用100百万字的基于Google News作为语料以及word2vec的CBOW方法训练一维度为300的预训练模型），若没有出现在预训练向量中的词则随机初始化后作为该词的向量  
2. 将各个字向量或词向量依序拼接后作为句向量  
3. 使用大小为h的划窗依序提取文本向量  
4. 将提取的文本向量分别套用max pooling作为该划窗的特征，以获取每个feature map中最重要的特征  
5. 一个特征由一个filter获取，即可以使用多重filter（不同的划窗窗口大小）获取更多特征  
6. 最后再接上一个全连接的Softmac层作为各个标签概率的判别器。



## 调参

1. 使用rectified linear units  
2. filter windows of 3, 4, 5 with 100 feature maps each
3. dropout rate of 0.5
4. L2 constraint of 3
5. Mini-batch size of 50
6. 在实验数据中没有使用early stop
7. 自训练集中随机抽取10%数据作为开发集
8. training is done through stochastic gradient descent over shuffled mini-batches with the Adadelta update rule.



## 实验

1. CNN-rand：baseline，所有字向量皆为随机初始化的
2. CNN-static：使用基于word2vec方法训练的预训练向量，若没有出现在该训练语料中的词则随机初始化
3. CNN-non-static：与static相同，皆使用预训练向量，但在每个任务中fine tune预训练的向量
4. CNN-multichannel：同时使用两种基于word2vec训练出的预训练向量，但其中一个后续会随着任务fine tune，一个不会（这里不太确定）



## 小结

1. CNN-rand表现不理想
2. 要不要fine tune预训练的向量在这几个实验数据集上并不显著
3. 最好随着下游任务微调预训练的词向量，因为预训练中的词向量与分类中的类别之间的关系不一定一致。如good和bad，两个字出现场合基本一致导致预训练词向量较为相似，但在下游分类任务中不一定会算在同一类（如是否为情感句的判别任务与好评差评分类任务中，两者不一定会分在一类）
4. Dropout有着显著的正则效果，可以提升约2%～4%