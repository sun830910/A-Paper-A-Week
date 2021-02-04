# Evaluation of BERT and ALBERT Sentence Embedding Performance on Downstream NLP Tasks

## Abstract

实验结果表明本文实验的模型在**语义相似度（Semantic Textual Similarity, STS）** 和**自然语言推理（Natural Language Inference, NLI）**优于BERT和ALBERT以[CLS]作为句向量的表达方式。

## Related Work

介绍了一下BERT和ALBERT的[CLS]提供了句向量。

## Models

使用下面六种实验模型测试效果：

1. 直接使用[CLS] token embedding
2. 使用池化后的token embedding

通过convolutional neural net的pooling layer对token embedding求平均，实验中使用max pooling

3. 使用Sentence BERT(SBERT)

SBERT采用了孪生网络的结构，将不同的句子输入到参数共享的两个BERT模型中，获取句子信息表示。

单纯使用BERT模型分别对两组输入取句向量后取平均池化或者CNN后计算Cosine相似度得到一个MSE loss。

4. 使用Sentence ALBERT

将实验模型3的BERT换成ALBERT而已。

5. 使用CNN-Sentence BERT

使用CNN结构，SBERT中采用Avg-pooling获取句子向量表示，本文将其替换成CNN网络结构获取句子向量表示，将其中输入有Batch Size 、number of tokens、transformer hidden size。

6. 使用CNN-Sentence ALBERT



## Result

1. 微调BERT和ALBERT是有效的，微调后效果较好。
2. CLS的效果较为逊色，无论是否微调，CLS的效果都较逊色于平均池化操作或其他方法。
3. 总体来看CNN-BERT>SBERT>Avg pooling>CLS，CNN-BERT表现最好
4. CNN对ALBERT的改进远大于对BERT的改善，可能因为ALBERT中内部参数共享，故存在不稳定性，CNN网络结构可以减缓这种不稳定性。