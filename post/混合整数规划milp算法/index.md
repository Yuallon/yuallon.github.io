
###前言

今天公司销售部的小伙伴让我帮忙写一下 **混合整数规划算法** 解释文档，用于发给客户那边。简单说一下为何用这个算法，因为公司接了某个活猪屠宰公司的数据分析项目，用于指导该公司活猪屠宰的方式（白条、猪蹄等不同部位）和屠宰的数量。公司根据客户的需求给出了 **混合整数规划算法**。顾将自己的学习记录于此。

###混合整数规划算法

在解释 **混合整数线性规划模型（MILP）** 前，先要解释一下什么是 **线性规划模型（Linear Programming LP）**。

线性规划模型：它指的是目标函数是线性的，所有的约束也是线性的，决策变量可以取任何实数。

举一个经典的饮食问题例子：超市里面有卖3种食品，玉米，牛奶和面包。价格、维他命A和卡路里信息见下表：

![混合整数规划图1](/混合整数规划图1.png)

现在的问题是买多少份的玉米，牛奶，面包，使得总价格最低，而维他命A的总摄取量不小于500但不大于50000，卡路里的总摄取量不小于2000但不大于2250。­­­­­

![混合整数规划图2](/混合整数规划图2.jpg)

现在回到之前的问题，如果在线性规划问题中有**部分**决策变量，比如上面的X_corn要求必须是整数， 那么这时的规划问题就转变成混合整数线性规划问题了。

**数学建模流程：**

- 第一步：假设决策变量，如上面，玉米、牛奶、面包。
- 第二步：建立目标函数，如上面，方程1。
- 第三步：寻求约束条件，如上面，方程2，3，4，5，6。
- 第四步：根据目标函数和约束条件求解方程组。

### 参考资料

- [什么是混合整数线性规划(MILP)模型？](https://www.zhihu.com/question/25560707?sort=created)



