# XGBoost 应用学习

- 本文档偏向XGBoost应用

## 一 . 简介

- XGBoost全称是e**X**treme **G**radient **B**oosting，可译为极限梯度提升算法。**它由陈天奇所设计，致力于让提升树突破自身的计算极限，以实现运算快速，性能优秀的工程目标。**
- 优点
  1. 业界甚至将其称之为“机器学习竞赛的胜利女神”
  2. XGBoost全称是e**X**treme **G**radient **B**oosting，可译为极限梯度提升算法, 和传统的梯度提升算法相比，XGBoost进行了许多改进，它能够**比其他使用梯度提升的集成算法更加快速**，**在分类和回归上都拥有超高性能的先进评估器。**
  3. 应用广泛: 除了比赛之中，高科技行业和数据咨询等行业也已经开始逐步使用XGBoost
- 缺点
  - 因为底层使用的是树模型,所以容易出现过拟合问题

## 二. XGBoost的三大板块

- XGBoost本身的核心是基于梯度提升树实现的集成算法，整体来说可以有三个核心部分：集成算法本身，用于集成的弱评估器，以及应用中的其他过程。三个部分中，前两个部分包含了XGBoost的核心原理以及数学过程，最后的部分主要是在XGBoost应用中占有一席之地

  ![image-20210406113449461](https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20210406113449461.png)

## 三. GBDT 梯度提升树

- 梯度提升树- GBDT
- 极限梯度提升树 - XGBoost

#### 3.1 提升集成算法-n_estimators

- 梯度提升（Gradient boosting）是构建预测模型的最强大技术之一，它是集成算法中提升法（Boosting）的代表算法。**集成算法通过在数据上构建多个弱评估器，汇总所有弱评估器的建模结果，以获取比单个模型更好的回归或分类表现。**弱评估器被定义为是表现至少比随机猜测更好的模型，即预测准确率不低于50%的任意模型。
- 梯度提升回归树是专注于回归的树模型的提升集成模型，其建模过程大致如下：**最开始先建立一棵树，然后逐渐迭代，每次迭代过程中都增加一棵树，逐渐形成众多树模型集成的强评估器。**

![image-20210406113855300](https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20210406113855300.png)

- **对于决策树而言，每个被放入模型的任意样本 最终一个都会落到一个叶子节点上。而对于回归树，每个叶子节点上的值是这个叶子节点上所有样本的均值。对于梯度提升回归树来说，每个样本的预测结果可以表示为所有树上的结果的加权求和：**

![image-20210406113949946](https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20210406113949946.png)

- #### XGB vs GBDT核心区别：求解预测值 的方式不同
  - GBDT中预测值是由所有弱分类器上的预测结果的加权求和，其中每个样本上的预测结果就是样本所在的叶子节点的均值。
  - XGBT中的预测值是所有弱分类器上的叶子权重直接求和得到，**计算叶子权重是一个复杂的过程。**

#### 3.2 有放回随机抽样：subsample

- 集成的目的是为了模型在样本上能表现出更好的效果，所以对于所有的提升集成算法，**每构建一个评估器，集成模型的效果都会比之前更好**。也就是随着迭代的进行，模型整体的效果必须要逐渐提升，最后要实现集成模型的效果最优。

- 在梯度提升树中，我们每一次迭代都要建立一棵新的树，因此我们每次迭代中，都要有放回抽取一个新的训练样本。不过，这并不能保证每次建新树后，集成的效果都比之前要好。因此我们规定，在梯度提升树中，**每构建一个评估器，都让模型更加集中于数据集中容易被判错的那些样本**。来看看下面的这个过程。

  ![image-20210406141704112](https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20210406141704112.png)

  ```python
  首先我们有一个巨大的数据集，在建第一棵树时，我们对数据进行初次又放回抽样，然后建模。建模完毕后，我们对模型进行一个评估，然后将模型预测错误的样本反馈给我们的数据集，一次迭代就算完成。紧接着，我们要建立第二棵决策树，于是开始进行第二次又放回抽样。但这次有放回抽样，和初次的随机有放回抽样就不同了，在这次的抽样中，我们加大了被第一棵树判断错误的样本的权重。也就是说，被第一棵树判断错误的样本，更有可能被我们抽中。基于这个有权重的训练集来建模，我们新建的决策树就会更加倾向于这些权重更大的，很容易被判错的样本。建模完毕之后，我们又将判错的样本反馈给原始数据集。下一次迭代的时候，被判错的样本的权重会更大，新的模型会更加
  倾向于很难被判断的这些样本。如此反复迭代，越后面建的树，越是之前的树们判错样本上的专家，越专注于攻克那
  些之前的树们不擅长的数据。对于一个样本而言，它被预测错误的次数越多，被加大权重的次数也就越多。我们相
  信，只要弱分类器足够强大，随着模型整体不断在被判错的样本上发力，这些样本会渐渐被判断正确。如此就一定程
  度上实现了我们每新建一棵树模型的效果都会提升的目标。
  ```

