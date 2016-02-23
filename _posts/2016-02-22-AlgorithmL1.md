---
layout: post
title:  算法导论LESSON 1
date:   2016-2-22 12:50:00
categories: 算法
tags: 算法导论学习笔记
---


**本文中含有数学公式，若无法显示请刷新重试，谢谢~**

# Lesson 1

## 插入排序

1 插入排序的基本原理（递增排序）

   取当前元素往前序寻找。依序比较，若前一元素比当前大，则被比较元素往后移动一位；以此类推，直至找到第一个比其小的元素，则停止寻找，这个位置的后一个就是应该放的位置。

2 插入排序的C++实现

``` 
void insertionsort(vector<int> sorted){
    int size = (int)sorted.size();
    //从第2项开始取为目标元素
    for(int j=1;j<size;j++){                        
        int key = sorted[j];
        //需要比较的是目标元素的前一项开始
        int k = j-1;      
        //向后移动的必要条件
        while(k>=0 && sorted[k]>key){             
            sorted[k+1] = sorted[k];
            k--;
        }
        //跳出循环，找到了应该在的位置
        sorted[k+1] = key;                        
    }
}
```

3 插入排序算法的分析

- 时间复杂度上界 $$O(n^2)$$


## 几个标记符号的数学定义

- 几个注意点： 

  - 渐进符号只关心两个函数在极限位置的大小状况。
  
  - 所有的渐进符号都代表着一种函数，为集合概念，『=』仅为方便记载，对应于集合论中『∈』符号，理论上前后不调换。

  - 性质：传递，自反，对称，转置对称

  - 若在等式或不等式中出现的时候，则表示这一类的匿名函数，若出现在等式右边，可以另设一个函数f(x)来替代这个符号所表示的位置。若出现在左边，则可用以下规则解释：

    > 无论怎样选择等号左边的匿名函数，总有一种方法来选择等号右边的匿名函数使等号成立。

    _Ex._ 
    $$
    2n^2+3n+1=2n^2+\Theta(n)=\Theta(n^2)
    $$

第一个等式：
$$ 
表明存在某个函数f(n) \in \Theta(n),使得对所有的n，有2n^2+3n+1=2n+f(n)。
$$
第二个等式：
$$
表明对于任意函数g(n)\in\Theta(n),存在h(x)\in\Theta(n^2),使得对所有的n，有2n^2+g(n)=h(n)。
$$
整体也蕴含着前项直接可以属于最后项（这个法则仅对有限项有效）

- 渐进上界
  $$
  O(g(n)) = \{f(n) ：\exists c>0,n_0>0 ;对于\forall n\ge n_0  都有0\le f(n) \le c·g(n)  \}
  $$
  - 渐进上界的计算方式：忽略所有低阶项和最高阶前之前的常数系数。

- 渐进下界
  $$
  \Omega (g(n)) = \{f(n) ：\exists c>0,n_0>0 ;对于\forall n\ge n_0  都有0\le c·g(n) \le f(n)  \}
  $$

- 渐进紧确界
$$
\Theta(g(n))=\{f(n):\exists 正常数 c_1,c_2,n_0;使得对\forall n\ge n_0,都有0\le c_1·g(n)\le f(n) \le c2·g(n) \}
$$

- o记号
   基本同O符号，但需要对所有c>0成立。

- ω记号
   基本同Ω符号，但需要对所有c>0成立



## 归并排序

1 归并排序的基本思想：分治法(Divide and Conquer)

2 归并排序的C++实现：

```
vector<int> sorted;
//归并操作
void merge(int left,int mid,int right){           
  //将左右两边分别放入新建的数组当中，方便进行归并操作
    int n1 = mid-left+1;            
    int n2 = right-mid;
    vector<int>L,R;
    L.clear();
    R.clear();
    for(int i=1;i<=n1;i++){
        L.push_back(sorted[left+i-1]);
    }
    for(int i=1;i<=n2;i++){
        R.push_back(sorted[mid+i]);
    }
  //归并排序，利用子序列已经排完序的性质，只需要比较首元素即可
    int i=0,j=0;
    for(int k=left;k<=right;k++){
        if(L[i]<=R[j]){
            sorted[k] = L[i];
            i++;
        }else{
            sorted[k] = R[j];
            j++;
        }
  //不使用哨兵，利用下标到达界限判断
        if(i==L.size()){
            k++;
            for(int u=j;u<R.size();u++){
                sorted[k] = R[u];
                k++;
            }
            break;
        }
        if(j==R.size()){
            k++;
            for(int u=i;u<L.size();u++){
                sorted[k] = L[u];
                k++;
            }
            break;
        }
    }
}
//执行归并函数所需要调用的入口
void mergesort(int leftlim,int rightlim){
  //若大于等于则表示已经只有一个元素，不需要进行递归进入下一步
    if(leftlim<rightlim){
        int mid = (leftlim+rightlim)/2;
      //左侧递归进行归并
        mergesort(leftlim,mid);
      //右侧递归进行归并
        mergesort(mid+1,rightlim);
      //合并
        merge(leftlim,mid,rightlim);
    }
}
```

3 归并排序的递归式
$$
T(n) = 2T(n/2)+\Theta(n)     (n > 1)
$$

$$
T(n) = \Theta(1) (n=1)
$$

4 递归式的时间复杂度分析

这里利用递归树的方法。具体在L2中解释，利用这个方法可以得到归并排序的时间复杂度为
$$
O(nlgn)
$$
可知归并排序在更大规模时比插入排序更优。
