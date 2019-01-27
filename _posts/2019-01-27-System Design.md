---
layout:     post
title:      蚌埠南站
subtitle:   System Design : MapReduce
date:       2019-01-27
author:     wangby511
header-img: img/ppost-bg-xbly2.jpeg
catalog: true
tags:
    - PERSONAL
---

# MapReduce

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