#### 3.3 迭代决策树：eta

- "eta"，是迭代决策树时的步长（shrinkage），又叫做学习率（learning rate）。和逻辑回归中的 类似，eta越大，迭代的速度越快，算法的极限很快被达到，有可能无法收敛到真正的最佳。 越小，越有可能找到更精确的最佳值，更多的空间被留给了后面建立的树，但迭代速度会比较缓慢。

  ![image-20210406142022627](https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20210406142022627.png)

## 四. XGBoost建模流程

`推荐使用XGBoost  API`

#### 4.1 回归建模

- XGBoost API建模

  ```python
  import xgboost as xgb
  from sklearn.datasets import load_boston  # 数据集 波士房价顿数据集
  from sklearn.model_selection import KFold  # 用于交叉验证
  from sklearn.model_selection import cross_val_score as CVS,train_test_split
  from sklearn.metrics import mean_squared_error as MSE  # 均方误差
  import pandas as pd 
  import numpy as np 
  import matplotlib.pyplot as plt 
  from time import time 
  import datetime
  
  data = load_boston() #波士顿数据集非常简单，但它所涉及到的问题却很多
  x= data.data
  y = data.target
  Xtrain,Xtest,Ytrain,Ytest = train_test_split(data.data,data.target,test_size=0.3,random_state=420)  # 分割训练数据和测试数据
  
  
  """
  使用pickle 保存
  """
  import pickle
  
  dtrain = xgb.DMatrix(Xtrain,Ytrain)
  
  params = {
      "silent":True,
      "obj":"reg:linear",
      "subsample":1
      ,"eta":0.05
      ,"gamma":20
      ,"lambda":3.5
      ,"alpha":0.2
      ,"max_depth":4
      ,"colsample_bytree":0.4
      ,"colsample_bylevel":0.6
      ,"colsample_bynode":1
      }
  num_round = 180
  
  bst = xgb.train(params=params,dtrain=dtrain,num_boost_round=num_round)
  
  # 保存模型
  
  pickle.dump(bst,open("xgboost-pickle-model.dat","wb"))
  
  
  """
  使用pickle 加载
  """
  from sklearn.datasets import load_boston
  from sklearn.model_selection import train_test_split as TTS
  from sklearn.metrics import mean_squared_error as MSE
  import pickle
  import xgboost as xgb
  
  
  data = load_boston()
  x = data.data
  y = data.target
  Xtrain,Xtest, Ytrain,Ytest = TTS(x,y,test_size=0.3,random_state=420)
  
  dtest = xgb.DMatrix(Xtest,Ytest)
  
  # 导入模型
  loaded_model = pickle.load(open("xgboost-pickle-model.dat","rb"))
  print("loaded model from : xgboost-pickle-model.dat ")
  
  # 预测
  ypreds = loaded_model.predict(dtest)
  
  from sklearn.metrics import r2_score
  
  MSE(Ytest,ypreds)  # 均方误差  9.107
  r2_score(Ytest,Ytest)  # 评分r平方
  
  
  ```

- sklean API

  ```python
  
  from xgboost import XGBRegressor as XGBR
  from sklearn.ensemble import RandomForestRegressor as RFR
  from sklearn.linear_model import LinearRegression as LinearR
  from sklearn.datasets import load_boston
  from sklearn.model_selection import KFold, cross_val_score as CVS, train_test_split as TTS
  from sklearn.metrics import mean_squared_error as MSE
  import pandas as pd
  import numpy as np
  import matplotlib.pyplot as plt
  from time import time
  import datetime
  
  data = load_boston()
  #波士顿数据集非常简单，但它所涉及到的问题却很多
  X = data.data
  y = data.target
  Xtrain,Xtest,Ytrain,Ytest = TTS(X,y,test_size=0.3,random_state=420)
  reg = XGBR(n_estimators=100).fit(Xtrain,Ytrain) #训练
  reg.predict(Xtest) #传统接口predict
  reg.score(Xtest,Ytest) #你能想出这里应该返回什么模型评估指标么？利用shift+Tab可以知道，R^2评估指标
  y.mean()
  MSE(Ytest,reg.predict(Xtest))#可以看出均方误差是平均值y.mean()的1/3左右，结果不算好也不算坏
  ```

  



