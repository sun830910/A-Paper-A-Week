# A Survey on Text Classification- From Shallow to Deep Learning

## Introduction

本文介绍了文本分类领域中自浅层学习到深度学习的重要或特色模型与演进。  

无论是浅层学习或深度学习都得对数据做预处理。  

重要差距在于浅层学习需要注重特征抽取与分类器设计，在特征抽取上较无合理性与较为主观，而深度学习则直接端到端出结果。



## Shallow Learning Models

预处理：文本分割、数据清洗、数据统计

### 文本表示方法

Bag-of-words(BOW)、N-gram、term frequency-inverse documnet frequency(TF-IDF)、word2vec和GloVe

BOW：词袋模型，At the core of the BOW is representing each text with a dictionary-sized vector. The individual value of thevector denotes the word frequency corresponding to its inherent position in the text.

N-gram：considers the information of agjacent words are builds a dictionary by considering the adjacent words.

TF-IDF：use the word frequency and inverses the document frequency to model the text.

Word2vec：employ local context information to obtain word vectors.

GloVe：with both the local context and global statistical features, trains on the nonzero elements in a word-word co-occurrence matrix.



### 分类器

PGM-based methods：Probabilistic graphical models（PGMS）, express the conditional dependencies among features in graphs.

Native Bayes：因结构简单而被广泛用于文本分类中，虽然它假设了特征彼此独立，但有时候是不独立的。

为改善在小样本上的表现，在特征选择方法上可以引入【计算训练集和对应类别之间的KL散度】来改善Native Bayes 分类器。

介绍了如Bayes、SVM、决策树的文本分类器与其变种。

