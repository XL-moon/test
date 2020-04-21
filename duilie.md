## 队列和堆栈相关算法
### 定义
```java<br>
队列和堆栈通常在算法题的考察中会作为一种辅助的数据结构出现。分别利用其先进先出和后进先出的特性。

但是，有时候会单纯的考察队列和堆栈的相关知识，常见的算法题包括：包含min函数的堆栈、两个栈实现队列以及自定义堆栈的实现等。
```
`插入图4,5`
### 面试题归纳
#### 1.题目：包含min函数的栈
```java<br>
定义栈的数据结构，请在该类型中实现一个能够得到栈最小元素的min函数。
在该栈中，调用min、push和pop方法。要求的时间复杂度均为O(1)
```
##### 解题思路
```java<br>
思路：题目要求我们的各个方法均为O(1)复杂度，则我们考虑增加辅助空间来实现，即增加一个专门用来存储min值的辅助栈。

比如，data中依次入栈，5, 4, 3, 8, 10, 11, 12, 1。则辅助栈依次入栈， 5, 4, 3，no,no, no, no, 1。no代表此次不如栈。即，每次入栈的时候，如果入栈的元素比min中的栈顶元素小或等于则入栈，否则不入栈。
```
##### 代码
```java<br>
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
`该实现算法中，在push和pop操作中，均有判断，判断值相等一定要用peek方法而不是pop！！！切记切记，关键点。`

#### 2.题目：用两个栈实现队列
```java<br>
用两个栈实现一个队列。队列的声明如下，请实现它的两个函数appendTail和deleteHead，
分别完成在对尾部插入节点和在队列头部删除节点功能。
```
##### 解题思路
```java<br>
栈是一种后进先出的数据结构，而队列是一种先进先出的数据结构。如何使用栈来实现队列呢？
思路：
首先，先建立两个栈，包括栈1和栈2。在入栈操作时，我们将节点push到栈1中。
其次，出栈操作时，判断栈2是否有节点，若有，则直接pop栈2；反之，判断栈1是否也为空，若为空，则抛出异常；反之，将栈1节点依次弹出，再存入栈2中，最后，从栈2中弹出一个元素。

```
##### 代码
```java<br>
/*
 * 剑指Offer面试题9：用两个栈实现队列
 * 题目：用两个栈实现一个队列。队列的声明如下，请实现它的两个函数appendTail和deleteHead，分别完成在对垒尾部插入节点
 * 和在队列头部删除节点功能。
 */
class MyQueue<T>{
	// 定义两个堆栈
	Stack<T> stack1 = new Stack<>();
	Stack<T> stack2 = new Stack<>();
	
	/**
	 * 所有的元素都插入在stack1中
	 * @param n
	 */
	public void appendTail(T n){
		stack1.push(n);
	}
	
	/**
	 * 删除元素从stack2中删除；若stack2为空，则将stack1中的元素取出，放入stack2中，之后再次从stack2中取出元素删除。
	 * @return
	 */
	public T deleteHead(){
		if(stack2.isEmpty()){
			while(!stack1.isEmpty()){
				stack2.push(stack1.pop());
			}
		}
		if(stack2.isEmpty()){
			try {
				throw new Exception("队列中没元素啦");
			} catch (Exception e) {
				e.printStackTrace();
			}
		}
		return stack2.pop();
	}
	
}
```

#### 3.题目：用两个队列实现一个栈
##### 解题思路
```java<br>
思路：
将元素都存入queue1中，当执行pop出栈操作时，将queue1中的节点依次出列，
并且添加到queue2中，将最后一个出列的节点删除并且返回，则搞定。
```
##### 代码
```java<br>
java代码如下：
/*
 * 相关题目：用两个队列实现一个栈
 */
class MyStack<E>{
	// 定义两个队列
	Queue<E> queue1 = new LinkedList<>();
	Queue<E> queue2 = new LinkedList<>();
	
	public void push(E n){
		queue1.add(n);
	}
	
	public E pop(){
		if(queue1.isEmpty()&queue2.isEmpty())
			try {
				throw new Exception("栈中没元素啦");
			} catch (Exception e) {
				e.printStackTrace();
			}
		// 若队列1不为空
		if(!queue1.isEmpty()){
			while(queue1.size()>1){
				queue2.add(queue1.remove());
			}
			return queue1.remove();
		}
		// 当队列1为空时，操作队列2
		while(queue2.size()>1){
			queue1.add(queue2.remove());
		}
		return queue2.remove();
		
	}
	
	public E peek(){
		if(queue1.isEmpty()&queue2.isEmpty())
			try {
				throw new Exception("栈中没元素啦");
			} catch (Exception e) {
				e.printStackTrace();
			}
		// 若队列1不为空
		if(!queue1.isEmpty()){
			while(queue1.size()>1){
				queue2.add(queue1.remove());
			}
			return queue1.peek();
		}
		// 当队列1为空时，操作队列2
		while(queue2.size()>1){
			queue1.add(queue2.remove());
		}
		return queue2.peek();
	}
}
```
`如果是使用一个队列来实现栈，可以使用双端队列`

### 自定义实现堆栈
堆栈（Stack）是一种常见的数据结构，符合后进先出（First In Last Out）原则，通常用于实现对象存放顺序的逆序。
栈的基本操作有push（添加到堆栈）,pop（从堆栈删除）,peek（检测栈顶元素且不删除）。

```java<br>

```