#### 4.2 分类建模

- XGBoost API

  ```python
  dtrain = xgb.DMatrix(Xtrain,Ytrain)
  dtest = xgb.DMatrix(Xtest,Ytest)
  params={
      'slient':True,
      'objective':'binary:logistic',
      'eta':0.1,
      'scale_pos_weight':1
  }
  
  num_round = 100
  bst = xgb.train(params=params,dtrain=dtrain,num_boost_round=num_round)
  
  preds = bst.predict(dtest)
  preds
  
  ```

  

- sklean  API

  ```python
  ## Indians_xgboost.py
  from numpy import loadtxt
  from xgboost import XGBClassifier
  from sklearn.model_selection import train_test_split
  from sklearn.metrics import accuracy_score
  """
  推荐使用XGBoost库本身
  """
  dataset = loadtxt('dataset_001.csv', delimiter=",")
  X = dataset[:,0:8]
  Y = dataset[:,8]
  seed = 7
  test_size = 0.33
  X_train, X_test, y_train, y_test = train_test_split(X, Y, test_size=test_size, random_state=seed)
  # 不可视化数据集loss
  #model = XGBClassifier()
  #model.fit(X_train, y_train)
  
  ##可视化测试集的loss
  model = XGBClassifier()
  eval_set = [(X_test, y_test)]
  model.fit(X_train, y_train, early_stopping_rounds=10, eval_metric="logloss", eval_set=eval_set, verbose=False)
  # 改为True就能可视化loss
  y_pred = model.predict(X_test)
  predictions = [round(value) for value in y_pred]
  
  accuracy = accuracy_score(y_test, predictions)
  print("Accuracy: %.2f%%" % (accuracy * 100.0))
  ##Accuracy: 77.56%
  ```

  

## 五. XGBoost调参优化策略

#### 5.1 XGBoost参数

- XGBoost一般调参顺序:
  1. num_round(n_estimators)   : **集成中弱评估器的数量**, 越大越好,取值范围[0, +∞],默认值10
  2. eta : 集成算法中的学习率，又称为步长; 以控制迭代速率，常用于防止过拟合; 取值范围[0-1]默认值0.3
  3. max_depth 或者 gamma:  **树的最大深度**max_depth，默认6;
  4. 随机抽样参数(作用微乎其微)
  5. 正则化参数 (作用微乎其微)

- 常用参数列表

