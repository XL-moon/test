
## 单链表相关算法
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
### 链表基础题大全
#### (1)求单链表中结点的个数
```java<br>
package com.lianbiao.test;
/*
 * 求单链表中结点的个数: getListLength
 */
public class Main {
     public static void main(String[] args) {
          Node head = new Node(3);
          Node node1 = new Node(5);
          Node node2 = new Node(6);
          Node node3 = new Node(9);
          Node node4 = new Node(7);
          Node node5 = new Node(2);
          Node node6 = new Node(1);
          head.next = node1;
          node1.next=node2;
          node2.next=node3;
          node3.next=node4;
          node4.next=node5;
          node5.next=node6;
          
          int listLength = getListLength(head);
          System.out.println(listLength);
     }
     // 打印链表的方法，方便test函数
     public static void printList(Node head) { 
        while (head != null) { 
            System.out.print(head.val + " "); 
            head = head.next; 
        } 
        System.out.println(); 
    }
     
     /*
      * 求单链表中结点的个数 
      * 注意检查链表是否为空。时间复杂度为O（n） 
      */
    public static int getListLength(Node head) { 
        // 注意头结点为空情况 
        if (head == null) { 
            return 0; 
        } 
 
        int len = 0; 
        Node cur = head; 
        while (cur != null) { 
            len++; 
            cur = cur.next; 
        } 
        return len; 
    }
     
}
// 建立链表结构类 val和next
class Node{
     
     int val;
     Node next;
     public Node(int val){
          this.val=val;
          this.next=null;
     }
}
```
##### (1)运行结果
```java<br>
7
```
#### (2)单链表反转：遍历和递归
```java<br>
package com.lianbiao.test;
import java.util.Stack;
/*
 * 单链表反转：遍历和递归
 */
public class Main {
     public static void main(String[] args) {
          Node head = new Node(3);
          Node node1 = new Node(5);
          Node node2 = new Node(6);
          Node node3 = new Node(9);
          Node node4 = new Node(7);
          Node node5 = new Node(2);
          Node node6 = new Node(1);
          head.next = node1;
          node1.next=node2;
          node2.next=node3;
          node3.next=node4;
          node4.next=node5;
          node5.next=node6;
          
          printList(head);
//        printList(reverseNode(head));
          printList(reverseNodeRec(head));
          
     }
     // 打印链表的方法，方便test函数
     public static void printList(Node head) { 
        while (head != null) { 
            System.out.print(head.val + " "); 
            head = head.next; 
        } 
        System.out.println(); 
    }
     
     /*
      *  翻转链表（遍历） 
      *  从头到尾遍历原链表，每遍历一个结点，
      *  将其摘下放在新链表的最前端。
      *  注意链表为空和只有一个结点的情况。时间复杂度为O（n）
      */
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
     
     // 翻转递归（递归） 
    // 递归的精髓在于你就默认reverseListRec已经成功帮你解决了子问题了！但别去想如何解决的 
    // 现在只要处理当前node和子问题之间的关系。最后就能圆满解决整个问题。 
    /*
         head
            1 -> 2 -> 3 -> 4
        
          head
            1--------------
                                |
                   4 -> 3 -> 2                            // Node reHead = reverseListRec(head.next);
               reHead      head.next
            
                   4 -> 3 -> 2 -> 1                    // head.next.next = head;
               reHead
               
                    4 -> 3 -> 2 -> 1 -> null            // head.next = null;
               reHead
     */ 
    public static Node reverseNodeRec(Node head){ 
        if(head == null || head.next == null){ 
            return head; 
        } 
         
        Node reHead = reverseNodeRec(head.next);     
        head.next.next = head;      // 把head接在reHead串的最后一个后面 
        head.next = null;               // 防止循环链表 
        return reHead; 
    }
     
     
     
}
// 建立链表结构类 val和next
class Node{
     
     int val;
     Node next;
     public Node(int val){
          this.val=val;
          this.next=null;
     }
}
```
##### (2)运行结果
```java<br>
3 5 6 9 7 2 1
1 2 7 9 6 5 3 
```
#### (3)从尾到头打印链表：遍历和递归
```java<br>
package com.lianbiao.test;
import java.util.Stack;
/*
 * 从尾到前打印链表：遍历和递归
 */
public class Main {
     public static void main(String[] args) {
          Node head = new Node(3);
          Node node1 = new Node(5);
          Node node2 = new Node(6);
          Node node3 = new Node(9);
          Node node4 = new Node(7);
          Node node5 = new Node(2);
          Node node6 = new Node(1);
          head.next = node1;
          node1.next=node2;
          node2.next=node3;
          node3.next=node4;
          node4.next=node5;
          node5.next=node6;
          
          // 反转前打印链表
          printList(head);
          reversePrintListRec(head);
          // reversePrintListStack(head);
     }
     // 打印链表的方法，方便test函数
     public static void printList(Node head) { 
        while (head != null) { 
            System.out.print(head.val + " "); 
            head = head.next; 
        } 
        System.out.println(); 
    }
     
     /**
     * 从尾到头打印单链表
     * 对于这种颠倒顺序的问题，我们应该就会想到栈，后进先出。所以，这一题要么自己使用栈，要么让系统使用栈，也就是递归。注意链表为空的情况
     * 。时间复杂度为O（n）
     */ 
    public static void reversePrintListStack(Node head) { 
     Stack<Node> stack =new Stack<Node>();
     Node curNode = head;
     while(curNode!=null){
          stack.push(curNode);
          curNode=curNode.next;
     }
     while(!stack.isEmpty()){
          curNode = stack.pop();
          System.out.print(curNode.val+" ");
     }
     System.out.println();
    } 
    /**
     * 从尾到头打印链表，使用递归（优雅！）
     */ 
    public static void reversePrintListRec(Node head) { 
        if (head == null) { 
            return; 
        } else { 
            reversePrintListRec(head.next); 
            System.out.print(head.val + " "); 
        } 
    }
     
}
// 建立链表结构类 val和next
class Node{
     
     int val;
     Node next;
     public Node(int val){
          this.val=val;
          this.next=null;
     }
}

```
##### (3)运行结果
```java<br>
3 5 6 9 7 2 1
1 2 7 9 6 5 3
```
#### (4)查找单链表中的倒数第K个结点（k > 0）: reGetKthNode
```java<br>
package com.lianbiao.test;
/*
 * 查找单链表中的倒数第K个结点（k > 0）: reGetKthNode
 */
public class Main {
     public static void main(String[] args) {
          Node head = new Node(3);
          Node node1 = new Node(5);
          Node node2 = new Node(6);
          Node node3 = new Node(9);
          Node node4 = new Node(7);
          Node node5 = new Node(2);
          Node node6 = new Node(1);
          head.next = node1;
          node1.next=node2;
          node2.next=node3;
          node3.next=node4;
          node4.next=node5;
          node5.next=node6;
          
          printList(head);
          System.out.println(reGetKthNode(head, 3).val);
          System.out.println(reGetKthNodePointer(head, 2).val);
          
     }
     // 打印链表的方法，方便test函数
     public static void printList(Node head) { 
        while (head != null) { 
            System.out.print(head.val + " "); 
            head = head.next; 
        } 
        System.out.println(); 
    }
     
     /*
     * 查找单链表中的倒数第K个结点（k > 0）
     * 最普遍的方法是，先统计单链表中结点的个数，让head往后走n-k步
     * 注意链表为空，k为0，k为1，k大于链表中节点个数时的情况
     */
     public static Node reGetKthNode(Node head, int k){
          if (k==0||head==null)
              return null;
          //统计链表长度
          int sum = getListLength(head);  //得到当前链表的长度
          for(int i=0;i<sum;i++){    // i从0开始计数，让head往后边走sum-k步，则到达
              if(i!=(sum-k))
                   head=head.next;
              else
                   break;
          }
          //若ksum，则head==null;
          return head;
     }
     /*   方法2：
     * 主要思路就是使用两个指针，先让前面的指针走到正向第k个结点
     * 这样前后两个指针的距离差是k-1，之后前后两个指针一起向前走
     * 前面的指针走到最后一个结点时，后面指针所指结点就是倒数第k个结点
     */
     public static Node reGetKthNodePointer(Node head, int k){
          if(k==0||head==null)
              return null;
          Node first =head;  // 先移动的指针
          Node last =head;   // 后移动的指针
          // 让first领先last k个身位
          while (k > 1 && first != null) {
              first = first.next;
            k--;
        }
          // 当节点数小于k，返回null
        if (k > 1 || first == null) {
            return null;
        }
        // 前后两个指针一起走，直到前面的指针指向最后一个节点
        while(first.next!=null){
          first=first.next;
          last=last.next;
        }
       
        return last;
     }
     
     public static int getListLength(Node head) {
        // 注意头结点为空情况
        if (head == null) {
            return 0;
        }
 
        int len = 0;
        Node cur = head;
        while (cur != null) {
            len++;
            cur = cur.next;
        }
        return len;
    }
     
     
     
     
}
// 建立链表结构类 val和next
class Node{
     
     int val;
     Node next;
     public Node(int val){
          this.val=val;
          this.next=null;
     }
}

```
##### (4)运行结果
```java<br>
3 5 6 9 7 2 1
7
2
```
#### (5)查找单链表的中间结点
```java<br>
package com.lianbiao.test;
/*
 * 查找单链表的中间结点
 */
public class Main {
     public static void main(String[] args) {
          Node head = new Node(3);
          Node node1 = new Node(5);
          Node node2 = new Node(6);
          Node node3 = new Node(9);
          Node node4 = new Node(7);
          Node node5 = new Node(2);
          Node node6 = new Node(1);
          head.next = node1;
          node1.next=node2;
          node2.next=node3;
          node3.next=node4;
          node4.next=node5;
          node5.next=node6;
          
          printList(head);
          System.out.println(getMiddleNode(head).val);
     }
     // 打印链表的方法，方便test函数
     public static void printList(Node head) { 
        while (head != null) { 
            System.out.print(head.val + " "); 
            head = head.next; 
        } 
        System.out.println(); 
    }
     
    /**
     * 此题可应用于上一题类似的思想。也是设置两个指针，只不过这里是，两个指针同时向前走，前面的指针每次走两步，后面的指针每次走一步，
     * 前面的指针走到最后一个结点时，后面的指针所指结点就是中间结点，即第（n/2+1）个结点。注意链表为空，链表结点个数为1和2的情况。时间复杂度O（n
     */ 
    public static Node getMiddleNode(Node head) { 
        if (head == null || head.next == null) { 
            return head; 
        } 
 
        Node one =head;    // 每次走一步
        Node two =head;    // 每次走两步
     // two指针每次走两步，直到指向最后一个结点，one指针每次走一步 
        while (two.next!=null) {
          two=two.next;
          one=one.next;
          if(two.next!=null)
              two=two.next;
          }
        return one;
 
       
    } 
     
}
// 建立链表结构类 val和next
class Node{
     
     int val;
     Node next;
     public Node(int val){
          this.val=val;
          this.next=null;
     }
}
```
##### (5)运行结果
```java<br>
3 5 6 9 7 2 1
9
```
#### (6)合并有序单链表
```java<br>

package com.lianbiao.test;
public class Main {
 
	public static void main(String[] args) {
 
		Node head = new Node(3);
		Node node1 = new Node(5);
		Node node2 = new Node(6);
		Node node3 = new Node(9);
		
		Node head2 = new Node(1);
		Node node5 = new Node(2);
		Node node6 = new Node(3);
		head.next = node1;
		node1.next=node2;
		node2.next=node3;
		
		head2.next=node5;
		node5.next=node6;
		
		Node mergeList = mergeList(head, head2);
		
		printList(mergeList);
	}
	// 打印链表的方法，方便test函数
	public static void printList(Node head) {  
        while (head != null) {  
            System.out.print(head.val + " ");  
            head = head.next;  
        }  
        System.out.println();  
    }
	
	/** 
     * 已知两个单链表pHead1 和pHead2 各自有序，把它们合并成一个链表依然有序 
     * 这个类似归并排序。尤其注意两个链表都为空，和其中一个为空时的情况。只需要O（1）的空间。时间复杂度为O（max(len1, len2)） 
     */  
	public static Node mergeList(Node list1 , Node list2){
		if(list1==null)
			return list2;
		if(list2==null)
			return list1;
		
		Node resultNode;
		if(list1.val<list2.val){
			resultNode = list1;
			list1 = list1.next;
		}else{
			resultNode = list2;
			list2 = list2.next;
		}
		
		resultNode.next = mergeList(list1, list2);
		
		return resultNode;
	}
	
}
// 建立链表结构类 val和next
class Node{
	
	int val;
	Node next;
	public Node(int val){
		this.val=val;
		this.next=null;
	}
}

```
##### (6)运行结果
```java<br>
1 2 3 3 5 6 9
```
#### (7)判断两个链表是否相交
```java<br>
package com.lianbiao.test;

public class Main {
 
	public static void main(String[] args) {
 
		Node head = new Node(3);
		Node node1 = new Node(5);
		Node node2 = new Node(6);
		Node node3 = new Node(9);
		
		Node head2 = new Node(1);
		Node node5 = new Node(2);
		Node node6 = new Node(3);
		head.next = node1;
		node1.next=node2;
		node2.next=node3;
		
		head2.next=node5;
		node5.next=node6;
		node6.next=node2;
		printList(head2);
		printList(head);
		
		System.out.println(isIntersect(head, head2));
		
	}
	// 打印链表的方法，方便test函数
	public static void printList(Node head) {  
        while (head != null) {  
            System.out.print(head.val + " ");  
            head = head.next;  
        }  
        System.out.println();  
    }
	
	// 判断两个单链表是否相交  
    /** 
     * 如果两个链表相交于某一节点，那么在这个相交节点之后的所有节点都是两个链表所共有的。 也就是说，如果两个链表相交，那么最后一个节点肯定是共有的。 
     * 先遍历第一个链表，记住最后一个节点，然后遍历第二个链表， 到最后一个节点时和第一个链表的最后一个节点做比较，如果相同，则相交， 
     * 否则不相交。时间复杂度为O(len1+len2)，因为只需要一个额外指针保存最后一个节点地址， 空间复杂度为O(1) 
     */  
    public static boolean isIntersect(Node head1, Node head2) {  
        if (head1 == null || head2 == null) {  
            return false;  
        }  
  
        Node tail1 = head1;  
        // 找到链表1的最后一个节点  
        while (tail1.next != null) {  
            tail1 = tail1.next;  
        }  
  
        Node tail2 = head2;  
        // 找到链表2的最后一个节点  
        while (tail2.next != null) {  
            tail2 = tail2.next;  
        }  
  
        return tail1 == tail2;  
    }  
	
	
}
// 建立链表结构类 val和next
class Node{
	
	int val;
	Node next;
	public Node(int val){
		this.val=val;
		this.next=null;
	}
}
```
##### (7)运行结果
```java<br>
1 2 3 6 9
3 5 6 9
ture
```
#### (8)两个单链表相交的第一个节点
```java<br>
package com.lianbiao.test;
 
 
public class Main {
 
	public static void main(String[] args) {
 
		Node head = new Node(3);
		Node node1 = new Node(5);
		Node node2 = new Node(6);
		Node node3 = new Node(9);
		
		Node head2 = new Node(1);
		Node node5 = new Node(2);
		Node node6 = new Node(3);
		head.next = node1;
		node1.next=node2;
		node2.next=node3;
		
		head2.next=node5;
		node5.next=node6;
		node6.next=node2;
		
		printList(head2);
		printList(head);
		
		Node firstCommonNode = getFirstCommonNode(head, head2);
		System.out.println(firstCommonNode.val);
	}
	// 打印链表的方法，方便test函数
	public static void printList(Node head) {  
        while (head != null) {  
            System.out.print(head.val + " ");  
            head = head.next;  
        }  
        System.out.println();  
    }
	
	/** 
     * 求两个单链表相交的第一个节点 对第一个链表遍历，计算长度len1，同时保存最后一个节点的地址。 
     * 对第二个链表遍历，计算长度len2，同时检查最后一个节点是否和第一个链表的最后一个节点相同，若不相同，不相交，结束。 
     * 两个链表均从头节点开始，假设len1大于len2 
     * ，那么将第一个链表先遍历len1-len2个节点，此时两个链表当前节点到第一个相交节点的距离就相等了，然后一起向后遍历，直到两个节点的地址相同。 
     * 时间复杂度，O(len1+len2) 
     *  
     *              ----    len2 
     *                   |__________ 
     *                   | 
     *       ---------   len1 
     *       |---|<- len1-len2 
     */  
    public static Node getFirstCommonNode(Node head1, Node head2) {  
        if (head1 == null || head2 == null) {  
            return null;  
        }  
        int len1 = 1;  
        Node tail1 = head1;  
        while (tail1.next != null) {  
            tail1 = tail1.next;  
            len1++;  
        }  
  
        int len2 = 1;  
        Node tail2 = head2;  
        while (tail2.next != null) {  
            tail2 = tail2.next;  
            len2++;  
        }  
  
        // 不相交直接返回NULL  
        if (tail1 != tail2) {  
            return null;  
        }  
  
        Node n1 = head1;  
        Node n2 = head2;  
  
        // 略过较长链表多余的部分  
        if (len1 > len2) {  
            int k = len1 - len2;  
            while (k != 0) {  
                n1 = n1.next;  
                k--;  
            }  
        } else {  
            int k = len2 - len1;  
            while (k != 0) {  
                n2 = n2.next;  
                k--;  
            }  
        }
     // 一起向后遍历，直到找到交点  
        while (n1 != n2) {  
            n1 = n1.next;  
            n2 = n2.next;  
        }  
  
        return n1;  
    }  
	
	
}
// 建立链表结构类 val和next
class Node{
	
	int val;
	Node next;
	public Node(int val){
		this.val=val;
		this.next=null;
	}
}
```
##### (8)运行结果
```java<br>
1 2 3 6 9
3 5 6 9
6
```
#### (9)判断链表中是否有环
```java<br>

package com.lianbiao.test;
 
public class Main {
 
	public static void main(String[] args) {
 
		Node head = new Node(3);
		Node node1 = new Node(5);
		Node node2 = new Node(6);
		Node node3 = new Node(9);
		Node node4 = new Node(7);
		Node node5 = new Node(2);
		Node node6 = new Node(1);
		head.next = node1;
		node1.next=node2;
		node2.next=node3;
		node3.next=node4;
		node4.next=node5;
		node5.next=node6;
		node6.next=node4;
 
		
		System.out.println(hasCycle(head));
	}
	// 打印链表的方法，方便test函数
	public static void printList(Node head) {  
        while (head != null) {  
            System.out.print(head.val + " ");  
            head = head.next;  
        }  
        System.out.println();  
    }
	
	/** 
     * 判断一个单链表中是否有环 
     * 这里也是用到两个指针。如果一个链表中有环，也就是说用一个指针去遍历，是永远走不到头的。因此，我们可以用两个指针去遍历，一个指针一次走两步 
     * ，一个指针一次走一步，如果有环，两个指针肯定会在环中相遇。时间复杂度为O（n） 
     */  
    public static boolean hasCycle(Node head) {  
        Node fast = head; // 快指针每次前进两步  
        Node slow = head; // 慢指针每次前进一步  
  
        while (fast != null && fast.next != null) {  
            fast = fast.next.next;  
            slow = slow.next;  
            if (fast == slow) { // 相遇，存在环  
                return true;  
            }  
        }  
        return false;  
    }  
	
}
// 建立链表结构类 val和next
class Node{
	
	int val;
	Node next;
	public Node(int val){
		this.val=val;
		this.next=null;
	}
}
```
##### (9)运行结果
```java<br>
ture
```
#### (10)链表中环中的第一个节点
```java<br>
package com.lianbiao.test;
 
import java.util.HashMap;
 
 
public class Main {
 
	public static void main(String[] args) {
 
		Node head = new Node(3);
		Node node1 = new Node(5);
		Node node2 = new Node(6);
		Node node3 = new Node(9);
		Node node4 = new Node(7);
		Node node5 = new Node(2);
		Node node6 = new Node(1);
		head.next = node1;
		node1.next=node2;
		node2.next=node3;
		node3.next=node4;
		node4.next=node5;
		node5.next=node6;
		node6.next=node4;
 
		System.out.println(getFirstNodeInCycleHashMap(head).val);
		
	}
	// 打印链表的方法，方便test函数
	public static void printList(Node head) {  
        while (head != null) {  
            System.out.print(head.val + " ");  
            head = head.next;  
        }  
        System.out.println();  
    }
	
	/** 
     * 求进入环中的第一个节点 用快慢指针做（本题用了Crack the Coding Interview的解法，因为更简洁易懂！） 
     */  
    public static Node getFirstNodeInCycle(Node head) {  
        Node slow = head;  
        Node fast = head;  
  
        // 1） 找到快慢指针相遇点  
        while (fast != null && fast.next != null) {  
            slow = slow.next;  
            fast = fast.next.next;  
            if (slow == fast) { // Collision  
                break;  
            }  
        }  
  
        // 错误检查，这是没有环的情况  
        if (fast == null || fast.next == null) {  
            return null;  
        }  
  
        // 2）现在，相遇点离环的开始处的距离等于链表头到环开始处的距离，  
        // 这样，我们把慢指针放在链表头，快指针保持在相遇点，然后  
        // 同速度前进，再次相遇点就是环的开始处！  
        slow = head;  
        while (slow != fast) {  
            slow = slow.next;  
            fast = fast.next;  
        }  
  
        // 再次相遇点就是环的开始处  
        return fast;  
    } 
    
    /** 
     * 求进入环中的第一个节点 用HashMap做 一个无环的链表，它每个结点的地址都是不一样的。 
     * 但如果有环，指针沿着链表移动，那这个指针最终会指向一个已经出现过的地址 以地址为哈希表的键值，每出现一个地址，就将该键值对应的实值置为true。 
     * 那么当某个键值对应的实值已经为true时，说明这个地址之前已经出现过了， 直接返回它就OK了 
     */  
    public static Node getFirstNodeInCycleHashMap(Node head) {  
        HashMap<Node, Boolean> map = new HashMap<Node, Boolean>();  
        while (head != null) {
        	
            if (map.containsKey(head)) {  
                return head; // 这个地址之前已经出现过了，就是环的开始处  
            } else {  
                map.put(head, true);  
                head = head.next;  
            }  
            
        }  
        return head;  
    }  
	
}
// 建立链表结构类 val和next
class Node{
	
	int val;
	Node next;
	public Node(int val){
		this.val=val;
		this.next=null;
	}
}
```
##### (10)运行结果
```java<br>
7
```
#### (11)给出一单链表头指针pHead和一节点指针pToBeDeleted，O(1)时间复杂度删除节点pToBeDeleted: delete
```java<br>
package com.lianbiao.test;
 
 
public class Main {
 
	public static void main(String[] args) {
 
		Node head = new Node(3);
		Node node1 = new Node(5);
		Node node2 = new Node(6);
		Node node3 = new Node(9);
		Node node4 = new Node(7);
		Node node5 = new Node(2);
		Node node6 = new Node(1);
		head.next = node1;
		node1.next=node2;
		node2.next=node3;
		node3.next=node4;
		node4.next=node5;
		node5.next=node6;
			
		printList(head);
		delete(head, node3);
		printList(head);
	}
	// 打印链表的方法，方便test函数
	public static void printList(Node head) {  
        while (head != null) {  
            System.out.print(head.val + " ");  
            head = head.next;  
        }  
        System.out.println();  
    }
	
	/** 
     * 给出一单链表头指针head和一节点指针toBeDeleted，O(1)时间复杂度删除节点tBeDeleted 
     * 对于删除节点，我们普通的思路就是让该节点的前一个节点指向该节点的下一个节点 
     * ，这种情况需要遍历找到该节点的前一个节点，时间复杂度为O(n)。对于链表， 
     * 链表中的每个节点结构都是一样的，所以我们可以把该节点的下一个节点的数据复制到该节点 
     * ，然后删除下一个节点即可。要注意最后一个节点的情况，这个时候只能用常见的方法来操作，先找到前一个节点，但总体的平均时间复杂度还是O(1) 
     */  
    public static void delete(Node head, Node toDelete){  
        if(toDelete == null){  
            return;  
        }  
        if(toDelete.next != null){          // 要删除的是一个中间节点  
            toDelete.val = toDelete.next.val;       // 将下一个节点的数据复制到本节点!  
            toDelete.next = toDelete.next.next;  
        }  
        else{       // 要删除的是最后一个节点！  
            if(head == toDelete){       // 链表中只有一个节点的情况    
                head = null;  
            }else{  
                Node node = head;  
                while(node.next != toDelete){   // 找到倒数第二个节点  
                    node = node.next;  
                }  
                node.next = null;  
            }  
        }  
    }  
	
}
// 建立链表结构类 val和next
class Node{
	
	int val;
	Node next;
	public Node(int val){
		this.val=val;
		this.next=null;
	}
}
```
##### (11)运行结果
```java<br>
3 5 6 9 7 2 1 
3 5 6 7 2 1 
```
## 多种单链表反转面试题总结
```java<br>
总结下面试题中常见的单链表反转：
1.常规的反转单链表
2.以K个为一组反转单链表，最后不足K个节点的部分也反转
3.以K个为一组反转单链表，最后不足K个节点的部分不反转
```
### 1.反转单链表
```java<br>
代码如下：

/*
     *  翻转链表（遍历） 
     *  从头到尾遍历原链表，每遍历一个结点，
     *  将其摘下放在新链表的最前端。
     *  注意链表为空和只有一个结点的情况。时间复杂度为O（n）
     */
    public static ListNode reverseNode(ListNode head){
         // 如果链表为空或只有一个节点，无需反转，直接返回原链表表头
         if(head == null || head.next == null)
             return head;
         
         ListNode reHead = null;
         ListNode cur = head;
         while(cur!=null){
             ListNode reCur = cur;      // 用reCur保存住对要处理节点的引用
             cur = cur.next;        // cur更新到下一个节点
             reCur.next = reHead;   // 更新要处理节点的next引用
             reHead = reCur;        // reHead指向要处理节点的前一个节点
         }
         return reHead;
    }
```
### 2.以K个为一组反转单链表，最后不足K个节点的部分也反转
#### 代码
```java<br>
/**
	 * 分组反转单链表，最后不足K个节点的部分也反转
	 * @param head
	 * @param k
	 * @return
	 */
	public static ListNode reverseKgroup(ListNode head, int k) {
		if (head == null)
			return head;
		ListNode cur = head;
		ListNode reHead = null;
 
		int count = 0;
		/* Reverse first k nodes of linked list */
		while (count < k && cur != null) {
			ListNode reCur = cur;
			cur = cur.next;
			reCur.next = reHead;
			reHead = reCur;
			count++;
		}
		/*
		 * cur is now a pointer to (k+1)th node Recursively call for the
		 * list starting from current. And make rest of the list as next of
		 * first node
		 */
		if (cur != null)
			head.next = reverseKgroup(cur, k);
		
		return reHead;
	}
	
```
#### 举例解释
```java<br>
举例解释：

输入的原始单链表为3-5-6-9-7-2-1-12，其中K为3；

经过第一次while循环，单链表变为6-5-3-9-7-2-1-12。此时跳出while循环是因为count<k不成立了，cur节点指向了9，head节点指向了3。所以接着判断cur是否为null，若不是，则刚好递归求出head.next。

经过第二次while循环，单链表为6-5-3-2-7-9-1-12。此时跳出while循环是因为count<k不成立了，cur节点指向了1，head节点指向了9。接着判断cur，并且递归求head.next节点。

第三次循环，跳出while是因为cur==null了，直接返回reHead，此时reHead指向了12。

可以看出，K个为一组反转单链表，核心代码还是常规的如何反转单链表。

```
`插入图片1`
### 3.以K个为一组反转单链表，最后不足K个节点的部分不反转
#### 例题
`插入图片2`
#### 解题思路
```java<br>
思路：核心代码还是反转常规的单链表，不过此题需要加上节点数量的判断，当节点数目不足K个时，不进行反转操作，直接返回。
```
#### 代码
```java<br>

/**
	 * 分组反转单链表，最后不足K个节点的部分不反转
	 * @param head
	 * @param k
	 * @return
	 */
	public static ListNode reverseKgroups(ListNode head, int k) {
		if (head == null)
			return head;
		ListNode cur = head;
		ListNode reHead = null;
 
		int count = 0;
		if (getSize(cur) >= k) {
			/* Reverse first k nodes of linked list */
			while (count < k && cur != null) {
				ListNode reCur = cur;
				cur = cur.next;
				reCur.next = reHead;
				reHead = reCur;
				count++;
			}
			/*
			 * cur is now a pointer to (k+1)th node Recursively call for the
			 * list starting from current. And make rest of the list as next of
			 * first node
			 */
			if (cur != null)
				head.next = reverseKgroups(cur, k);
			return reHead;
		}
		return cur;
	}
统计节点数目函数如下：

/**
	 * 统计该节点之后的节点数量
	 * @param head
	 * @return
	 */
	public static int getSize(ListNode head) {
		int count = 0;
		ListNode curNode = head;
		while (curNode != null) {
			count++;
			curNode = curNode.next;
		}
		return count;
	}

```
#### 结果
```java<br>
AC结果如下：
```
`插入图片3`

