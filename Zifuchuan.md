## 字符串相关算法
### 字符串简介
```java<br>
字符串是一种非常常见的数据类型，我们应当对API中常用的函数进行一定的学习和了解。
在字符串的考察中，比较经典的是自定义一个函数实现字符串转整数的功能；
加大难度之后，会考察以下常见的5种关于字符串中“最长”问题：
1.最长公共子序列
2.最长公共子串
3.最长递增子序列
4.最长公共前缀
5.最长不含重复元素的子串
```
### 面试题
#### 题目
```java<br>
自定义一个函数，实现字符串转换成整数。
```
#### 解题分析
```java<br>
分析：
这道题看似简单，而且面试官也没有提醒你应该注意哪些。但是！！，这道题的目的就是为了考察你的特殊情况处理能力，
你能不能想到会有哪些特殊情况或者边界处理，这才是本题的重点。
那么，遇到这道题，我们应该如何面对？我们要不慌不乱的和面试官亲切交谈，制定该函数的一些规则，即如何处理异常输入等，
之后，再遍历数组，根据需求进行相应的异常处理哦~

思路：
首先，我们应该要想到本题的一些特殊情况。
本题的特殊情况如下：
1.能够排除首部的空格，从第一个非空字符开始计算
2.允许数字以正负号(+-)开头
3.遇到非法字符便停止转换，返回当前已经转换的值，如果开头就是非 法字符则返回0
4.在转换结果溢出时返回特定值，这里是最大/最小整数
5.我们需要针对以上的特殊情况和面试官亲切交流，询问如果有特殊情况该如何处理。

其次，我们要想到一些测试用例，根据测试用例来询问面试官输出是否应该为XX。
先来几组测试用例：

"    010"
"    +004500"
"  -001+2a42"
"   +0 123"
"-2147483648"
"2147483648"
"   - 321"
"      -11919730356x"
"9223372036854775809"

以上的测试用例对应的正确输出如下：
10
4500
-1
0
-2147483648
2147483647
0
-2147483648
2147483647
如果你能想到这些特殊情况和测试用例，那么恭喜你，你已经成功了90%，面试官会从心底里开始欣赏你。
```
#### 代码
```java<br>
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
### 5种关于字符串中“最长”问题的解法
常见的5种关于字符串中“最长”问题：
1.最长公共子序列
2.最长公共子串
3.最长递增子序列
4.最长公共前缀
5.最长不含重复元素的子串

#### 1.最长公共子序列
```java<br>
子序列不需要连续，给定两个不同长度的字符串，如何求出最长公共子序列？
```
##### 递归解法
```java<br>
递归解法：
/**
	 * 最长公共子序列，返回值为长度
	 * @param x
	 * @param y
	 * @return
	 */
	int longestPublicSubSequence(String x, String y){
		if(x.length() == 0 || y.length() == 0){
			return 0;
		}
		if(x.charAt(0) == y.charAt(0)){
			return 1 + longestPublicSubSequence(x.substring(1), y.substring(1));
		}else{
			return Math.max(longestPublicSubSequence(x.substring(1), y.substring(0)), 
					longestPublicSubSequence(x.substring(0), y.substring(1)));
		}
	}
  
```
##### 非递归解法
```java<br>
非递归解法：
int getLCS(String str, String str2){
     
       int n1 = str.length();
       int n2 = str2.length();
       int[][] dp = new int[n1+1][n2+1];
       for(int i=1;i<=n1;i++){
           for(int j=1;j<=n2;j++){
               if(str.charAt(i-1)==str2.charAt(j-1)){ //此处应该减1.
                   dp[i][j]=dp[i-1][j-1]+1;
               }else{
                   dp[i][j]=Math.max(dp[i-1][j],dp[i][j-1]);
               }
           }
       }
       return dp[n1][n2];
   }
画图分析，最长公共子序列：
若对应位置字符相等，则可以这样表示：
```
`插入图片7`
```java<br>
反之，若不相等，则表示如下：
```
`插入图片8`<br>
递归和非递归的区别是，非递归方法中将结果保存在数组中。

#### 2.最长公共子串
```java<br>
/**
	 * 最长公共子串
	 * @param str1
	 * @param str2
	 * @return
	 */
	int longestPublicSubstring(String str1, String str2){
		int l1 = str1.length();
		int l2 = str2.length();
		int[][] val = new int[l1][l2];
		for(int i = 0; i < l1; i++){
			for(int j = 0; j < l2; j++){
				if(str1.charAt(i) == str2.charAt(j)){
					if(i >= 1 && j >= 1){
						val[i][j] = val[i - 1][j - 1] + 1;
					}else{
						val[i][j] = 1;
					}
				}else{
					val[i][j] = 0;
				}
			}
		}
		// 找到val数组中的最大值
		int max = 0;
		for(int i = 0; i < l1; i++){
			for(int j = 0; j < l2; j++){
				max = Math.max(val[i][j], max);				
			}
		}
		return max;
	}
