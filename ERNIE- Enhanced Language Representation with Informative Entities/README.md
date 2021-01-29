# ERNIE: Enhanced Language Representation with Informative Entities

## Abstract

与百度提出的ERNIE同名，但不同篇与结构，这篇是清华与华为研究者提出的基于知识图谱增强BERT的预训练模型。

使用大量文本语料和知识图谱来训练，使得模型可以同时获得丰富的文本语义和知识。

## Introduction

虽然之前的预训练语言模型（如：BERT）获得了非常好的结果，但却忽略了将知识信息融入语言理解中，虽然原先的预训练模型可以通过上下文学习获得词之间的关系，故认为丰富的知识可以让语言模型进行语言理解以及在知识驱动的任务（如：实体类型判别和关系分类）上表现的更好。



两个融入外部知识进语言模型的难点：

1. 结构化知识的编码：如何根据输入文本有效提取和编码相关的知识进语言模型中。
2. 不同信息之间的融合：在预训练模型的预处理是不同于知识相关工程的预处理的，两者是不同的向量表示空间，如何设计一个预训练结构可以更好的融合语法、语义几知识是一大难点。



ERNIE

1. 为了提取与编码相关知识，先对输入文本*实体识别*并将该实体与知识图谱中的对应实体对齐，**使用如TransE的知识编码算法对知识图谱结构进行编码而不是直接使用**，将信息实体对embedding作为ERNIE的输入。
2. 与BERT相似，引入了masked language model 和 next sentence prediction作为预训练任务。为了更好的支持文本和知识的融合，设计了一预训练任务为**randomly masking some of the named entity alignments in the input text and asking the model to select appropriate entities from KGs to complete the alignment**。与现在的预训练模型仅通过本地文本来预测tokens不同，我们的训练任务需要模型学着融合文本和知识才能预测tokens和实体。

在ERNIE中的知识融合层有两个输入：一个是token embedding，另一个是concatenation of the token embedding and entity embedding



## Methodology

输入有长度为n的token序列以及长度为m的实体序列，n和m在多数情况下不相等，因为不是每个token都是知识图谱中的实体。在本文中只在实体所在token的第一个位置标记其实体信息（we align an entity to the first token in its named entity phrase）。

### Model Architecture

ERNIE可以分为两个模块：

1. T-Encoder：负责自输入tokens中获取词汇和语义的信息。
2. K-Encoder：融合外部知识进文本信息中，将自T-Encoder中提取的实体抽取出来后计算外部知识，最后都加入进文本信息中。

先将输入文本的每个token依据其token embedding, segment embedding, positional embedding计算其input embedding.

再将实体序列中的各个实体依据预先训练的知识嵌入模型TransE获得其entity embedding。

将input embedding 和 entity embedding作为K-Encoder的输入，而K-Encoder有两个输出分别是经过K-Encoder的input embedding序列以及entity embedding序列。

### Pre-training for Injecting Knowledge

为了使得知识能输入预训练模型中而提出一训练任务-randomly masks some token-entity alignments and then requires the system to predict all corresponding entities based on aligned tokens.

为降低预测的空间，只需自实体序列中预测实体即可。

与BERT的预训练任务相似，加入了一些错误在token-entity alignments任务中，



### Fine-tuning for Specific Tasks

#### Relation Classification

relation classification：the task requires systems to classify relation labels of given entity pairs based on context.

传统做法:Apply the pooling layer to the final output embeddings of the given entity mentions, and represent the given entity pair with the concatenation of their mention embeddings for classification.

本文做法：modifies the input token sequence by adding two mark tokens to highlight entity mentions. These extra mark tokens play a similar role like position embeddings in the conventional relation classification models.Then we can take the [CLS] token embedding for classifcation. We design different tokens [HD] and [TL] for head entities and tail entities respectively.

#### Entity Typing

Its a simplified version of relation classification.

Previous typing models make full use of both context embedding and entity mention embeddings.

This paper modified input sequence with the mention mark token [ENT] can guide ERNIE to combine both context information and entity mention information attentively.

# 结语

在BERT的前车之鉴下引入了TransE算法对知识图谱中知识编码的后压入模型中，并尝试了实体替换的训练任务，据作者所言可以更好的进行一些知识驱动的任务（如：relation classification and entity typing）