# Readme

## 摘要

（请简要说明创作本作品之动机、功能、特性、创新处、实用性）

​		传统的恶意代码检测包括基于签名特征码 （ signature ）的检测和基于启发式规则（heuristic）的检测，在应对数量繁多的未知恶意代码时，正面临越来越大的挑战。但面对现阶段恶意代码爆炸式的增长趋势，仅依赖人工进行恶意代码分析，在实施上变得愈发困难。因此基于机器学习的恶意请求识别就先的越来越重要。

​		基于机器学习，从 URL特征，域名特征， Web特征的关联分析，使恶意URL识别具有高准确率，并具有学习推断的能力。虽然机器学习在训练模型后无法达到百分百的效果，但相比传统手段，均有不同程度地检测效果提升。

本作品采集了数量充分的恶意请求样本和正常请求样本，训练出逻辑回归模型。然后根据模型可对请求进行识别，判断它是否为恶意请求。



## 作品概述

（建议包括：背景分析、相关工作、特色描述及应用前景分析等）   

在市面上，Google的Chrome已将检测模型与机器学习相结合，支持安全浏览，向用户警示潜在的恶意网址。结合成千上万的垃圾邮件、恶意软件、有启发式信号的含勒索软件的附件和发送者的签名（已被标识为恶意的），对新的威胁进行识别和分类。

目前大多数网站检测方式是通过建立URL黑白名单的数据库匹配进行排查，虽然具有一定的检测效果，但有一定滞后性，不能够对没有记录在案的URL进行识别。而基于机器学习，从 URL特征，域名特征， Web特征的关联分析，使恶意URL识别具有高准确率，并具有学习推断的能力。一些开源工具如Phinn提供了另个角度的检测方法，如果一个页面看起来非常像Google的登录页面，那么这个页面就应该托管在Google域名。Phinn使用了机器学习领域中的卷积神经网络算法来生成和训练一个自定义的Chrome扩展，这个 Chrome扩展可以将用户浏览器中呈现的页面与真正的登录页面进行视觉相似度分析，以此来识别出恶意URL（钓鱼网站）。 

基于机器学习算法的防护技术为实现高准确率、自动化的未知恶意代码检测提供了行之有效的技术途径，已逐渐成为业内研究的热点。

与此同时，机器学习作为新兴的前沿技术，即使解决或克服传统安全攻防技术的问题与难点，在一些场景与环境下，仍有无法避免的缺陷或者是即使解决了问题也无法满足实际需求，即无法采用机器学习算法进行安全攻防的盲点。如：无法发现未知模式的恶意行为 、误报大量测试异常的正常行为、对数据数量与质量有强依赖性等                

## 作品设计与实现

（建议包括系统方案、实现原理、硬件框图、软件流程、功能、指标等）

### 系统方案

#### 设计原则

准确性：url识别要能得到正确的结果

可维护性：易维护是应用系统成功与否的重要因素，它包含两层含义，故障易于排查、日常管理操作简便。

可扩充性：有新的恶意请求出现时，可以重新训练模型，以适应环境的变化。

#### 设计方案

##### 现状

目前大多数网站检测方式是通过建立URL黑白名单的数据库匹配进行排查，虽然具有一定的检测效果，但有一定滞后性，不能够对没有记录在案的URL进行识别。

传统的恶意代码检测仅依赖人工进行恶意代码分析，在实施上变得愈发困难。

#### 主要业务需求分析

1.对现有的恶意请求与正常请求进行训练，使恶意URL识别具有高准确率，并具有学习推断的能力。

2.作为数据集的请求要有足够的准确性。

3.训练模型所占用的内存不能超出上限

#### 方案

使用sklearn模块进行逻辑回归模型的训练，，在进行机器学习任务时，只需要简单的调用sklearn里的模块就可以实现大多数机器学习任务。

用文本向量化的方法将每条url的文本特征转化成固定的数字特征。

用sklearn中的模块对模型指标进行评价



### 实现原理

恶意请求检测的本质是一个分类问题，即把待检测样本区分成恶意或合法的请求。本作品的基本思想是通过机器学习（逻辑回归）建立检测模型，从而识别网站的恶意请求和正常请求。基本流程如下图所示：

- 读取正常请求和恶意请求数据集，预处理设置类标y和数据集x
- 通过N-grams处理数据集，并构建TF-IDF特征矩阵，每个请求对应矩阵的一行数据
- 数据集拆分为训练数据和测试数据
- 使用机器学习逻辑回归算法对特征矩阵进行训练，得出对应的模型
- 使用训练的模型对 未知URL请求进行检测，判断其是恶意请求或正常请求

![](C:\Users\10305\Desktop\信息安全作品赛\image\1基本流程图.png)

#### 数据集：

在https://github.com/foospidy/payloads中收集了常见的网站恶意请求，如SQL注入、XSS攻击等的Payload。实验数据包括：

正常请求：goodqueries.txt ，1265974条，来自http://secrepo.com网站日志请求
恶意请求：badqueries.txt，44532条，XSS、SQL注入等攻击的payload
测试请求：txt.txt

#### 逻辑回归的算法：

