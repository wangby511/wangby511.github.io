---
layout:     post
title:      蚌埠南站
subtitle:   System Design
date:       2019-01-27
author:     wangby511
header-img: img/post-bg-xbly2.jpg
catalog: true
tags:
    - PERSONAL
---


## MapReduce

Input

Split

Map 拆解

Shuffle

Reduce 组合

Finalize

#### 统计单词出现次数
```
Map(string key,string value){
    //key: the id of a line
    //value: the content of a line
    for eachword in value:
       OutputTemp(word,1);
}

Reduce(string key,list valuelist){
    //key: the name of a word
    //valuelist: the appearance of a word
    int sum = 0;
    for value in valuelist:
       sum += value;
    OutputFinal(key,sum);
}
```

#### build inverted index
```
Map(string key,string value){
    //key: the id of a document
    //value: the content of the document
    for eachword in value:
       OutputTemp(word,key);
}

Reduce(string key,list valuelist){
    //key: the name of a word
    //valuelist: the appearances of this word in documents
    List sumlist;
    for value in valuelist:
       sumlist.append(value);
    OutputFinal(key,sumlist);
}
```
## Google File System

#### 架构层次

（上）应用1，2，3

     算法（MapReduce)

     数据模型(Big Table)

（下）文件系统（GFS)

#### 保存超大文件

Master

ChunkServer1,2,3...

Master 包含Metadata(Info and Index)

ChunkServer 包含Index(diskOffset)和具体Chunks

1 block = 64KB 保存检验和checksum（32bit）

#### 减少ChunkServer挂掉带来的损失

保存3个副本

Chunk03 -> CS3,CS4,CS5

Chunk26 -> CS5,CS7,CS6

...

若ChunkServer04的chunk03损坏，通知master得到其他copies后，可以向ChunkServer04，05请求。

若ChunkServer04损坏 (master记录每个ChunkServer正常timstamp时间，ping不同且很久没回应),

master启动修复进程 workinglist，寻找所有含有CS4的chunk（此时应该少于三个副本），按照存活副本数量从小到大排序修复（按应急）。

若出现热点，加入热点平衡进程.

记录每个chunk访问信息和ChunkServer信息

Chunk stats

Chunk00,access frequency = 100

Chunk01,access frequency = 50

...

Server stats

ChunkServer01, free space = 21%, free bandwidth = 15%

ChunkServer02, free space = 50%, free bandwidth = 21%

...

当副本过度繁忙，复制到更多ChunkServer，基于ChunkServer的硬盘和带宽利用率来选择.

#### 读操作
![](https://raw.githubusercontent.com/wangby511/wangby511.github.io/master/images/2019-01-28.12.08.56.png)

#### 写操作
![](https://raw.githubusercontent.com/wangby511/wangby511.github.io/master/images/2019-01-28.12.09.09.png)


