|                           参数含义                           |              xgb.train()  <br/>-- XGBoost 接口               |           xgb.XGBRegressor() <br/>-- sklearn 接口            |                             备注                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|                   **集成中弱评估器的数量**                   |                      num_round，默认10                       |                    n_estimators，默认100                     | num_round越大，模型的学习能力越强，模型也越容易过拟合。这个参数非常强大，常常能够一次性将模型调整到极限。 |
|                 训练中是否打印每次训练的结果                 |                      slient，默认False                       |                       slient，默认True                       |                                                              |
|         **随机抽样的时候抽取的样本比例，范围(0,1]**          |                       subsample，默认1                       |                       subsample，默认1                       | 每构建一个评估器，都让模型更加集中于数据集中容易被判错的那些样本。 |
| **集成算法中的学习率，又称为步长; 以控制迭代速率，常用于防止过拟合** |                  eta，默认0.3 取值范围[0,1]                  |                    learning_rate，默认0.1                    | 迭代决策树时的步长，又叫学习率。eta越大，迭代的速度越快，。 越小，越有可能找到更精确的最佳值，但迭代速度会比较缓慢。 |
|                     **选择弱评估器种类**                     | xgb_model<br>可以输入gbtree，gblinear或dart。输入的评估器不同，使用的params参数也不同，每种评估器都有自己的params列表。评估器必须于param参数相匹配，否则报错。 |         booster<br>可以输入gbtree，gblinear或dart。          | gbtree代表梯度提升树，dart是抛弃提升树，在建树的过程中会抛弃一部分树，比梯度提升树有更好的防过拟合功能。输入gblinear使用线性模型。 |
|                      **选用的损失函数**                      |                   obj：默认binary:logistic                   | **XGBRegressor**回归objective：默认reg:linear<br>**XGBClassififier**分类objective：默认binary:logistic | **reg:linear** 使用线性回归的损失函数，均方误差，回归时使用<br/>**binary:logistic** 使用逻辑回归的损失函数，对数损失log_loss，二分类时使用<br/>**binary:hinge** 使用支持向量机的损失函数，Hinge Loss，二分类时使用<br/>**multi:softmax** 使用softmax损失函数，多分类时使用 |
|                        L1正则项的参数                        |                alpha，默认0，取值范围[0, +∞]                 |              reg_alpha，默认0，取值范围[0, +∞]               |                                                              |
|                        L2正则项的参数                        |                lambda，默认1，取值范围[0, +∞]                |              reg_lambda，默认1，取值范围[0, +∞]              |                                                              |
|                        复杂度的惩罚项                        |               gamma, 默认为0 ,取值范围[0, +∞]                |                gamma，默认0，取值范围[0, +∞]                 |     在树的叶节点上进行进一步分枝所需的最小目标函数减少量     |
|                       **树的最大深度**                       |                       max_depth，默认6                       |                       max_depth，默认6                       |                                                              |
|                每次生成树时随机抽样特征的比例                |                   colsample_bytree，默认1                    |                   colsample_bytree，默认1                    |                                                              |
|             每次生成树的一层时随机抽样特征的比例             |                   colsample_bylevel，默认1                   |                   colsample_bylevel，默认1                   |                                                              |
|           每次生成一个叶子节点时随机抽样特征的比例           |                   colsample_bynode，默认1                    |                             N.A.                             |                                                              |
| 一个叶子节点上所需要的最小,即叶子节点上的二阶导数之和,类似于样本权重 |                   min_child_weight，默认1                    |                   min_child_weight，默认1                    |                                                              |
|            控制正负样本比例，表示为负/正样本比例             |                   scale_pos_weight，默认1                    |                   scale_pos_weight，默认1                    |                                                              |

#### 5.2 模型参考指标

- **衡量模型在未知数据上的准确率**的指标，叫做**泛化误差（Genelization error**）。泛化误差越小，模型就越理想。 mean_squared_error
- 评分 score()
  - 回归 ，R^2评估指标
  - 分类 , 返回预测的准确率
- accuracy_score 精确度
- recall_score  召回率
- f1_score   分类问题的一个衡量指标: 它是精确率和召回率的调和平均数，最大为1，最小为0
- auc    AUC的物理意义为任取一对例和负例，正例得分大于负例得分的概率，AUC越大，表明方法效果越好

#### 5.3 调参顺序

- XGBoost一般调参顺序:
  1. num_round(n_estimators)   : **集成中弱评估器的数量**, 越大越好,取值范围[0, +∞],默认值10
  2. eta : 集成算法中的学习率，又称为步长; 以控制迭代速率，常用于防止过拟合; 取值范围[0-1]默认值0.3
  3. max_depth 或者 gamma:  **树的最大深度**max_depth，默认6;
  4. 随机抽样参数(作用微乎其微)
  5. 正则化参数 (作用微乎其微)

#### 5.4 调参策略

##### 5.4.1 绘制学习曲线

