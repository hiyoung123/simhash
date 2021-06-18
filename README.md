# SimHash 算法的简单实现

## 介绍
SimHash 算法是 Google 出品的一款短小精悍，用来处理海量文本去重的算法。

## 优点
* 实现简单
* 速度快，且精度比较好
## 缺点
* 文本长度过小的话，精度不太准确，一般文本长度大于 500 个单词，效果比较好。
* 对比分布式表示文本表示方法，精度还是不太好。
## 算法流程
* 分词：将文本进行分词处理
* Hash：将每个词进行 Hash 处理成固定长度（一般为64/128）的 0/1 特征编码，即每一位非 0 即 1
* 加权：根据每个词的权重（TF-IDF）对特征编码进行加权，W = Hash * Weight, 如果对应hash值为 0，则乘上 -Weight
* 合并：将所有词计算后的 W 进行累加，代表文本的 W。
* 降维：对于上述累加结果的每一位，如果大于 0, 则置 1 ，否则置 0。从而得到每一位都是 0/1 的 SimHash 编码。
* 计算：通过上述方法得到了两个文本的 SimHash 编码，然后计算两者的汉明距离，得到的结果如果小于 n（一般为3）,则表示两个文本是相同的。
## 依赖
* jieba
## 使用
```python
    text1 = '哈哈哈，你妈妈喊你回家吃饭哦，回家罗回家罗'
    text2 = '哈哈哈，你妈妈叫你回家吃饭啦，回家罗回家罗'
    sh = SimHash()
    encoded1 = sh.encode(text1)  # 进行SimHash编码
    encoded2 = sh.encode(text2)  # 进行SimHash编码
    similar = sh.similar(text1, text2, 3)  # 相似度计算，n=3

    print(encoded1)
    print(encoded2)
    print(similar)

    text1 = '南京市长江大桥上的汽车'
    text2 = '南京长江大桥上的车'
    sh = SimHash()
    encoded1 = sh.encode(text1)  # 进行SimHash编码
    encoded2 = sh.encode(text2)  # 进行SimHash编码
    similar = sh.similar(text1, text2, 3)  # 相似度计算，n=3

    print(encoded1)
    print(encoded2)
    print(similar)

```
结果：
```python
0000000000000000000000110101100010001101111001011101110100100110
0000000000000000000000110101100010001101111001001001110100100110
True
0000000000000000010000110001101110100100100011000110010100000011
0000000000000000000000100000000000100000100000000001000000000001
False
```

```python
    sh = SimHash()
    encoded1 = sh.encode(text1)  # 进行SimHash编码
    encoded2 = sh.encode(text2)  # 进行SimHash编码
    encoded3 = sh.encode(text3)  # 进行SimHash编码
    print(encoded1)
    print(encoded2)
    print(encoded3)

    similar = sh.similar(text1, text2, 3)
    print(similar)
    similar = sh.similar(text1, text3, 3)
    print(similar)
    similar = sh.similar(text2, text3, 3)
    print(similar)

```
结果
```python
0000000000000000000000111100100101011111111001000110000100001101
0000000000000000000000111100100100011111111001100110000100001101
0000000000000000000000100000100011011011110011000010000100001000
True
False
False
```
