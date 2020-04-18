# 小徐
# 一.Java基础算法
## 排序算法
### 1.快速排序
```java<br>
public void quickSort (int a[], int low, int high){
        
        int i=low;    
        int j=high;    
        int key=a[low];    
    
        if (low < high){    
        while(i<j){   //此处的while循环结束，则完成了元素key的位置调整    
            while(i<j && key<=a[j]){    
                j--;    
            }    
            a[i]=a[j];    
            while(i<j && key>=a[i]){    
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
### 2.归并排序
```java<br>
        public void mergeSort(int[] num,int left,int right){
		if(num==null)
			return ;
		int mid = (left+right)/2;
		if(left<right){
			// 处理左边
			mergeSort(num, left, mid);
			// 处理右边
			mergeSort(num, mid+1, right);
			// 左右归并
			merge(num, left, mid, right);
		}
	}
	private void merge(int[] num, int left, int mid, int right) {
		// 定义一个辅助数组，所以该算法的空间复杂度为O(N)
		int[] temp = new int[right-left+1];
		int i = left;
		int j = mid+1;
		int k = 0;
		// 找出较小值元素放入temp数组中
		while(i<=mid&&j<=right){
			if(num[i]<num[j])
				temp[k++] = num[i++];
			else
				temp[k++] = num[j++];
		}
		// 处理较长部分
		while(i<=mid){
			temp[k++] = num[i++];
		}
		while(j<=right){
			temp[k++] = num[j++];
		}
		// 使用temp中的元素覆盖num中的元素
		for(int k2 = 0;k2<temp.length;k2++){
			num[k2+left] = temp[k2];
		}
	}
```







## 单链表相关算法
### 1.单链表反转
```java<br>
class Node{  
    int val;  
    Node next;  
    public Node(int val){  
         this.val=val;  
    }
} 

/*
从头到尾遍历原链表，每遍历一个结点
将其摘下放在新链表的最前端。
注意链表为空和只有一个结点的情况。
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
```
### 2.合并有序的单链表
```java<br>
// 给出两个分别有序的单链表，将其合并成一条新的有序单链表。
举例：1→3→5和2→4→6合并之后为1→2→3→4→5→6 步骤：首先，我们通过比较确定新链表的头节点，然后移动链表1或者链表2的头指针。然后通过递归来得到新的链表头结点的next
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
## 二叉树相关算法
### 1.分层遍历二叉树
```java<br>
// 定义二叉树结构
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    public TreeNode(int val) {
        this.val = val;
    }
}

// 分层遍历（宽度优先遍历12345678...）
public static void levelTraversal(TreeNode root){  
    if(root==null)  
        return ;  
    LinkedList<TreeNode> queue = new LinkedList<TreeNode>();  
    queue.add(root);  // 队列初始化，将根节点加入队列
    while(!queue.isEmpty()){  
        TreeNode cur = queue.remove();  
        System.out.print(cur.val+" ");  
        if(cur.left!=null)  
            queue.add(cur.left);  
        if(cur.right!=null)  
            queue.add(cur.right);  
    }  
}
// 前序遍历（深度优先遍历）
// 使用一个辅助堆栈，初始时将根节点加入堆栈，当堆栈不为空时，弹出一个节点，分别将其不为空的右子节点和左子节点加入堆栈。 
public static void preorderTraversal(TreeNode root){  
    if(root==null)  
        return ;  
    Stack<TreeNode> stack = new Stack<TreeNode>(); // 辅助stack  
    stack.push(root);  
    while(!stack.isEmpty()){  
        TreeNode cur = stack.pop();  // 出栈栈顶元素   
        System.out.print(cur.val+" ");  
        // 关键点：要先压入右孩子，再压入左孩子，这样在出栈时会先打印左孩子再打印右孩子  
        if(cur.right!=null)  
             stack.push(cur.right);  
        if(cur.left!=null)  
             stack.push(cur.left);  
    }  
}
```
## 队列和堆栈相关算法
### 1.包含min函数的栈
```java<br>
// 定义栈的数据结构，请在该类型中实现一个能够得到栈最小元素的min函数。在该栈中，调用min、push和pop方法。要求的时间复杂度均为O(1)
// 思路：题目要求我们的各个方法均为O(1)复杂度，则我们考虑增加辅助空间来实现，即增加一个专门用来存储min值的辅助栈。
// 比如，data中依次入栈，5, 4, 3, 8, 10, 11, 12, 1。则辅助栈依次入栈， 5, 4, 3，no,no, no, no, 1。no代表此次不如栈。
// 即，每次入栈的时候，如果入栈的元素比min中的栈顶元素小或等于则入栈，否则不入栈。
import java.util.Stack;   
public class Main {  
    Stack<Integer> stack = new Stack<>();  
    Stack<Integer> minStack = new Stack<>();  
  
    public void push(int node) {  
        stack.push(node);  
        if(minStack.isEmpty()||minStack.peek()>=node)  
            minStack.push(node);  
    }   
/** 
 * 首先需要对stack执行出栈操作, 
 * 判断minStack中是否需要出栈操作 
 */  
    public void pop() {  
        stack.pop();  
        if(stack.peek()==minStack.peek()){  
            minStack.pop();  
        }  
    }  
 
    public int top() {  
        return stack.peek();  
    }  
 
