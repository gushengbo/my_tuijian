# my_tuijian
1. 使用BM25进行物品协同过滤。
优点：高效处理稀疏矩阵： 用户-物品交互矩阵通常非常稀疏，即大多数用户只与少量物品产生交互。BM25 非常适合处理这种稀疏数据，特别是在有大量物品时。
      考虑“流行物品”的平衡： 类似于文本搜索中的 IDF，BM25 可以控制流行物品的权重，防止推荐系统只推荐那些被大量用户交互的热门物品。这是为什么?
   
   在传统的物品协同过滤中，物品相似度的计算往往使用余弦相似度或皮尔逊相关系数，而 BM25 则提供了一种更加复杂和细致的方式，能够考虑到“词频”和“文档频率”（在推荐系统中类似于交互频率和物品流行度）的平衡。

   2.1. IDF 的平衡机制

在信息检索中，IDF 的作用是降低那些在许多文档中频繁出现的词（如“the”之类的高频词）的权重，因为这些词对区分文档没有帮助。同样，在推荐系统中，BM25 通过类似的方式对非常流行的物品进行权重削减。这意味着：

如果某个物品被大量用户交互，BM25 认为它对个性化推荐的贡献较小，因为它对每个用户来说都是常见的物品。
因此，BM25 会自动降低这些“流行物品”的权重，避免系统频繁推荐这些物品。

实验： 单纯使用BM25，指标能从0到18。 但相比于共现矩阵的30，就差很多。可以作为一种特征把，或者作为一种召回。



如何处理文本类的resource_name(43223),resource_tags(3007), subject(50):

用word2vec 然后用k-means或者DBSCAN聚类都不行，太多噪声点了。大多都在一个簇里面。

用双塔模型训练，一边是用户subject,一边是resource_name，但好像不收敛，用二元分类。

用共现矩阵也不行，太稀疏了。

最后！！！先把英文文本分开，分为一个簇，然后对中文文本清晰，用word2vec，k-means(簇47).效果还行

单独用resource_name，效果不行

![image](https://github.com/user-attachments/assets/0e273c37-ee18-4592-a058-0fbb770977b9)

resource_name和subject搭配一起用，还可以。但是！！现在的问题是，为什么越训越掉点。
![image](https://github.com/user-attachments/assets/5ad3305f-a00c-4125-8d52-fb57b62a9b09)

只用last_score, resource_name,subject。分数达到0.24
基于用户点击序列的tags和候选物品的tags。计算相似度。（如果加上长线相似度，疯狂掉点，0.18）
只用last_score, 跟tags相似度，分数0.254.
但是tags相似度跟resource_name,subject，又会掉点0.22

加入物品popularity特征也不行。



