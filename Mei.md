
# 二叉树
## 1.前序遍历
```java<br>
/**
     * 前序遍历，中序遍历，后序遍历
     *  前序遍历递归解法： 
     * （1）如果二叉树为空，空操作 
     * （2）如果二叉树不为空，访问根节点，前序遍历左子树，前序遍历右子树
     */
     public static void preorderTraversalRec(TreeNode root){
          if(root==null)
              return ;
          System.out.print(root.val+" ");
          preorderTraversalRec(root.left);
          preorderTraversalRec(root.right);
     }
      /**
     *  前序遍历迭代解法：用一个辅助stack，总是把右孩子放进栈
     */
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
## 2.中序遍历
```java<br>
/**
     * 中序遍历递归解法 
     * （1）如果二叉树为空，空操作。 
     * （2）如果二叉树不为空，中序遍历左子树，访问根节点，中序遍历右子树
     */
     public static void inorderTraversalRec(TreeNode root){
          if(root==null)
              return;
          inorderTraversalRec(root.left);
          System.out.print(root.val+" ");
          inorderTraversalRec(root.right);
          
     }
     /**
     * 中序遍历迭代解法 ，用栈先把根节点的所有左孩子都添加到栈内，
     * 然后输出栈顶元素，再处理栈顶元素的右子树
     */
     public static void inorderTraversal(TreeNode root){
          if(root==null)
              return;
          Stack<TreeNode> stack =new Stack<TreeNode>();
          TreeNode cur = root;
          while(true){
              while(cur!=null){      // 先添加一个非空节点所有的左孩子到栈
                   stack.push(cur);
                   cur = cur.left;
              }
              if(stack.isEmpty())
                   break;
              
              cur = stack.pop();    // 因为此时已经没有左孩子了，所以输出栈顶元素
              System.out.print(cur.val+" ");
              cur = cur.right;       // 准备处理右子树
          }
  }
```
## 3.后序遍历
```java<br>
/**
     * 后序遍历递归解法 
     * （1）如果二叉树为空，空操作 
     * （2）如果二叉树不为空，后序遍历左子树，后序遍历右子树，访问根节点
     */ 
    public static void postorderTraversalRec(TreeNode root) { 
        if (root == null)  
            return; 
        postorderTraversalRec(root.left); 
        postorderTraversalRec(root.right); 
        System.out.print(root.val + " "); 
    }
    /**
     *  后序遍历迭代解法
     */
    public static void postorderTraversal(TreeNode root){
     if (root == null)  
            return;
     Stack<TreeNode> stack =new Stack<TreeNode>();  // 第一个stack用于添加node和它的左右孩子
     Stack<TreeNode> output =new Stack<TreeNode>(); // 第二个stack用于翻转第一个stack输出 
     
     TreeNode cur = root;
     stack.push(cur);
     while(!stack.isEmpty()){  // 确保所有元素都被翻转转移到第二个stack
          cur = stack.pop();    // 把栈顶元素添加到第二个stack
          output.push(cur);
          
          if(cur.left!=null)    // 把栈顶元素的左孩子和右孩子分别添加入第一个stack
              stack.push(cur.left);
          if(cur.right!=null)
              stack.push(cur.right);
     }
     
     while(!output.isEmpty()){    // 遍历输出第二个stack，即为后序遍历 
          cur = output.pop();
          System.out.print(cur.val+" ");
     }
  }
```
### 测试类
```java<br>
package com.ywq.package1;
 
public class Main {
 
	/* 
		    1  
		  /  \  
		 2    3  
		/ \  / \  
	   4  5 7   6  
	*/  
	public static void main(String[] args){
		
		TreeNode root = new TreeNode(1);  
        TreeNode r2 = new TreeNode(2);  
        TreeNode r3 = new TreeNode(3);  
        TreeNode r4 = new TreeNode(4);  
        TreeNode r5 = new TreeNode(5);  
        TreeNode r6 = new TreeNode(6);
        TreeNode r7 = new TreeNode(7);
          
        root.left = r2;  
        root.right = r3;  
        r2.left = r4;  
        r2.right = r5;  
        r3.right = r6; 
        r3.left = r7;
        
        
        
        preorderTraversalRec(root);
        System.out.println();
        preorderTraversal(root);
        
        inorderTraversalRec(root);
        System.out.println();
        inorderTraversal(root);
        
        postorderTraversalRec(root);
        System.out.println();
        postorderTraversal(root);
        
	}
 
}
// 建立二叉树节点类
class TreeNode{
	int val;
	TreeNode left;
	TreeNode right;
	public TreeNode(int val){
		this.val = val;
	}
}
```
## 4.求二叉树中节点个数
```java<br>