/** 
 * 直接peek minStack 
 * @return 
 */  
    public int min() {  
        return minStack.peek();  
    }  
}
```
## 字符串相关算法
### 1.字符串转整数
```java<br>
// 自定义一个函数，实现数字字符串转换成整数，如String str = “125748”转换成125748
/*
特殊情况：
能够排除首部的空格，从第一个非空字符开始计算
允许数字以正负号(+-)开头
遇到非法字符便停止转换，返回当前已经转换的值，如果开头就是非 法字符则返回0
在转换结果溢出时返回特定值，这里是最大/最小整数
*/
public static int myAtoi(String str) {  
    if(str==null||str.length()==0)  
        return 0;  
    char[] array = str.toCharArray();  
    long result = 0;  // 要返回的结果result  
    int count = 0;  // 记录‘+’或者‘-’出现的次数  
    int num = 0;   // 判断空格出现的位置  
    int flag = 1; // 正数还是负数  
    for (int i = 0; i < array.length; i++) {  
        Character c = array[i];  
        if(c>='0'&&c<='9'){  
            result = result*10+c-'0';  
            // 判断是否溢出  
            if(flag==1&&result>Integer.MAX_VALUE){  
                return Integer.MAX_VALUE;  
            }else if(flag==-1&&-result<Integer.MIN_VALUE)  
                return Integer.MIN_VALUE;  
            num++;  
        }else if(c==' '&&num==0&&count==0)  
            continue;  
        else if(c=='+'&&count==0){  
            count = 1;  
        }  
        else if(c=='-'&&count==0){  
            flag = -1;  
            count = 1;  
        }  
        else{  
            return (int) (flag*result);  
 
        }  
    }  
    return (int) (flag*result);  
}  
```
## 数组相关算法
### 1.查找多数元素
数组也是一种极其常见的数据结构，在面试中和数组相关的算法题出现频率贼高。<br>
对数组的操作，一般会要求时间复杂度和空间复杂度。<br>
所以，最常用的方法就是设置两个指针，分别指向不同的位置，不断调整指针指向来实现O（N）时间复杂度内实现算法。<br>
常见的面试题有：拼接一个最大/小的数字、合并两个有序数组、调整数组顺序使奇数位于偶数前面、查找多数元素、数组中的重复元素<br>
```java<br>
// 找出一个数组中占50%以上的元素，即寻找多数元素，并且多数元素是一定存在的假设
// 思路1：将数组排序，则中间的那个元素一定是多数元素
public int majorityElement(int[] nums) {  
    Arrays.sort(nums);  
    return nums[nums.length/2];  
}  

// 该代码的时间复杂度为O（NlogN），面试官会问你能不能进行优化时间复杂度？
// 思路2：利用HashMap来记录每个元素的出现次数
public int majorityElement(int[] nums) {  
    HashMap<Integer, Integer> map = new HashMap<>();  
 
    for (int i = 0; i < nums.length; i++) {  
        if(!map.containsKey(nums[i])){  
            map.put(nums[i], 1);  
        }else {  
            int values = map.get(nums[i]);  
            map.put(nums[i], ++values);  
        }  
    }  
    int n = nums.length/2;  
    Set<Integer> keySet = map.keySet();  
    Iterator<Integer> iterator = keySet.iterator();  
    while(iterator.hasNext()){  
        int key = iterator.next();  
        int value = map.get(key);  
        if (value>n) {  
            return key;  
        }  
    }  
    return 0;   
}

// 该代码的时间复杂度为O（N），空间复杂度为O（N）。面试官还不满意，问你能不能用O（N）+O（1）实现该算法？
// 思路3：Moore voting algorithm--每找出两个不同的element，就成对删除即count--，最终剩下的一定就是所求的。
public int majorityElement(int[] nums) {  
      int elem = 0;  
      int count = 0;   
      for(int i = 0; i < nums.length; i++)  {      
         if(count == 0)  {  
             elem = nums[i];  
             count = 1;  
         }  
         else    {  
             if(elem == nums[i])  
                 count++;  
             else  
                 count--;  
         }  
 
     }  
     return elem;       
}  
```

### 2.把数组排成最小的数
```java<br>
// 输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。
// 例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。
// 思路：我们需要定义一种新的比较大小规则，数组根据这个规则可以排成一个最小的数字。
// 排序规则：两个数字m和n，我们比较mn和nm的大小，来确定在新的比较规则下n和m的大小关系，来确定哪个应该排在前面
// 步骤：将整型数组转换为String数组。在新的规则下对数组进行排序（本例使用了选择排序）
public String PrintMinNumber(int [] num) {  
    if(num==null||num.length==0)  
        return "";  
    int len = num.length;  
    String[] str = new String[len];  
    for(int i = 0; i < len; i++){  
        str[i] = String.valueOf(num[i]);  
    }  
    for (int i = 0; i < str.length; i++) {  
        for (int j = i+1; j < str.length; j++) {  
            if(compare(str[i], str[j])){  
                String temp = str[j];  
                str[j] = str[i];  
                str[i] = temp;  
            }  
        }  
    }  
    StringBuilder sb = new StringBuilder();  
    for(int i = 0;i<str.length;i++){  
        sb = sb.append(str[i]);  
    }  
    return sb.toString();  
 
}  
private boolean compare(String s1,String s2){  
    int len = s1.length()+s2.length();  
    String str1 = s1+s2;  
    String str2 = s2+s1;  
    for (int i = 0; i < len; i++) {  
        if(Integer.parseInt(str1.substring(i,i+1))>Integer.parseInt(str2.substring(i,i+1)))  
            return true;  
        if(Integer.parseInt(str1.substring(i,i+1))<Integer.parseInt(str2.substring(i,i+1)))  
            return false;  
    }  
    return false;    
}
```












## 查找算法
### 1.二分查找
```java<br>
// 递归
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

// 非递归
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

























