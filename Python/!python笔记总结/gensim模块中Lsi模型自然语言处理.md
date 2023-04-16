# gensim模块中Lsi模型自然语言处理

- 使用jieba分词处理词库，使用gensim中的Lsi模型训练语料库

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import jieba
from gensim import corpora
from gensim import models
from gensim import similarities

q_l = ["今天你冷吗", "北京的雾霾去哪了", "北京的天蓝吗", "你叫什么名字", "你今年几岁啦", ]  # 问题库
q_s = "你今年多大了"  # 问题样本

# 1.问题库分词处理
all_doc_list = []
for doc in q_l:
    doc_list = [word for word in jieba.cut(doc)]
    all_doc_list.append(doc_list)
doc_test_list = [word for word in jieba.cut(q_s)]
# print(all_doc_list)
# print(doc_test_list)
'''
[['今天', '你', '冷', '吗'], ['北京', '的', '雾', '霾', '去', '哪', '了'], ['北京', '的', '天蓝', '吗'], ['你', '叫', '什么', '名字'], ['你', '今年', '几岁', '啦']]
['你', '今年', '多大', '了']
'''

# 2.制作语料库
dictionary = corpora.Dictionary(all_doc_list)  # 制作词袋：就是将很多很多的词,进行排列形成一个 词(key) 与一个 标志位(value) 的字典
# print("token2id", dictionary.token2id)
# print("dictionary", dictionary)
'''
token2id {'今天': 0, '你': 1, '冷': 2, '吗': 3, '了': 4, '北京': 5, '去': 6, '哪': 7, '的': 8, '雾': 9, '霾': 10, '天蓝': 11, '什么': 12, '叫': 13, '名字': 14, '今年': 15, '几岁': 16, '啦': 17}
dictionary Dictionary(18 unique tokens: ['今天', '你', '冷', '吗', '了']...)
'''


# 2.1问题库的语料库
corpus = [dictionary.doc2bow(doc) for doc in all_doc_list]
# 这里是将all_doc_list 中的每一个列表中的词语 与 dictionary 中的Key进行匹配得到一个匹配后的结果,例如['你', '今年', '几岁', '了']，然后根据上面得到的 token2id {'今天': 0, '你': 1, '冷': 2, '吗': 3, '了': 4, '北京': 5, '去': 6, '哪': 7, '的': 8, '雾': 9, '霾': 10, '天蓝': 11, '什么': 12, '叫': 13, '名字': 14, '今年': 15, '几岁': 16, '啦': 17}
# 就可以得到 [(1, 1), (4, 1), (15, 1), (16, 1)]
# 1代表的的是 你 1代表出现一次, 4代表的是 了  1代表出现了一次, 以此类推 15 = 今年 , 16 = 几岁
# print("corpus", corpus, type(corpus))
'''
corpus [[(0, 1), (1, 1), (2, 1), (3, 1)], [(4, 1), (5, 1), (6, 1), (7, 1), (8, 1), (9, 1), (10, 1)], [(3, 1), (5, 1), (8, 1), (11, 1)], [(1, 1), (12, 1), (13, 1), (14, 1)], [(1, 1), (15, 1), (16, 1), (17, 1)]] <class 'list'>
'''

# 2.2问题的语料库
# 将需要寻找相似度的分词列表 做成 语料库 doc_test_vec
doc_test_vec = dictionary.doc2bow(doc_test_list)
# print("doc_test_vec", doc_test_vec, type(doc_test_vec))
'''
doc_test_vec [(1, 1), (4, 1), (15, 1)] <class 'list'>
'''
# 3. 将corpus语料库(初识语料库) 使用Lsi模型进行训练
lsi = models.LsiModel(corpus)
# 4 文本相似度获取
# 4.1 利用稀疏矩阵相似度算法 将 问题库的 语料库corpus的训练结果作为初始值
index = similarities.SparseMatrixSimilarity(lsi[corpus], num_features=len(dictionary.keys()))
'''
举个例子：
lsi 就是很多月饼模子
lsi[corpus] 把 问题库中的每一个问题都用模子压出一个月饼
lsi[doc_test_vec] 把问题样本压出一个月饼，拿着这个月饼，通过一定的算法去与lsi[corpus]比较。
'''
# 4.2 将 语料库doc_test_vec 在 语料库corpus的训练结果 中的 向量表示 与 语料库corpus的 向量表示 做矩阵相似度计算
sim = index[lsi[doc_test_vec]]
# print("sim", sim, type(sim))
'''
sim [ 4.3786496e-01  3.3099478e-01 -1.4901161e-08  4.3786496e-01
  8.7572986e-01] <class 'numpy.ndarray'>  
计算结果是：问题样本与问题库中的每一个问题匹配度的评分，根据评分就可以取到相似度最高问题
'''
# 5.排序，获取结果
cc = sorted(enumerate(sim), key=lambda item: -item[1])
# print(cc)
'''[(4, 0.87572986), (0, 0.43786496), (3, 0.43786496), (1, 0.33099478), (2, -1.4901161e-08)]'''

ret = q_l[cc[0][0]]
print(' 问题样本:', q_s, '\n', '匹配结果:', ret)
'''你今年多大了 你今年几岁啦'''
```