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
#### 堆栈定义
```java<br>
堆栈（Stack）是一种常见的数据结构，符合后进先出（First In Last Out）原则，通常用于实现对象存放顺序的逆序。
栈的基本操作有push（添加到堆栈）,pop（从堆栈删除）,peek（检测栈顶元素且不删除）。
```
#### 实现方式1
使用一个队列实现
可以使用LinkedList或者ArrayDeque实现，主要是实现其常用的push、pop以及peek方法。
```java<br>
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.LinkedList;
 
public class MyStackTest{
	public static void main(String[] args) {
		MyStack<Integer> stack = new MyStack<Integer>();
		// 将0、1、2、3、4存入堆栈stack
		for (int i = 0; i < 5; i++) {
			stack.push(i);
		}
		System.out.println("After pushing 5 elements: " + stack);
		int m = stack.pop();
		System.out.println("Popped element = " + m);
		
		System.out.println("After popping 1 element : " + stack);
		int n = stack.peek();
		System.out.println("Peeked element = " + n);
		System.out.println("After peeking 1 element : " + stack);
	}
}
class MyStack<T> {
//	private Deque<Integer> queue = new ArrayDeque<Integer>();
	private Deque<Integer> queue = new LinkedList<Integer>();
    // push存入元素
	public void push(Integer element) {
		queue.addFirst(element);
	}
	// pop取出元素
	public Integer pop() {
		return queue.removeFirst();
	}
    // peek获取元素，但是并不取出元素
	public Integer peek() {
		return queue.getFirst();
	}
 
	public String toString() {
		return queue.toString();
	}
 
}
LinkedList:特有方法：
addFirst();
addLast();

getFirst();
getLast();
获取元素，但不删除元素。如果集合中没有元素，会出现NoSuchElementException

removeFirst();
removeLast();
获取元素，但是元素被删除。如果集合中没有元素，会出现NoSuchElementException

在JDK1.6出现了替代方法。
offerFirst();
offerLast();

peekFirst();
peekLast();
获取元素，但不删除元素。如果集合中没有元素，会返回null。

pollFirst();
pollLast();
获取元素，但是元素被删除。如果集合中没有元素，会返回null
```

#### 实现方式2
使用int数组来实现
```java<br>

public class StackTest{
	public static void main(String[] args) {
		Stack2 stack = new Stack2();
		// 将0、1、2、3、4存入堆栈stack
		for (int i = 0; i < 5; i++) {
			stack.push(i);
		}
		System.out.println("After pushing 5 elements: " + stack);
		int m = stack.pop();
		int m1 = stack.pop();
		int m2 = stack.pop();
		int m3 = stack.pop();
		System.out.println("Popped element = " + m3);
		
		System.out.println("After popping 1 element : " + stack);
		int n = stack.peek();
		System.out.println("Peeked element = " + n);
		System.out.println("After peeking 1 element : " + stack);
	}
}
class Stack2 {
    /**
     * 栈的最大深度
     **/
    protected int MAX_DEPTH = 10;
 
    /**
     * 栈的当前深度
     */
    protected int depth = 0;
 
    /**
     * 实际的栈
     */
    protected int[] stack = new int[MAX_DEPTH];
 
    /**
     * push，向栈中添加一个元素
     *
     * @param n 待添加的整数
     */
    protected void push(int n) {
        if (depth == MAX_DEPTH - 1) {
            throw new RuntimeException("栈已满，无法再添加元素。");
        }
        stack[depth++] = n;
    }
 
    /**
     * pop，返回栈顶元素并从栈中删除
     *
     * @return 栈顶元素
     */
    protected int pop() {
        if (depth == 0) {
            throw new RuntimeException("栈中元素已经被取完，无法再取。");
        }
 
        // --depth，dept先减去1再赋值给变量dept，这样整个栈的深度就减1了（相当于从栈中删除）。
        return stack[--depth];
 
    }
 
    /**
     * peek，返回栈顶元素但不从栈中删除
     *
     * @return
     */
    protected int peek() {
        if (depth == 0) {
            throw new RuntimeException("栈中元素已经被取完，无法再取。");
        }
        return stack[depth - 1];
    }
    
    @Override
    public String toString() {
    	// TODO Auto-generated method stub
    	return stack.toString();
    }
}
这种实现方式有个问题，在pop方法中，只是单纯的移动了指针，相当于从堆栈中删除了该元素，其实并没有做到真正删除。虽然也可以实现简单的堆栈功能，但会产生如图所示的现象：
```
`插入图片6`