Logistic回归是一种广义线性回归（generalized linear model），目的是从特征学习出一个0/1分类模型，而这个模型是将特性的线性组合作为自变量，由于自变量的取值范围是负无穷到正无穷。因此，使用logistic函数（或称作sigmoid函数）将自变量映射到(0,1)上，映射后的值被认为是属于y=1的概率。
Logistic回归模型的适用条件二分类问题 所以选择逻辑回归做url分类.

#### 数据向量化

机器学习的算法需要输入一个固定结构的向量，而每一条url都没有特定的结构。故需要将每条url的文本特征转化成固定的数字特征。这里可以借鉴文本处理中将文本向量化的常用方法：TD-IDF.
TD-IDF 是一种用于资讯检索与文本挖掘的常用加权技术，被经常用于描述文本特征。
TF 词频（Term Frequency），表示词条在某文档中出现的频率。
IDF 逆向文件频率（Inverse Document Frequency）,作用在于如果包含词条的文档越少，则 IDF越大，说明该词条具有很好的类别区分能力。TF-IDF 倾向于过滤掉常见的词语，保留重要的词语。
TD-IDF输入的是文本的词语，需要将url分词。我们选择通过长度为N的滑动窗口将文本分割为N-Gram序列。n越大 ，产生的字母组合种类越多（256^n），产生的向量维度会更大，运算开销会增大，考虑到本机的性能，这里我们选择n=3。

#### 训练模型：

通过构建的特征矩阵作为训练集，调用逻辑回归进行训练和测试，Python中机器学习两个核心函数为fit()和predict()。这里，调用train_test_split()函数将数据集随机划分，代码如下：

```python
 #转化为tf-idf的特征矩阵
        self.vectorizer = TfidfVectorizer(tokenizer=self.get_ngrams)
        # 使用fit_transform学习的词汇和文档频率（df），将文档转换为文档 - 词矩阵。返回稀疏矩阵
        X = self.vectorizer.fit_transform(queries)

        # 使用 train_test_split 分割 X y 列表
        X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.01, random_state=0)

        # 定理逻辑回归方法模型
        self.lgs = LogisticRegression()

        # 使用逻辑回归方法训练模型实例 lgs
        self.lgs.fit(X_train, y_train)
```

模型指标

### 软件流程与功能

train.py训练出模型并保存，test输出模型 precision、 recall 、f1-score 、support 四个指标。detect对url进行识别。软件流程图如下：

![](C:\Users\10305\Desktop\信息安全作品赛\image\2软件流程.png)

### 指标

- precision：精确度，预测为正实际也为正占预测数的比率。

- recall：召回率，实际为正预测也为证占实际数的比率。

- f1-score：一般形式为
  $$
  fβ=(1+β2)⋅precision⋅recall/(β2⋅precison+recall)
  $$
  ，所以
  $$
  f1=precision⋅recall/(precison+recall)
  $$
  

- support：支持度，即实际类别个数。

- accuracy：准确度。

- macro avg与weighted avg：具体请查看[这个连接](https://www.baidu.com/link?url=VVb2GbKzdgxKGjtPFM5VlytUC3M-Ei8e0u2GMr-C9JhwBKIQQFrWfldWWdEzLHsmDq-KPdXUsa_UvnwaQitOVa&wd=&eqid=f12238e4000b77da000000065d8997db)。

通过测试数据集test.txt,利用model.score()方法查看准确率。通过classification_report方法计算 precision  、 recall 、f1-score 、support 这四个指标





## 作品测试与分析

（建议包括测试方案、测试环境搭建、测试设备、测试数据、结果分析等）

测试模型准确度、精确度、召回率、支持度与f1-score

1.测试准确度

用train_test_split方法随机划分训练集与测试集，测试集数量为数据集的百分之一

```
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.01, random_state=0)
```

用model.score计算准确率，结果为

![](C:\Users\10305\Desktop\信息安全作品赛\image\3.准确性.png)

由此可见，模型准确率较高，能识别出大部分url

2.测试精确度、召回率、支持度与f1-score

在test.py中读取test.txt中的url，在test.txt中有100条正常请求与100条恶意请求。

用classification_report方法进行评估，得到结果为

![](C:\Users\10305\Desktop\信息安全作品赛\image\4评估指标.png)

## 创新性说明

（本部分内容主要说明作品的创新性）

## 总结

## 参考文献

[1] 张东, 张尧, 刘刚, 宋桂香. 基于机器学习算法的主机恶意代码检测技术研究[J]. 网络与信息安全学报, 2017(7): 25-32.

[2] 杨轶, 苏璞睿, 应凌云, 等. 基于行为依赖特征的恶意代码相似性比较方法[J]. 软件学报, 2011, 22(10): 2438-2453.

[3] 杨晔. 基于行为的恶意代码检测方法研究[D]. 西安: 西安电子科技大学, 2015.

[4] 用机器学习玩转恶意URL检测 - 云+社区 - 腾讯云  https://cloud.tencent.com/developer/article/1043275

[5]《机器学习实战》Peter Harrington 著  李锐 李鹏 曲亚东 王斌 译

[6] 《统计学习方法》李航 著