/***********************************************************************
	 4. 求二叉树中的节点个数: getNodeNumRec（递归），getNodeNum（迭代）
**********************************************************************/
    /** 
     * 求二叉树中的节点个数递归解法： O(n) 
     * （1）如果二叉树为空，节点个数为0  
     * （2）如果二叉树不为空，二叉树节点个数 = 左子树节点个数 + 
     *            右子树节点个数 + 1 
     */ 
	public static int getNodeNumRec(TreeNode root){
		if(root==null)
			return 0;
		else 
			return getNodeNumRec(root.left)+getNodeNumRec(root.right)+1;
	}
	/** 
     *  求二叉树中的节点个数迭代解法O(n)：基本思想同LevelOrderTraversal， 
     *  即用一个Queue，在Java里面可以用LinkedList来模拟  
     */ 
	public static int getNodeNum(TreeNode root){
		if(root==null)
			return 0;
		
		Queue<TreeNode> queue = new LinkedList<TreeNode>();
		queue.add(root);
		int count=1;
		while(!queue.isEmpty()){
			TreeNode cur = queue.remove();
			if(cur.left!=null){
				queue.add(cur.left);
				count++;
			}
			if(cur.right!=null){
				queue.add(cur.right);
				count++;
			}
		}
		return count;
	}
```
## 5.求二叉树的深度
```java<br>

/***********************************************************************
	 5. 求二叉树的深度: getDepthRec（递归），getDepth
**********************************************************************/
	/** 
     * 求二叉树的深度（高度） 递归解法： O(n) 
     * （1）如果二叉树为空，二叉树的深度为0  
     * （2）如果二叉树不为空，二叉树的深度 = max(左子树深度， 右子树深度) + 1 
     */ 
	public static int getDepthRec(TreeNode root){
		if(root==null)
			return 0;
		
		int leftNode = getDepthRec(root.left);
		int rightNode = getDepthRec(root.right);
		return (leftNode>rightNode? leftNode:rightNode)+1;
		
			
	}
	public static int getDepth(TreeNode root){
		if(root==null)
			return 0;
		
		int depth=0;
		int curLevelNodeNums=1;
		int nextLevelNodeNums=0;
		LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
		queue.add(root);
		while(!queue.isEmpty()){
			TreeNode cur = queue.remove();
			curLevelNodeNums--;
			if(cur.left!=null){
				queue.add(cur.left);
				nextLevelNodeNums++;
			}
			
			if(cur.right!=null){
				queue.add(cur.right);
				nextLevelNodeNums++;
			}
			
			if(curLevelNodeNums==0){
				depth++;
				curLevelNodeNums=nextLevelNodeNums;
				nextLevelNodeNums=0;
			}
			
		}
		return depth;
	}
```
## 6.分层遍历二叉树
```java<br>
/***********************************************************************
	 6.分层遍历二叉树（按层次从上往下，从左往右）: levelTraversal 
**********************************************************************/
    /** 
     * 分层遍历二叉树（按层次从上往下，从左往右）迭代 
     * 相当于广度优先搜索，使用队列实现。队列初始化，将根节点压入队列。当队列不为空，进行如下操作：弹出一个节点 
     * ，访问，若左子节点或右子节点不为空，将其压入队列 
     */ 
    public static void levelTraversal(TreeNode root){
    	if(root==null)
    		return ;
    	
    	LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
    	queue.add(root);
    	while(!queue.isEmpty()){
    		TreeNode cur = queue.remove();
    		System.out.print(cur.val+" ");
    		if(cur.left!=null)
    			queue.add(cur.left);
    		if(cur.right!=null)
    			queue.add(cur.right);
    		
    	}
   }
