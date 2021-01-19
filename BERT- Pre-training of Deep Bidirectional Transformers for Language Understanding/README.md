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



  



