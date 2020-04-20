
# 面试中算法总结
## 1.排序和查找算法
### (1)快速排序
#### 方法描述
```java<br>
快速排序：是一种分区交换排序算法。采用分治策略对两个子序列再分别进行快速排序，是一种递归算法。

法描述：在数据序列中选择一个元素作为基准值，每趟从数据序列的两端开始交替进行，将小于基准值的元素交换到序列前端，将大于基准值的元素交换到序列后端，介于两者之间的位置则成为了基准值的最终位置。同时，序列被划分成两个子序列，再分别对两个子序列进行快速排序，直到子序列的长度为1，则完成排序。
```
#### 举例说明
```java<br>
举例：假设要排序的数组是a[6],长度为7，首先任意选取一个数据（通常选用第一个数据）作为关键数据，然后将所有比它的数都放到它前面，所有比它大的数都放到它后面，这个过程称为一躺快速排序。一躺快速排序的算法是：

1.设置两个变量i，j，排序开始的时候i=0；j=6；
2.以第一个数组元素作为关键数据，赋值给key，即key=a[0]；
3.从j开始向前搜索，即由后开始向前搜索（j--），找到第一个小于key的值，两者交换；
4.从i开始向后搜索，即由前开始向后搜索（i++），找到第一个大于key的值，两者交换；
5.重复第3、4步，直到i=j；此时将key赋值给a[i]；

例如：待排序的数组a的值分别是：（初始关键数据key=49）

            a[0] a[1] a[2] a[3] a[4] a[5] a[6]<br>
            `49`  38   65   97   76   13   27
第1次交换后: 27   38   65   97   76   13  `49`
第2次交换后: 27   38  `49`  97   76   13   65
第3次交换后: 27   38   13   97   76  `49`  65
第4次交换后: 27   38   13  `49`  76   97   65
此时完成了一趟循环，将49赋值给a[3]，数据分为三组，分别为{27,38,13}{49}{76,96,65}，利用递归，分别对第一组和第三组进行排序，则可得到一个有序序列，这就是快速排序算法。
```
#### 代码
```java<br>
快速排序代码如下：
public void sort(int a[], int low, int high){    
    int i=low;    
    int j=high;    
    int key=a[low];    
 
    if (low < high){    
        while(i<j){ // 此处的while循环结束，则完成了元素key的位置调整    
            while(i<j&&key<=a[j]){    
                j--;    
            }    
            a[i]=a[j];    
            while(i<j&&key>=a[i]){    
                i++;    
            }    
            a[j]=a[i];    
            a[i]=key;  //此处不可遗漏  
        }     
        sort(a,low,i-1);    
        sort(a,i+1,high);    
    }    
}
```
### (2)二分查找
#### 方法描述
```java<br>

定义：二分查找又称折半查找，优点是比较次数少，查找速度快，平均性能好；其缺点是要求待查表为有序表，且插入删除困难。因此，折半查找方法适用于不经常变动而查找频繁的有序列表。

步骤：首先，假设表中元素是按升序排列，将表中间位置记录的关键字与查找关键字比较，如果两者相等，则查找成功；否则利用中间位置记录将表分成前、后两个子表，如果中间位置记录的关键字大于查找关键字，则进一步查找前一子表，否则进一步查找后一子表。重复以上过程，直到找到满足条件的记录，使查找成功，或直到子表不存在为止，此时查找不成功。

算法前提：必须采用顺序存储结构；必须按关键字大小有序排列。

实现方式：包含递归实现和非递归实现两种方式。
```
#### 递归代码
```java<br>
二分查找的递归代码实现如下：
private  int halfSearch(int[] a,int left,int right,int target) {  
     int mid=(left+right)/2;  
     int midValue=a[mid];  
     if(left<=right){  
         if(midValue>target){  
             return halfSearch(a, left, mid-1, target);  
         }else if(midValue<target) {  
             return halfSearch(a, mid+1, right, target);  
         }else {  
             return mid;  
         }  
    }  
    return -1;  
}
```
#### 非递归代码
```java<br>
非递归实现代码如下：

private  int halfSearch(int[] a,int target){  
    int i=0;  
    int j=a.length-1;  
    while(i<=j){  
        int mid=(i+j)/2;  
        int midValue=a[mid];  
        if(midValue>target){  
            j=mid-1;  
        }else if(midValue<target){  
            i=mid+1;  
        }else {  
            return mid;  
        }  
    }  
    return -1;  
}
```

## 2.单链表相关算法
### 单链表定义
```java<br>
常见的链表题有：单链表反转、合并有序单链表、求单链表的中间节点、判断单链表相交或者有环、求出进入环的第一个节点、求单链表相交的第一个节点等。
我们首先给出单链表的定义：用代码实现。
class Node{  
    int val;  
    Node next;  
    public Node(int val){  
         this.val=val;  
    }
}  
```
### 单链表反转
#### 单链表反转的定义及步骤
```java<br>
单链表反转：比如1→2→3→4→5，反转之后返回5→4→3→2→1

步骤：
1.从头到尾遍历原链表，每遍历一个结点
2.将其摘下放在新链表的最前端。
3.注意链表为空和只有一个结点的情况。
```
#### 单链表反转的代码
```java<br>
代码实现如下：
public static Node reverseNode(Node head){  
      // 如果链表为空或只有一个节点，无需反转，直接返回原链表表头  
      if(head == null || head.next == null)  
          return head;  
 
      Node reHead = null;  
      Node cur = head;  
      while(cur!=null){  
          Node reCur = cur;      // 用reCur保存住对要处理节点的引用  
          cur = cur.next;        // cur更新到下一个节点  
          reCur.next = reHead;   // 更新要处理节点的next引用  
          reHead = reCur;        // reHead指向要处理节点的前一个节点  
      }  
      return reHead;  
 }
```
### 合并有序的单链表
#### 举例
```java<br>
合并有序的单链表：给出两个分别有序的单链表，将其合并成一条新的有序单链表。

举例：1→3→5和2→4→6合并之后为1→2→3→4→5→6 

步骤：首先，我们通过比较确定新链表的头节点，然后移动链表1或者链表2的头指针，然后通过递归来得到新的链表头结点的next。
```
#### 代码
```java<br>
代码实现如下：
public static Node mergeList(Node list1 , Node list2){  
    if(list1==null)  
        return list2;  
    if(list2==null)  
        return list1;  
    Node resultNode;  
    if(list1.val<list2.val){ // 通过比较大小，得到新的节点 
        resultNode = list1;  
        list1 = list1.next;  
    }else{  
        resultNode = list2;  
        list2 = list2.next;  
    }  
    // 递归得到next
    resultNode.next = mergeList(list1, list2);  
    return resultNode;  
}
```
## 3.二叉树相关算法
### 二叉树定义
```java<br>
二叉树相比单链表，会有更多的指针操作，如果面试官想进一步考察应聘者指针操作，那么二叉树无疑是理想的考题。二叉树常见的考题包括：分层遍历（宽度优先遍历. 前序遍历、中序遍历、后序遍历以及求二叉树中两个节点的最低公共祖先节点。

我们首先定义二叉树这种数据结构，代码实现如下：

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    public TreeNode(int val) {
        this.val = val;
    }
}
```
### 二叉树举例
```java<br>
面试中，对分层遍历二叉树考察最多。举例如下，针对如下所示的二叉树。

```
```java<br>
```



