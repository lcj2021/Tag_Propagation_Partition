# Tag_Propagation_Partition



## Algorithm

参数：-p 划分块数

### 0 建图

### 1 random_tag

随机选取$p \times 5$ 个结点，给它们随机打上[1, p]的tag。这些结点加入`queue`作为第一轮bfs_walk的源点

### 2 bfs_walk

分r轮进行，终止条件为图上所有结点的tag均能覆盖其邻居的tag。遍历时会忽略度数为1的结点。每轮每个结点只会遍历一次（vis数组记录），每次遍历只会打1个tag。

#### 第1轮：

以`1阶段`的种子结点作为起始点作多源BFS，遍历到一个点（记为top）时，考察top的还未被覆盖的邻居结点的`tag_list`，从中选取其邻居中tag最多的，（若邻居最多的tag不唯一）当前已分配最少的tag分配给`top`。

分配完成对`top`的出边进行检查，若更新tag后的`top`能覆盖其某邻居，则将对应边（st数组记录）置位1。往后的遍历将不再考察该边。若`top`成功覆盖其所有邻居，则将`covered_cnt+1`；若仍未成功，则加入下一轮的遍历队列中。

#### 第2~r轮

源点来自于上一轮未能覆盖其邻居的结点。其余分配、检查逻辑均与第1轮相同。

### 3 degree_1_vertex_assignment

将度数为1的节点打上其邻居的tag，分配其当前已分配最少的tag

### 4 union_tag

将p个tag进行合并，以求达到tag间规模的balance

## Build
```shell
mkdir release
cd release && cmake ../
make
```

## Run
```shell
./main -p 8 -filename ../dataset/mini.txt
./main -p 8 -filename ../dataset/com-amazon.ungraph.txt
./main -p 16 -filename ../dataset/com-lj.ungraph.txt
```



# Statistic
- The datasets are from [snap](http://snap.stanford.edu/data/index.html)

[LJ](http://snap.stanford.edu/data/com-LiveJournal.html)

| p    | VERTEX_CNT | ALL_TAG_CNT | Time    |
| ---- | ---------- | ----------- | ------- |
| 8    | 3997962    | 7827080     | 35.0114 |
| 16   | 3997962    | 9115827     | 46.0436 |
| 32   | 3997962    | 9053842     | 47.9411 |
| 128  | 3997962    | 9877578     | 68.9839 |
| 256  | 3997962    | 9732271     | 82.4134 |
| 512  | 3997962    | 10944786    | 123.017 |



[Amazon](http://snap.stanford.edu/data/com-Amazon.html)

| p    | VERTEX_CNT | ALL_TAG_CNT | Time    |
| ---- | ---------- | ----------- | ------- |
| 8    | 334863     | 557979      | 1.01654 |
| 16   | 334863     | 651342      | 1.10861 |
| 32   | 334863     | 709462      | 1.43612 |
| 128  | 334863     | 762469      | 1.71044 |
| 256  | 334863     | 781182      | 2.2125  |
| 512  | 334863     | 794393      | 2.93649 |



[Orkut](http://snap.stanford.edu/data/com-Orkut.html)

| p    | VERTEX_CNT | ALL_TAG_CNT | Time    |
| ---- | ---------- | ----------- | ------- |
| 8    | 3072441    | 6507625     | 131.083 |
| 16   | 3072441    | 7768115     | 178.266 |
| 32   | 3072441    |             |         |
| 128  | 3072441    | 8741467     | 255.466 |
| 256  | 3072441    | 8726848     | 286.928 |
| 512  | 3072441    | 9174221     | 383.242 |


# Results Comparison on Ubuntu 18.04 (Single 128Gb Memory Machine)

## NE 

- The results of Neighborhood Expansion (NE) algorithm.
[Graph Edge Partitioning via Neighborhood Heuristic](http://www.kdd.org/kdd2017/papers/view/graph-edge-partitioning-via-neighborhood-heuristic)

[LJ](http://snap.stanford.edu/data/com-LiveJournal.html)

| p    | VERTEX_CNT | Time    | RF      |
| ---- | ---------- | ------- | ------- |
| 8    | 3997962    | 24.3755 | 1.31405 |
| 16   | 3997962    | 25.3646 | 1.43483 |
| 32   | 3997962    | 31.9216 | 1.56744 |
| 128  | 3997962    | 76.6040 | 1.89865 |
| 256  | 3997962    | 146.308 | 2.08690 |
| 512  | 3997962    | 280.972 | 2.31126 |


[Orkut](http://snap.stanford.edu/data/com-Orkut.html)

| p    | VERTEX_CNT | ALL_TAG_CNT | Time    | RF      |
| ---- | ---------- | ----------- | ------- | ------- |
| 8    | 3072441    | 6507625     | 75.3679 | 1.74282 |
| 16   | 3072441    | 7768115     | 84.4289 | 2.05309 |
| 32   | 3072441    |             | 114.714 | 2.51501 |
| 128  | 3072441    | 8741467     | 263.342 | 3.75545 |
| 256  | 3072441    | 8726848     | 457.818 | 4.56794 |
| 512  | 3072441    | 9174221     | 871.835 | 5.60136 |

## TP

TBD



# Results Comparison on Ubuntu 20.04 (Single 16Gb Memory Machine)

[LJ](http://snap.stanford.edu/data/com-LiveJournal.html)

| p    | VERTEX_CNT | Time(NE) | RF(NE)  | Time(Tag) | RF(Tag)     |
| ---- | ---------- | -------- | ------- | --------- | ----------- |
| 8    | 3997962    | 29.4255  | 1.32195 | 35.0114   | 1.957767483 |
| 16   | 3997962    | 20.0249  | 1.43483 | 46.0436   | 2.2807      |
| 32   | 3997962    | 23.8069  | 1.56446 | 47.9411   | 2.2652      |
| 128  | 3997962    | 46.7652  | 1.90293 | 68.9839   | 2.4714      |
| 256  | 3997962    | 73.5395  | 2.09239 | 82.4134   | 2.4348      |
| 512  | 3997962    | 133.925  | 2.29503 | 123.017   | 2.7383      |

[Orkut](http://snap.stanford.edu/data/com-Orkut.html)

| p    | VERTEX_CNT | Time(NE) | RF(NE)  | Time(Tag) | RF(Tag)     |
| ---- | ---------- | -------- | ------- | --------- | ----------- |
| 8    | 3072441    | 106.484  | 1.72397 | 131.083   | 2.118063455 |
| 16   | 3072441    | 74.4747  | 2.07452 | 178.266   | 2.528320316 |
| 32   | 3072441    | 82.045   | 2.50314 | 182.723   | 2.5078125   |
| 128  | 3072441    | 152.228  | 3.74753 | 255.466   | 2.845121192 |
| 256  | 3072441    | 232.683  | 4.54942 | 286.928   | 2.840363086 |
| 512  | 3072441    | 398.487  | 5.52626 | 383.242   | 2.985971415 |

## 