画图分析：
```
`插入图片9`


#### 3.最长递增子序列
```java<br>
这是一道典型的动态规划试题。使用memo[ ]数组保存中间结果
/**
	 * 最长递增子序列 动态规划
	 * @param num
	 * @return
	 */
	public static int LongestLIS(int[] num){
		if(num.length<1)
			return 0;
		// 定义一个memo数组memo[i]表示以num[i]结尾的序列的长度-1
		int[] memo = new int[num.length];
		for(int i = 1;i<num.length;i++){
			for(int j = 0;j<i;j++){
				if(num[i]>num[j]){
					memo[i] = Math.max(memo[i], 1+memo[j]);
				}
			}
		}
		// 遍历memo数组，找到最大值，并且返回max+1;
		int max = 0;
		for (int i = 0; i < memo.length; i++) {
			max = Math.max(max, memo[i]);
		}
		return max+1;
	}
```

#### 4.最长公共前缀
```java<br>
/*
 * 求一个字符串数组的最长公共前缀
 * Write a function to find the longest common prefix string amongst an array of strings.
 */
public class Main {
     public static void main(String[] args) {
          String[] s = {"acvxx","axc","aaa"};
          System.out.println(longestCommonPrefix(s));
     }
     public static String longestCommonPrefix(String[] strs) {
          if(strs == null || strs.length == 0)   
              return "";
         String pre = strs[0];
         int i = 1;
         while(i < strs.length){
             while(strs[i].indexOf(pre) != 0)  // 字符串String的indexOf方法使用
                 pre = pre.substring(0,pre.length()-1);
             i++;
         }
         return pre;
    }
}
该方法巧妙之处就是充分利用了indexOf( )方法，先将数组中的第一个元素作为最长前缀，然后向后遍历数组，判断是否是后边字符串的前缀，若不是，则从后边减小该前缀，直到最后前缀为“”。

```
#### 5.最长不含重复元素的子串
##### 题目
`插入图片10`
##### 解题分析
```java<br>
题目的大意是说：请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。假设该字符串中只包含‘a’-'z'的字符。
打开tag瞅瞅，如下：
```
`插入图片11`
```java<br>
于是我们使用了HashMap和双指针来解决该问题。
思路：HashMap中不断更新维护每个字符出现的位置。使用count变量进行计数，当统计的最长子字符串被重复字符破坏时，比较count和max的大小，判断是否需要更新max变量。
总结：不管是哪种情况，每次都需要更新map中的信息，并且需要更新count变量
```
##### 代码
```java<br>
java实现如下：

package pak3;
 
import java.util.HashMap;
import java.util.Map;
 
/**
 * 3. Longest Substring Without Repeating Characters
 * 求最长不含重复字符的子字符串
 * @author ywq
 *
 *
 */
public class Main {
	public static void main(String[] args) {
		System.out.println(lengthOfLongestSubstring("arbacacfr"));
		System.out.println(lengthOfLongestSubstring("hkcpmprxxxqw"));
		System.out.println(lengthOfLongestSubstring("dvdf"));
		System.out.println(lengthOfLongestSubstring("tmmzuxt"));
		System.out.println(lengthOfLongestSubstring("jbpnbwwd"));
		
	}
	
	public static int lengthOfLongestSubstring(String s) {
        if(s==null||s.length()==0)
        	return 0;
        // 建立一个HashMap用来存放字符和位置信息
        Map<Character,Integer> map = new HashMap<Character, Integer>();
        int max = 0; // 用来记录最大值
        int count = 0; // 用来统计长度
        char[] c = s.toCharArray();
        for(int i =0;i<c.length;i++){
        	if(!map.containsKey(c[i])){
        		map.put(c[i], i);
        		count++;
        	}else {
        		// 若map中已经包含该字符，分为两种情况讨论
				Integer index = map.get(c[i]);
				// 情况1：上次出现的该字符并不在当前所统计的最长字符串中，只需要更新位置信息。并且统计count++
				if(i-index>count){
					count++;
					map.put(c[i], i);
					continue;
				}
				// 情况2：上次出现的该字符影响了当前最长不重复的子字符串
				// 则更新位置信息、max变量和count计数
				map.put(c[i], i);
				if(count>max){
	        		max = count;
	        	}
				count = i - index;
			}
        }
        // 防止出现没有重复字符的情况，此时max = 0
        return max>count?max:count;
    }
}
```
##### 结果图
```java<br>
LeetCode AC图：
```
`插入图片12`