- sklean API

  ```python
  from xgboost import XGBRegressor as XGBR
  from sklearn.ensemble import RandomForestRegressor as RFR
  from sklearn.linear_model import LinearRegression as LinearR
  from sklearn.datasets import load_boston
  from sklearn.model_selection import KFold, cross_val_score as CVS, train_test_split as TTS
  from sklearn.metrics import mean_squared_error as MSE  # 均方误差
  import pandas as pd
  import numpy as np
  import matplotlib.pyplot as plt
  from time import time
  import datetime
  data = load_boston()
  #波士顿数据集非常简单，但它所涉及到的问题却很多
  X = data.data
  y = data.target
  Xtrain,Xtest,Ytrain,Ytest = TTS(X,y,test_size=0.3,random_state=420)
  
  #如果开启参数slient：在数据巨大，预料到算法运行会非常缓慢的时候可以使用这个参数来监控模型的训练进度
  reg = XGBR(n_estimators=10,silent=True)#xgboost库silent=True不会打印训练进程，只返回运行结果，默认是False会打印训练进程
  #sklearn库中的xgbsoost的默认为silent=True不会打印训练进程，想打印需要手动设置为False
  CVS(reg,Xtrain,Ytrain,cv=5,scoring='neg_mean_squared_error').mean()#-92.67865836936579
  def plot_learning_curve(estimator,title, X, y, 
                          ax=None, #选择子图
                          ylim=None, #设置纵坐标的取值范围
                          cv=None, #交叉验证
                          n_jobs=None #设定索要使用的线程
                         ):
      
      from sklearn.model_selection import learning_curve
      import matplotlib.pyplot as plt
      import numpy as np
      
      train_sizes, train_scores, test_scores = learning_curve(estimator, X, y
                                                              ,shuffle=True
                                                              ,cv=cv
                                                              ,random_state=420
                                                              ,n_jobs=n_jobs)      
      if ax == None:
          ax = plt.gca()
      else:
          ax = plt.figure()
      ax.set_title(title)
      if ylim is not None:
          ax.set_ylim(*ylim)
      ax.set_xlabel("Training examples")
      ax.set_ylabel("Score")
      ax.grid() #绘制网格，不是必须
      ax.plot(train_sizes, np.mean(train_scores, axis=1), 'o-'
              , color="r",label="Training score")
      ax.plot(train_sizes, np.mean(test_scores, axis=1), 'o-'
              , color="g",label="Test score")
      ax.legend(loc="best")
      return ax
  
  cv = KFold(n_splits=5, shuffle = True, random_state=42) #交叉验证模式
  plot_learning_curve(XGBR(n_estimators=100,random_state=420)
                      ,"XGB",Xtrain,Ytrain,ax=None,cv=cv)
  plt.show()
  
  ```

- 细化学习曲线找出最佳的**n_estimators**

  ```python
  #细化学习曲线
  axisx = np.linspace(0.75,1,25)
  rs = []
  var = []
  ge = []
  for i in axisx:
      reg = XGBR(n_estimators=180,subsample=i,random_state=420)
      cvresult = CVS(reg,Xtrain,Ytrain,cv=cv)
      rs.append(cvresult.mean())
      var.append(cvresult.var())
      ge.append((1 - cvresult.mean())**2+cvresult.var())
  print(axisx[rs.index(max(rs))],max(rs),var[rs.index(max(rs))])
  print(axisx[var.index(min(var))],rs[var.index(min(var))],min(var))
  print(axisx[ge.index(min(ge))],rs[ge.index(min(ge))],var[ge.index(min(ge))],min(ge))
  rs = np.array(rs)
  var = np.array(var)
  plt.figure(figsize=(20,5))
  plt.plot(axisx,rs,c="black",label="XGB")
  plt.plot(axisx,rs+var,c="red",linestyle='-.')
  plt.plot(axisx,rs-var,c="red",linestyle='-.')
  plt.legend()
  plt.show()
  ```

  

##### 5.4.2 网格搜索

- 由于xgboost里参数太多,不推荐使用网格搜索;

- ```python
  #使用网格搜索来查找最佳的参数组合
  from sklearn.model_selection import GridSearchCV
  param = {"reg_alpha":np.arange(0,5,0.05),"reg_lambda":np.arange(0,2,0.05)}
  gscv = GridSearchCV(reg,param_grid = param,scoring = "neg_mean_squared_error",cv=cv)
  #======【TIME WARNING：10~20 mins】======#
  time0=time()
  gscv.fit(Xtrain,Ytrain)
  print(datetime.datetime.fromtimestamp(time()-time0).strftime("%M:%S:%f"))
  gscv.best_params_
  gscv.best_score_
  preds = gscv.predict(Xtest)
  from sklearn.metrics import r2_score，mean_squared_error as MSE
  r2_score(Ytest,preds)
  MSE(Ytest,preds) 
  #网格搜索的结果有什么样的含义呢？为什么会出现这样的结果？你相信网格搜索得出的结果吗？
  ```

  

##### 5.4.3 交叉验证(推荐)

- **交叉验证**（cross-validation 简称cv）将数据集分为k等份，对于每一份数据集，其中k-1份用作训练集，单独的那一份用作验证集。
  通常情况下，留一法对模型的评估可能会不准确，一般采用xgboost.cv可以进行交叉验证

