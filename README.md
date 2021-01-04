## A Neural Probabilistic Language Model

### 目的

因测试样本与训练样本存在单词训练上的差异而导致维度灾难(one -hot表示)上的问题，故本文通过提出学习各个单词的分布式表示来解决维度问题。

### 贡献

将神经网络引入语言模型的训练中，并得到了词嵌入这个副产物。  

词嵌入对后面深度学习在自然语言处理方面有较大的贡献，也是获得词的语义特征的有效方法。

### 小结

统计语言模型的目的在于学习单词序列中各个词之间的联合概率分布函数，模型通过训练集可以对指数级的样本进行建模，同时学习**每个单词的分布式表示**以及**单词序列的概率函数**。

传统方法中使用N-gram模型可以得到不错的效果，但会面临维度过高的问题。

在语言模型中的条件函数表达的是在前一个单词为某个单词X的情况下，下一个单词为某个单词Y的概率。这个函数的参数可以在迭代后贴合训练数据的最大对数似然。

---

## Attention Is All You Need

### 目的

使用注意力机制进行编码/解码，可以让模型较不重视输入与输出的距离依赖，且可以达到并行计算的效果。

### 贡献

使用注意力机制，提出了一种全新的编码/解码结构-Transformer，代替CNN、RNN。

### 小结

自注意力机制通过同一序列中两个不同位置之间的关系计算该序列的表示。  

Encoder有六层，每层有两个子层，第一子层为多头注意力网络，第二子层为一个简单的前向全连接层网络，并在两个子层上都有用残差网络与层归一化。  

Decoder也有六层，为了配合Encoder的每层各有两个子层，在Decoder上有三个子层，并在每个子层上引入残差网络与层归一化。  

注意力函数可以作为query与key-value对之间的映射，输出则为value计算权重后的和。

#### Scaled Dot-Product Attention

使用【Scaled Dot-Product Attention】计算注意力，其中输入由dk维度的queries和keys组成，与dv维度的values。  

首先计算query与所有keys之间的点积后，各自除于dk的平方(因为若dk太大，会导致点积容易变得太大)，然后套用softmax函数获得各个values的权重。  

将queries打包成一矩阵后可以同时计算，用以加速。

#### Multi-head Attention

可以让模型同时利用多个不同表示空间中不同位置的信息。  

Transformer中在三种不同地方使用了多头注意力机制：  

1. Encoder-Decoder attention layers：其中queries来自之前的decoder层，keys和values来自encoder的输出
2. Encoder中，让encoder中的每个位置可以于前一层encoder的所有位置计算。
3. Decoder中。

#### Position Encoding

偶数位置加入sine函数，奇数位置加入cosine函数。