### 4.总结
```java<br>
总结：
       各个形式的反转单链表，最重要的理清楚head、reHead和cur三个节点的关系，通过核心代码和方法的递归来实现分组反转。
附上完整代码：

public class Main {
	public static void main(String[] args) {
		ListNode head = new ListNode(3);
		ListNode node1 = new ListNode(5);
		ListNode node2 = new ListNode(6);
		ListNode node3 = new ListNode(9);
		ListNode node4 = new ListNode(7);
		ListNode node5 = new ListNode(2);
		ListNode node6 = new ListNode(1);
		ListNode node7 = new ListNode(12);
		head.next = node1;
		node1.next = node2;
		node2.next = node3;
		node3.next = node4;
		node4.next = node5;
		node5.next = node6;
		node6.next = node7;
		printList(head);
//		printList(reverseNode(head));
//		printList(reverseKgroups(head, 3));
		printList(reverseKgroup(head, 3));
 
	}
 
	// 打印链表的方法，方便test函数
	public static void printList(ListNode head) {
		while (head != null) {
			System.out.print(head.val + " ");
			head = head.next;
		}
		System.out.println();
	}
	/**
	 * 分组反转单链表，最后不足K个节点的部分不反转
	 * @param head
	 * @param k
	 * @return
	 */
	public static ListNode reverseKgroups(ListNode head, int k) {
		if (head == null)
			return head;
		ListNode cur = head;
		ListNode reHead = null;
 
		int count = 0;
		if (getSize(cur) >= k) {
			/* Reverse first k nodes of linked list */
			while (count < k && cur != null) {
				ListNode reCur = cur;
				cur = cur.next;
				reCur.next = reHead;
				reHead = reCur;
				count++;
			}
			/*
			 * cur is now a pointer to (k+1)th node Recursively call for the
			 * list starting from current. And make rest of the list as next of
			 * first node
			 */
			if (cur != null)
				head.next = reverseKgroups(cur, k);
			return reHead;
		}
		return cur;
	}
 
	/**
	 * 分组反转单链表，最后不足K个节点的部分也反转
	 * @param head
	 * @param k
	 * @return
	 */
	public static ListNode reverseKgroup(ListNode head, int k) {
		if (head == null)
			return head;
		ListNode cur = head;
		ListNode reHead = null;
 
		int count = 0;
		/* Reverse first k nodes of linked list */
		while (count < k && cur != null) {
			ListNode reCur = cur;
			cur = cur.next;
			reCur.next = reHead;
			reHead = reCur;
			count++;
		}
		/*
		 * cur is now a pointer to (k+1)th node Recursively call for the
		 * list starting from current. And make rest of the list as next of
		 * first node
		 */
		if (cur != null)
			head.next = reverseKgroup(cur, k);
		
		return reHead;
	}
	/**
	 * 统计该节点之后的节点数量
	 * @param head
	 * @return
	 */
	public static int getSize(ListNode head) {
		int count = 0;
		ListNode curNode = head;
		while (curNode != null) {
			count++;
			curNode = curNode.next;
		}
		return count;
	}
 
 
	/*
	 * 翻转链表（遍历） 从头到尾遍历原链表，每遍历一个结点， 将其摘下放在新链表的最前端。 注意链表为空和只有一个结点的情况。时间复杂度为O（n）
	 */
	public static ListNode reverseNode(ListNode head) {
		// 如果链表为空或只有一个节点，无需反转，直接返回原链表表头
		if (head == null || head.next == null)
			return head;
 
		ListNode reHead = null;
		ListNode cur = head;
		while (cur != null) {
			ListNode reCur = cur; // 用reCur保存住对要处理节点的引用
			cur = cur.next; // cur更新到下一个节点
			reCur.next = reHead; // 更新要处理节点的next引用
			reHead = reCur; // reHead指向要处理节点的前一个节点
		}
		return reHead;
	}
 
}
class ListNode {
      int val;
      ListNode next;
      ListNode(int x) { val = x; }
}
```
