# my_tuijian
1. 使用BM25进行物品协同过滤。
优点：高效处理稀疏矩阵： 用户-物品交互矩阵通常非常稀疏，即大多数用户只与少量物品产生交互。BM25 非常适合处理这种稀疏数据，特别是在有大量物品时。
      考虑“流行物品”的平衡： 类似于文本搜索中的 IDF，BM25 可以控制流行物品的权重，防止推荐系统只推荐那些被大量用户交互的热门物品。这是为什么?
   在传统的物品协同过滤中，物品相似度的计算往往使用余弦相似度或皮尔逊相关系数，而 BM25 则提供了一种更加复杂和细致的方式，能够考虑到“词频”和“文档频率”（在推荐系统中类似于交互频率和物品流行度）的平衡。