```
## 7.求二叉树中第K层的节点个数
```java<br>
/***********************************************************************
	 7. 求二叉树第K层的节点个数:getNodeNumKthLevelRec(递归) getNodeNumKthLevel(迭代)
**********************************************************************/
    /** 
     * 求二叉树第K层的节点个数   递归解法：  
     * （1）如果二叉树为空或者k<1返回0 
     * （2）如果二叉树不为空并且k==1，返回1 
     * （3）如果二叉树不为空且k>1，返回root左子树中k-1层的节点个数与root右子树k-1层节点个数之和 
     *  
     * 求以root为根的k层节点数目 等价于 求以root左孩子为根的k-1层（因为少了root那一层）节点数目 加上 
     * 以root右孩子为根的k-1层（因为少了root那一层）节点数目 
     *  
     * 所以遇到树，先把它拆成左子树和右子树，把问题降解 
     *  
     */  
    public static int getNodeNumKthLevelRec(TreeNode root, int k){
    	if(root==null || k<1)
    		return 0;
    	if(k==1)
    		return 1;
    	
    	int leftNum = getNodeNumKthLevelRec(root.left, k-1);
    	int rightNum = getNodeNumKthLevelRec(root.right, k-1);
    	return leftNum+rightNum;
    	
    }
    /** 
     *  求二叉树第K层的节点个数   迭代解法：  
     *  同getDepth的迭代解法 
     */
    public static int getNodeNumKthLevel(TreeNode root, int k){
    	if(root==null)
    		return 0;
    	LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
    	queue.add(root);   // 先将root存入队列
    	
    	int i = 1; // 用来记录当前层数,和k做比较
    	int currentLevelNode = 1;
    	int nextLevelNode = 0;
    	// 当队列不为空，且当前层数小于k时，执行以下循环
    	while(!queue.isEmpty() && i<k){ // 当输入k大于深度时，返回0;
    		TreeNode cur = queue.remove();
    		currentLevelNode--;
    		
    		if(cur.left!=null){  // 处理左子树
    			queue.add(cur.left);
    			nextLevelNode++;
    		}
    		
    		if(cur.right!=null){  // 处理右子树
    			queue.add(cur.right);
    			nextLevelNode++;
    		}
    		
    		if(currentLevelNode==0){  // 当前层的节点全部遍历完，则开始处理下一层。
    			currentLevelNode = nextLevelNode;
    			nextLevelNode = 0;
    			i++;
    		}
    	}
    	// 当循环结束时，说明到达了K层
    	return currentLevelNode;
   }
```
## 8.求二叉树中叶子节点的个数
```java<br>
/***********************************************************************
	 8. 求二叉树中叶子节点的个数: getNodeNumLeafRec（递归），getNodeNumLeaf（迭代）
**********************************************************************/
    /** 
     * 求二叉树中叶子节点的个数（递归） 
     */ 
    public static int getNodeNumLeafRec(TreeNode root){
    	if (root == null)  
            return 0;  
    	
    	if(root.left==null && root.right==null)
    		return 1;
    	
    	return getNodeNumLeafRec(root.left)+getNodeNumLeafRec(root.right);
    }
    
    /** 
     *  求二叉树中叶子节点的个数（迭代） 
     *  还是基于Level order traversal 
     */ 
    public static int getNodeNumLeaf(TreeNode root){
    	if(root==null)
    		return 0;
    	LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
    	queue.add(root);
    	int numLeaf = 0;
    	while(!queue.isEmpty()){
    		TreeNode cur = queue.remove();
    		
    		if(cur.left==null&&cur.right==null) // 判断是否是叶子节点
    			numLeaf++;
    		if(cur.left!=null)     // 有左孩子，则加入queue
    			queue.add(cur.left);
    		if(cur.right!=null)    // 有右孩子，则加入queue
    			queue.add(cur.right);
    		
    	}
    	// 当循环退出时，返回numLeaf
    	return numLeaf;
   }
