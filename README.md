# CCF-BDCI 视频拷贝检测

## 当前思路
1. 提取视频关键帧；
2. 通过resnet18提取关键帧特征；
3. 对特征进行~~PCA降维~~（失败中）和L2正则化；
4. 所有视频两两计算得相似度矩阵（余弦相似度）；
5. 对于相似度top-K视频对，进行帧级匹配（按相似度建图，跑最长路）。

## 一些经验
1. 特征不宜过细，采用resnet50提取特征的效果比resnet18差10~20个点；
2. 当前算法对参数比较敏感，目前取相似度前K=20视频进行帧级匹配，帧级匹配阶段，帧间相似度阈值0.85，最大跨度为10帧；
3. 主要瓶颈在于视频级匹配，只要目标视频落入Top-K视频，基本可以得到正确的帧匹配；
4. query与refer抽帧密度接近可能较好，也可能是抽帧不易过密。进行了query一秒五帧，refer一秒一帧与它们都一秒一帧两组测试，结果一秒一帧不仅运行速度快，而且得分大大高于另一组。

## TODO

1. 细粒度抽帧（当前1s抽一帧，感觉已经足够了）；
2. 代码重构（还差video_retrieval）；
3. 继续case analysis（不同视频，相同位置、角度与表情的大妈和男生的相似度竟然有85%，特征提取要继续研究）。

## 当前成绩

* 初赛（最大误差5s）得分：F1-score = 0.60054720，排名：第10名
* 复赛（最大误差3s）得分：F1-score = 0.47160494，排名：第8名

## 参考

[1] Tan H K , Ngo C W , Hong R , et al. Scalable detection of partial near-duplicate videos by visual-temporal consistency[C]// Proceedings of the 17th International Conference on Multimedia 2009, Vancouver, British Columbia, Canada, October 19-24, 2009. ACM, 2009.

[2] Jiang Y G , Jiang Y , Wang J . VCDB: A Large-Scale Database for Partial Copy Detection in Videos[M]// Computer Vision – ECCV 2014. Springer International Publishing, 2014.

[3] 顾佳伟, 赵瑞玮, 姜育刚. 视频拷贝检测方法综述[J]. 计算机研究与发展, 2017(6).

[4] 刘红. 一种基于图的近重复视频子序列匹配算法[J]. 计算机应用研究, 2013(12):343-348.

[5] FFmpeg视频抽帧那些事 - 阿水的文章 - 知乎 https://zhuanlan.zhihu.com/p/85895180