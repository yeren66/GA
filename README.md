# 遗传算法解排考问题

解决问题流程
1. 刚开始考虑使用遗传算法解决该问题的初始解，但效果很差。同时对问题的认识，数据的处理以及解的形式都不太了解。
2. 在同学们的启发下，使用带模拟退火的贪心算法求解该问题的初始解，初始解问题解决。同时开始使用冲突表等表格来记录数据，代码效率得到了提升。对数据进行了一定处理，也与大家达成了一个大致相同的评估函数，用于评估排考方案的优劣。
3. 考虑使用遗传算法在初始解的基础上迭代出更好的解，效果依旧很差。原因是该问题的解的形式较为灵活，遗传算法在交叉遗传过程中极易出现不稳定的解，即没有找到一种交叉遗传算子可以稳定的交叉不同解的优势基因，从而导致算法不收敛，遗传算法退化为随机寻找。
4. 后开始考虑退而求其次，使用（2）中的带模拟退火的贪心算法，并加上约束条件，多次运行求局部最优解。
5. 对整个流程进行梳理总结，对结果数据进行处理，指标设定，以及数据可视化。
评估函数
间隔场次设置为4（可变），即两次考试间隔大于4场不计入评估函数。
这项评估函数考虑了每个学生的排考情况，以及每个场次的学生总人数安排情况。
越小，即每个学生的所有考试间隔越大，各场次学生数越均衡，认为该排考方案越好。
数据处理
1.  conflict.csv  
 白：0；绿：1~30；黄：31~100；红：100+；（max = 652）
该冲突表横纵坐标均为课程id，交叉点表示两课程的冲突人数，可以看出图较为稀疏，即可行解很多。
2. students_number.csv
3. decode.csv
学生人数表记录了每个课程的总学生数，解码表记录了课程id以及对应的课程encode(name)。
代码实现
见GA_adapt文件夹
代码结构如上，其中Course，Scheme，Student分别描述了课程，排考方案，学生的各实例，Public主要用于调整超参数，GA_adapt是遗传算法主体，Greedy_arithmetic是贪心算法主体，其他部分均用于读写及数据处理。
主要的超参数有：
a. 每场次最大课程数 = 100 
b. 每场次限制学生人数 = 1400
c. 场次数 = 56
d. 变异概率（p）= 0.6
关于p值的选取，我进行了如下实验：
📎rate.csv
rate表记录了不同的p值（变异概率）以及对应的rate，score，time_cost
可以看出，p值越小，每次越容易生成一个可行的排课方案，所需时间也越少（迭代次数增多），但方案的满意度较差，p值越大，越难找到可行的排课方案，所需时间也越多，但方案满意度较好。
基于以上原因，我选择了p=0.6作为超参数，并以此生成了三个样例解决方案。
样例解决方案
📎result_decode_1(score=71.03116139999989).csv📎result_decode_2(score=71.10364919999985).csv📎result_decode_3(score=71.9353452666664).csv📎result_encode_1(score=71.03116139999989).csv📎result_encode_2(score=71.10364919999985).csv📎result_encode_3(score=71.9353452666664).csv
其中encode、decode表示是否解码，score表示该方案的得分。
