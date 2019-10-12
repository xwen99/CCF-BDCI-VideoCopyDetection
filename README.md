# CCF-BDCI 视频拷贝检测

## Baseline 思路
1. 并行提取视频关键帧；
2. 通过resnet18提取关键帧特征；
3. 通过通过CNN特征计算得到query与refer对应关系；
4. 视频侵权时间段还需要进一步分析；

## 数据说明

训练数据集分为3个部分：

* query文件夹，其中包括3000个视频，为侵权视频训练集，格式为mp4，文件名为视频id，例如：b394c1e0-afd9-11e9-a9d1-fa163ee49799.mp4,其中b394c1e0-afd9-11e9-a9d1-fa163ee49799为视频id，与文件train.csv中字段对应

* refer文件夹，其中包括200个视频，为版权长视频视频集，格式为mp4，文件名为视频id，例如，2528707200.mp4，2528707200表示视频id，与文件train.csv中字段对应

* train.csv文件，记录侵权视频和版权长视频对应的关系及具体匹配时间，其中每列有8个空格分隔，具体字段说明参见下表：

| 列名             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| query_id         | 侵权视频id，对应query文件夹中视频名称                        |
| query_time_range | 侵权视频的具体侵权时间段，起止时间用逗号分隔，时间单位毫秒   |
| refer_id         | 侵权视频所侵权的版权视频id，对应refer文件夹中视频名称        |
| refer_time_range | 侵权视频所侵权的版权视频的具体时间段，起止时间用逗号分隔，时间单位毫秒 |

测试数据集分为2个部分：

• query文件夹，其中包括1500个视频，为侵权视频测试集，由训练集refer文件夹长视频生成，格式为mp4，文件名为视频id，例如：b394c1e0-afd9-11e9-a9d1-fa163ee49799.mp4,其中b394c1e0-afd9-11e9-a9d1-fa163ee49799为视频id，与文件evaluation_public.csv中字段对应

• evaluation_public.csv文件，测试集，没有标注结果，用于提交模型竞赛结果，具体字段说明如下：

| 列名     | 说明                                        |
| -------- | ------------------------------------------- |
| query_id | 可能侵权视频id，id对应query文件夹中视频名称 |

## 文件路径

..  
├── code  
│   ├── baseline  
│   ├── CBVR  
│   └── ccf_video_copy_detection.ipynb  
├── download  
│   ├── submit_example.csv  
│   ├── test_query.tar.gz  
│   ├── train.csv  
│   ├── train_query.tar.gz  
│   └── train_refer.tar.gz  
├── test  
│   ├── query  
│   └── submit_example.csv  
└── train  
    ├── query  
    ├── query_frame  
    ├── refer  
    ├── refer_frame  
    └── train.csv  

其中`query`和`refer`为训练数据文件夹，而`query_frame`和`refer_frame`分别为提取的关键帧的存放文件夹。