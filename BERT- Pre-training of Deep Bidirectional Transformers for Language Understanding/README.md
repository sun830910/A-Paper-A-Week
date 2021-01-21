# BERT- Pre-training of Deep Bidirectional Transformers for Language Understanding

## Abstract

使用无标注的语料为输入，并同时从双向读入语料，以双向Transformer作为Encoder训练一预训练语言模型。  

可以不用大量修改就应用于下游任务。  



## Introduction

两种使用预训练语言模型的方法：feature-based 和 fine-tuning  

1. feature-based：基于面对特定任务的模型结构，融合预训练向量作为额外的特征，如：ELMo。
2. fine-tuning：在下游任务中简单fine-tuning所有已经训练的参数，如：GPT。

这两种方法与预训练时共享相同的目标函数。  



原先在预训练模型上仅为单向的，故只能选择从左到右的模型结构，本文作者认为在预训练语言模型方面，自上下文双向获取特征是重要的。  

BERT中使用masked language model（MLM）减缓之前方法的单向限制，并随机mask输入中的一些tokens，而训练目标则是预测被mask的token原始的单词是什么。  

并额外使用“next sentence prediction”任务对一对输入进行训练预测。  



## BERT

共有两步骤使用：pre-training 和 fine-tuning。  

Pre-training时，模型使用没有标注的数据在各项pre-training tasks上训练。  

Fine-tuning时，模型使用pre-trained参数初始化后，配合下游任务的标注数据在各项下游任务上使用。  

BERT在结构上是一个多层的双向Transformer来做编码  

Base BERT的超参数为L=12,H=768,A=12,Total Parameters=110M. 

Large BERT的超参数为L=24,H=1024,A=16,Total Parameters=340M  

GPT中使用了单向的self-attention而BERT中使用了双向的self-attention。  

### Input/Output Representations

为了让BERT适应多变的下游任务，在输入上可以输入单句或者一组句子（问题，回答）。  

使用了WordPiece embeddings避免OOV问题。   

每一个序列的第一个token总是一个特殊的分类token[CLS]，这个token也可以作为序列的表示。

若输入为两个句子时，则打包成一个序列，分为两步：

1.  将两个句子之间以一个特殊的token[SEP]区隔。  
2. 添加一个可学习的embedding学习每个token是属于哪个句子的。  

输入包含了对应的token向量、句区隔[SEP]以及位置编码。  

### Pre-training BERT

使用两种无监督任务预训练BERT：

1. Masked LM：原先双向语言模型行不同是因为会在学习过程中看到自己，故以一定概率随机mask掉input token，然后以softmax+词表的方式预测这些被masked的词是什么，在本文中对每个序列中的token mask概率为15%。

   在fine-tuning时没有[MASK]这个token，有80%的概率将token转为[MASK]、10%概率转为随机token、10%概率维持原有token。

2. Next Sentence Prediction：训练一二分类器判别是否为上下句，





  



