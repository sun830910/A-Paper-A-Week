# ERNIE 2.0: A Continual Pre-Training Framework for Language Understanding

## Abstract

Propose a continual pre-training framework named ERNIE2.0 which incrementally builds pre-training tasks and then learn pre-trained models on these constructed tasks via continual multi-task learning.

Construct several tasks and train the ERNIE2.0 model to capture lexical, syntactic and semantic aspects of infromation in the training data.

## Introduction

支持多任务接续训练，可以接续之前训练出的参数在新的训练任务上继续训练。

### Continual Pre-training

The process of continual pre-training contains two steps.

1. 构建一由大量数据、先验知识构成的无监督预训练数据集
2. 通过连续多任务学习逐渐更新ERNIE模型

### Pre-training Tasks Construction

Construct different kinds of tasks at each time, including word-aware tasks, structure-aware tasks and semantic-aware tasks.

### Continual Multi-task Learning

目标是提出一新模型框架可以自不同的任务中接续学习，但会有两个问题：**如何在不忘记之前知识的情况下学习新任务** 以及 **如何在这些任务上有效的预训练** 。

Whenever a new task comes, the continual multi-task learning method **first uses the previously learned parameters to initialize the model**, and then train the newly-introduced task together with the original tasks simultaneously.

The parameters of the encoder can be updated across all learning tasks.

There are two kinds of loss function in our framework, one is the sentence-level loss and the other one is the token-level loss, which are similar to the loss function of BERT.

Each pre-training task has its own loss function.

During pre-training, one sentence-level loss function can be combined with multiple token-level loss function can be combined with multiple token-level loss functions to continually update the model.



## ERNIE 2.0 Model

### Model Structure

与GPT、BERT、XLM相同，使用了multi-layer Transformer作为basic encoder。

### 特色

使用了一种Task Embedding。

使用0到N到的索引来表示不同的任务，每个不同任务有自己的一个task embedding，而ERNIE 2.0 的模型输入共有**Token embedding、Position embedding、 Sentence embedding、Task embedding**四种。

### Pre-training Tasks

分别构建三种不同粒度的预训练任务，分别为word-aware tasks、structure-aware tasks、semantic-aware tasks。

#### Word-aware Pre-training Tasks

Knowledge Masking Task：predict the whole masked phrases and named entities to help the model learn the dependency information in both local contexts and global contexts.

Capitalization Prediction Task：大写开头的字母通常具有特殊的含义。Add a task to predict whether the word is capitalized or not.

Token-Document Relation Prediction Task：Predict whether the token in a segment appears in other segments of the original document.

#### Structure-aware Pre-training Tasks

Sentence Recordering Task：This task aims to learn the relationships among sentences. 将一文章段落随机切成m个段落，然后随机交换顺序后重新组合，让模型学习一个k分类，其中k为一与m有关的损失。经验证明模型根据此任务可以学习到句子与文档之间的关系。

Sentence Distance Task：Aim to Learn the sentence distance using document-level information.This task is modeled as a 3-class classification problem, 0 represents that the two sentences are adjacent in the same document, 1 represent that the two sentences are in the same document but not adjacent, 2 represents thatt the two sentences are from two different documents.

#### Semantic-aware Pre-training Tasks

Discourse Relation Task：This task predict the semantic or rhetorical relation between two sentences.

IR Relevance Task：This task to learn the short text relevance in information retrieval. Its a 3-class classification task which predicts the relationship between a query and a title. We take the query as the first sentence and the title as the second sentence. The search log data from a commercial search engine is used  as our pre-training data.

There are 3 kinds of labels in this tasks. The query and title pairs that are labelled as 0 stand for strong relevance, which means that the title is clicked by the users afer they input the query. Those labelled as 1 represent weak relevance, which implies that when the query is input by the users, these titles appear in the search results but failed to be clicked by users. The label 2 means that the query and title are completely irrelevant and random in terms of semantic infromation.



# 感想

没特别细看，但感觉就是多搞了很多预训练任务让效果优于其他模型。