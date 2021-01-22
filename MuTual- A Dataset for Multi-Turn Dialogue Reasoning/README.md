# MuTual- A Dataset for Multi-Turn Dialogue Reasoning

## Abstract

在当今的对话系统中，最大的问题是有时会给出一个有逻辑问题的答案。为促进在对话合理性的研究，而提出了这个数据集。数据集出自中国学生的英文听力理解考试。  

SOTA模型在这个数据集上仅有71%，远低于人类的94%  

## Introduction

在现有的对话系统中，都在大语料上训练后用来预测哪一个回答正确。

在回答上共有两种做法：提取与生成。  

数据集提供一个Context和四个与Context有关的Candidates，但只有一个candidate是逻辑正确的。  

本数据集难点在于需要模型对社交礼节和关系有所理解，而这些是难以从大语料训练中获得的。  

原始数据由<diualogue, question, answer>构成，经过写手人工重写后，使得数据依循传统答案选择设定，即让模型自阅读多轮对话后选择出一正确答案。  

MuTual is the first human-labeled reasoning-based dataset for multi-turn dialogue.  

## Dataset

### Collection

自公开网站上爬取听力测验，测验题目类型为两人对话或一简单叙述类型。爬下后以<Conversation(audio), Question and Choices(text), Answer(image)>形式保存原始数据  

Step 1：Pre-processing：若两组问题和答案相同，则判定为重复并删除其一。若有超过3个candidate，则随机删除至只剩下3个为止。答案在原始数据中以图片形式保存，故使用OCR系统将其转为text形式，并人工确认所有的OCR结果。原始对话以语言形式存储于原始数据中，故使用ASR系统将器转换为文字。  

Step 2：Candidate Response Creation：候选答案由1个正确答案、2个错误答案，并以人工方式撰写一以正确答案为基础进行修改的错误答案。