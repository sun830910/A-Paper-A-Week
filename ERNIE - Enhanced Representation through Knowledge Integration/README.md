# ERNIE - Enhanced Representation through Knowledge Integration

## Abstract

与BERT的mask策略相比，ERNIE被设计来学习knowledge mask策略，其中包含了entity-level的mask和phrase-level的mask。

Entity-level strategy：mask entities which are usually composed of multiple words.

Phrase-level strategy：mask whole phrase which is composed of several words standing together as comceptual unit.

## Introduction

原先的语言模型可以预测字段中失去的字，但并不代表具有先验知识。

像是模型无法预测Harry Potter和J.K.Rowling之间的关系，故若模型可以学习到更多的先验知识，则也增加了更多可靠性。

在本文中提出了一个名叫ERNIE的模型，它使用了knowledge masking策略。其中包含两种类型的策略：phrase-level strategy 和 entity-level strategy。

并在训练时将由多个字构成的、属于同一个单位的entity或phrase同时mask掉。

## Methods

在示范句子“Harry Potter is  series of fantasy novel written by J. K. Rowling.”中，BERT仅将 K. token部分mask掉，而ERNIE则是将J. K. Rowling 3个tokens同时mask掉，以此让模型学习到Harry Potter 和 J. K. Rowling之间的关系。

### Transormer Encoder

与BERT、GPT、XLM相似，使用了多层Transformer作为basic encoder。

三个输入：corresponding token、segment、position embedding。

输出序列的第一个token皆为分类embedding[CLS]。

### Knowledge Integration

use prior knowledge to enhance pretrained language model.

Instead of adding the knowledge embedding directly, we proposed a multi-stage knowledge masking strategy to integrate phrase and entity level knowledge into the Language representation.

#### Basic-Level Masking

It treat a sentence as a sequence of basic Language unit, for English, the basic language unit is word, and for Chinese , the basic language unit is Chinese Character.

在训练时，随机mask 15 percents的basic language units.

#### Phrase-Level Masking

The second, stage, employ phrase-level masking.

For English, Use lexical analysis and chunking tools to get the boundary of phrases in the sentences, and use some language dependent segmentation tools to get the word/phrase information in other language such as Chinese.

Randomly select a few phrases in the sentence, mask and predict all the basic units in the same phrase. At this stage, phrase information is encoded in to the word embedding.

#### Entity-Level Masking

The third stage.

Analyze the named entities in a sentence, and then mask and predict all slots in the entities.