- **XGBoost API**  (推荐)

  - xgb.cv()  主要是提供了一种交叉验证的方式

    ```python
    import xgboost as xgb
    from sklearn.datasets import load_boston
    data = load_boston()
    X = data.data
    y = data.target
    
    dfull = xgb.DMatrix(X,y)
    param1 = {'silent':True #并非默认
             ,'obj':'reg:linear' #并非默认
             ,"subsample":1
             ,"max_depth":6
             ,"eta":0.3
             ,"gamma":0
             ,"lambda":1
             ,"alpha":0
             ,"colsample_bytree":1
             ,"colsample_bylevel":1
             ,"colsample_bynode":1
             ,"nfold":5}
    num_round = 200
    time0 = time()
    cvresult1 = xgb.cv(param1, dfull, num_round)
    print(datetime.datetime.fromtimestamp(time()-time0).strftime("%M:%S:%f"))
    fig,ax = plt.subplots(1,figsize=(15,10))
    #ax.set_ylim(top=5)
    ax.grid()
    ax.plot(range(1,201),cvresult1.iloc[:,0],c="red",label="train,original")
    ax.plot(range(1,201),cvresult1.iloc[:,2],c="orange",label="test,original")
    ax.legend(fontsize="xx-large")
    plt.show()
    ```

    

- sklean api

  ```python
  from xgboost import XGBRegressor as XGBR
  from sklearn.ensemble import RandomForestRegressor as RFR
  from sklearn.linear_model import LinearRegression as LinearR
  from sklearn.datasets import load_boston
  from sklearn.model_selection import KFold, cross_val_score as CVS, train_test_split as TTS
  from sklearn.metrics import mean_squared_error as MSE  # 均方误差
  import pandas as pd
  import numpy as np
  import matplotlib.pyplot as plt
  from time import time
  import datetime
  data = load_boston()
  #波士顿数据集非常简单，但它所涉及到的问题却很多
  X = data.data
  y = data.target
  Xtrain,Xtest,Ytrain,Ytest = TTS(X,y,test_size=0.3,random_state=420)
  reg = XGBR(n_estimators=100).fit(Xtrain,Ytrain) #训练
  reg.predict(Xtest) # 传统接口predict
  reg.score(Xtest,Ytest) #你能想出这里应该返回什么模型评估指标么？利用shift+Tab可以知道，R^2评估指标
  MSE(Ytest,reg.predict(Xtest))#可以看出均方误差是平均值y.mean()的1/3左右，结果不算好也不算坏
  
  reg = XGBR(n_estimators=100) #交叉验证中导入的没有经过训练的模型
  CVS(reg,Xtrain,Ytrain,cv=5).mean()
  #这里应该返回什么模型评估指标，还记得么？ 返回的是与reg.score相同的评估指标R^2（回归），准确率（分类）
  #严谨 vs 不严谨
  CVS(reg,Xtrain,Ytrain,cv=5,scoring='neg_mean_squared_error').mean()
  
  
  #如果开启参数slient：在数据巨大，预料到算法运行会非常缓慢的时候可以使用这个参数来监控模型的训练进度
  reg = XGBR(n_estimators=10,silent=True)#xgboost库silent=True不会打印训练进程，只返回运行结果，默认是False会打印训练进程
  #sklearn库中的xgbsoost的默认为silent=True不会打印训练进程，想打印需要手动设置为False
  CVS(reg,Xtrain,Ytrain,cv=5,scoring='neg_mean_squared_error').mean()#-92.67865836936579
  def plot_learning_curve(estimator,title, X, y, 
                          ax=None, #选择子图
                          ylim=None, #设置纵坐标的取值范围
                          cv=None, #交叉验证
                          n_jobs=None #设定索要使用的线程
                         ):
      
      from sklearn.model_selection import learning_curve
      import matplotlib.pyplot as plt
      import numpy as np
      
      train_sizes, train_scores, test_scores = learning_curve(estimator, X, y
                                                              ,shuffle=True
                                                              ,cv=cv
                                                              ,random_state=420
                                                              ,n_jobs=n_jobs)      
      if ax == None:
          ax = plt.gca()
      else:
          ax = plt.figure()
      ax.set_title(title)
      if ylim is not None:
          ax.set_ylim(*ylim)
      ax.set_xlabel("Training examples")
      ax.set_ylabel("Score")
      ax.grid() #绘制网格，不是必须
      ax.plot(train_sizes, np.mean(train_scores, axis=1), 'o-'
              , color="r",label="Training score")
      ax.plot(train_sizes, np.mean(test_scores, axis=1), 'o-'
              , color="g",label="Test score")
      ax.legend(loc="best")
      return ax
  
  cv = KFold(n_splits=5, shuffle = True, random_state=42) #交叉验证模式
  plot_learning_curve(XGBR(n_estimators=100,random_state=420)
                      ,"XGB",Xtrain,Ytrain,ax=None,cv=cv)
  plt.show()
  ```

  



