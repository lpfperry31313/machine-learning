# 异常检测

* 我们要找到一个function，这个function要做的事情是：检测输入x的时，决定现在输入的x跟我们的训练数据是否相似
* Anomaly Detection这件事并不一定是找不好的结果，只是找跟训练数据不一样的数据
* 为什么不用binary classification？
    * 1. 没有办法知道整个异常的数据（Class2）是什么样的，所以不应该将异常的数据视为一个类别，应为它的变化太大了
    * 2. 很多情况下不太容易收集到异常的资料，收集正常的资料往往比较容易，收集异常的资料往往比较困难，比如fraud detection
* 异常检测分两类
    * 一、with labels
        * 用已有 X ~ label 训练出classifier，我们期待训练好一个classifier以后，机器有能力知道新给定的训练数据不在原本的训练数据中，它会给新的训练数据贴上“unknown”的标签
        * 方法一
            * 训练一个分类器，可以得出每类的predict score，然后计算confidence score（cross entropy），设定阈值判断测试数据是否异常
            * 用roc_auc、pr_auc、cost table等方法评估模型
            * 缺点：正常训练数据是猫狗，异常的老虎比猫更像猫，异常的狗比狗更像狗（狼: 狗 0.98 猫 0.02）
            * 解决：https://openreview.net/forum?id=ryiAv2xAZ
        * 方法二
            * 训练一个神经网络，直接输出confidence score
            * https://arxiv.org/abs/1802.04865
        * 方法三
            * 生成模型
            * https://arxiv.org/abs/1802.10560
    * 二、No labels （all clean / polluted）
        * 例子：银行收集了大量的交易记录，它把所有的交易记录都当做是正常的，然后告诉机器这是正常交易记录，然后希望机器可以借此侦测出异常的交易。但所谓的正常的交易记录可能混杂了异常的交易，只是银行在收集资料的时候不知道这件事。所以我们更多遇到的是：手上有训练资料，但我没有办法保证所有的训练资料都是正常的，可能有非常少量的训练资料是异常的。

    