```
## 9.判断两颗二叉树是否是相同的(有错误)
```java<br>
/***********************************************************************
	 9. 判断两棵二叉树是否相同的树：isSameRec(递归), isSame(迭代)
**********************************************************************/
    /** 
     * 判断两棵二叉树是否相同的树。 
     * 递归解法：  
     * （1）如果两棵二叉树都为空，返回真 
     * （2）如果两棵二叉树一棵为空，另一棵不为空，返回假  
     * （3）如果两棵二叉树都不为空，如果对应的左子树和右子树都同构返回真，其他返回假 
     */  
    public static Boolean isSameRec(TreeNode root1,TreeNode root2){
    	if(root1==null&&root2==null)
    		return true;
    	if(root1!=null&&root2==null)
    		return false;
    	if(root1==null&&root2!=null)
    		return false;
    	
    	if(root1.val==root2.val){
    		if(isSameRec(root1.left, root2.left)&&isSameRec(root1.right, root2.right))
        		return true;
        	else 
    			return false;
    	}else {
			return false;
		}
    }
    /** 
     * 判断两棵二叉树是否相同的树（迭代） 
     * 遍历一遍即可，这里用preorder 
     */ 
    public static Boolean isSame(TreeNode root1,TreeNode root2){
    	if(root1==null&&root2==null)
    		return true;
    	if(root1!=null&&root2==null)
    		return false;
    	if(root1==null&&root2!=null)
    		return false;
    	
    	Stack<TreeNode> stack1 = new Stack<TreeNode>();
    	Stack<TreeNode> stack2 = new Stack<TreeNode>();
    	stack1.add(root1);
    	stack2.add(root2);
    	while(!stack1.isEmpty()&&!stack2.isEmpty()){
    		TreeNode cur1 = stack1.pop();
    		TreeNode cur2 = stack2.pop();
    		if(cur1.val!=cur2.val){
    			return false;
    		}else{
    			if(cur1.right!=null)
    				stack1.push(cur1.right);
    			if(cur1.left!=null)
    				stack1.push(cur1.left);
    			
    			if(cur2.right!=null)
    				stack2.push(cur2.right);
    			if(cur2.left!=null)
    				stack2.push(cur2.left);
    		}
    	}
    	return true;
   } 
```
## 9.判断两颗二叉树是否是相同的(修改后)
```java<br>
public static Boolean isSame(TreeNode root1,TreeNode root2){
    	// 如果两个树都是空树，则返回true  
        if(root1==null && root2==null){  
            return true;  
        }  
        // 如果有一棵树是空树，另一颗不是，则返回false  
        if(root1==null || root2==null){  
            return false;  
        }  
        Stack<TreeNode> s1 = new Stack<TreeNode>();  
        Stack<TreeNode> s2 = new Stack<TreeNode>();  
        s1.push(root1);  
        s2.push(root2);  
        while(!s1.isEmpty() && !s2.isEmpty()){  
            TreeNode n1 = s1.pop();  
            TreeNode n2 = s2.pop();  
            if(n1==null && n2==null){  
                continue;  
            }else if(n1!=null && n2!=null && n1.val==n2.val){  
                s1.push(n1.right);  
                s1.push(n1.left);  
                s2.push(n2.right);  
                s2.push(n2.left);  
            }else{  
                return false;  
            }  
        }  
        return true; 
    }
```
### 测试类
```java<br>
package com.ywq.package1;
 
import BinaryTree.TreeNode;
 
public class Main {
 
	/* 
		    1  
		  /  \  
		 2    3  
		/ \  / \  
	   4  5 7   6  
	*/  
	public static void main(String[] args){
		
	TreeNode root = new TreeNode(1);  
        TreeNode r2 = new TreeNode(2);  
        TreeNode r3 = new TreeNode(3);  
        TreeNode r4 = new TreeNode(4);  
        TreeNode r5 = new TreeNode(5);  
        TreeNode r6 = new TreeNode(6);
        TreeNode r7 = new TreeNode(7);
          
        root.left = r2;  
        root.right = r3;  
        r2.left = r4;  
        r2.right = r5;  
        r3.right = r6; 
        r3.left = r7;
        
        TreeNode root1 = new TreeNode(1);  
        TreeNode r21 = new TreeNode(2);  
        TreeNode r31 = new TreeNode(3);  
        TreeNode r41 = new TreeNode(4);  
        TreeNode r51 = new TreeNode(5);  
        TreeNode r61 = new TreeNode(6);
        TreeNode r71 = new TreeNode(7);
          
        root1.left = r21;  
        root1.right = r31;  
        r21.left = r41;  
        r21.right = r51;  
        r31.right = r61; 
        r31.left = r71;
        
        System.out.println(getNodeNumRec(root));
        System.out.println(getNodeNum(root));
        
        System.out.println(getDepthRec(root));
        System.out.println(getDepth(root));
        
        
        levelTraversal(root);
        
        System.out.println(getNodeNumKthLevelRec(root.left,1));
        System.out.println(getNodeNumKthLevel(root,2));
        
        System.out.println(getNodeNumLeafRec(root));
        System.out.println(getNodeNumLeaf(root));
        
        System.out.println(isSameRec(root1, root));
        System.out.println(isSame(root1, root));
        
	}
 
}
// 建立二叉树节点类
class TreeNode{
	int val;
	TreeNode left;
	TreeNode right;
	public TreeNode(int val){
		this.val = val;
	}
	
}
```
## 10.判断是否是完全二叉树
```java<br>
/** 
	     判断二叉树是不是完全二叉树（迭代） 
	    若设二叉树的深度为h，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数， 
	    第 h 层所有的结点都连续集中在最左边，这就是完全二叉树。 
	    有如下算法，按层次（从上到下，从左到右）遍历二叉树，当遇到一个节点的左子树为空时， 
	    则该节点右子树必须为空，且后面遍历的节点左右子树都必须为空，否则不是完全二叉树。 
	 */  
    public static Boolean isCompleteBinaryTree(TreeNode root) {
    	if(root==null)
    		return false;
    	// 使用一个队列，将节点依次放入取出
    	Queue<TreeNode> queue = new LinkedList<TreeNode>();
    	queue.add(root);
    	
    	// 设置一个标志位
    	Boolean flag = false;
    	while(!queue.isEmpty()){
    		// 取出一个节点
    		TreeNode cur = queue.remove();
    		if(flag){
    			// 已经出现了有空子树的节点了，后面出现的必须为叶节点（左右子树都为空） 
    			if(cur.left!=null||cur.right!=null){
    				return false;
    			}
    			
    		}else {
				if(cur.left!=null&&cur.right!=null){  // 如果左子树和右子树都非空，则继续遍历 
					queue.add(cur.left);
					queue.add(cur.right);
				}else if(cur.left!=null&&cur.right==null){  // 如果左子树非空但右子树为空，说明已经出现空节点，之后必须都为空子树  
					queue.add(cur.left);
					flag = true;
				}else if(cur.left==null&&cur.right==null){  // 如果左右子树都为空，则后面的必须也都为空子树
					flag = true;
				}else if(cur.left==null&&cur.right!=null){  // 如果左子树为空但右子树非空，说明这棵树已经不是完全二叉完全树！
					return false;
				}
			}
    	}
		return true;
    	}
```
## 11.判断二叉树是不是平衡二叉树
```java<br>

/** 
     * 判断二叉树是不是平衡二叉树 递归解法：  
     * （1）如果二叉树为空，返回真 
     * （2）如果二叉树不为空，如果左子树和右子树都是AVL树并且左子树和右子树高度相差不大于1，返回真，其他返回假 
     */
    public static Boolean isAVLRec(TreeNode root){
    	if(root==null)
    		return false;
    	// 如果左子树和右子树高度相差大于1，则非平衡二叉树, getDepthRec()是前面实现过的求树高度的方法  
    	if(Math.abs(getDepth(root.left)-getDepth(root.right))>1){
    		return false;
    	}
    	// 递归判断左子树和右子树是否为平衡二叉树  
    	return isAVLRec(root.left)&&isAVLRec(root.right);
   }
