## 数组相关算法
### 数组简介
```java<br>
数组也是一种极其常见的数据结构，在面试中和数组相关的算法题出现频率贼高。
对数组的操作，一般会要求时间复杂度和空间复杂度。所以，最常用的方法就是设置两个指针，
分别指向不同的位置，不断调整指针指向来实现O（N）时间复杂度内实现算法。
```
常见的面试题有：拼接一个最大/小的数字、合并两个有序数组、调整数组顺序使奇数位于偶数前面、查找多数元素、数组中的重复元素
### 面试题1：查找多数元素
#### 题目
```java<br>
找出一个数组中占50%以上的元素，即寻找多数元素，并且多数元素是一定存在的假设。
```
##### 思路1
```java<br>
思路1：将数组排序，则中间的那个元素一定是多数元素

public int majorityElement(int[] nums) {  
    Arrays.sort(nums);  
    return nums[nums.length/2];  
}  
该代码的时间复杂度为O（NlogN），面试官会问你能不能进行优化时间复杂度？
```
##### 思路2
```java<br>
思路2：利用HashMap来记录每个元素的出现次数

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
该代码的时间复杂度为O（N），空间复杂度为O（N）。面试官还不满意，问你能不能用O（N）+O（1）实现该算法？
```
##### 思路3
```java<br>
思路3：Moore voting algorithm--每找出两个不同的element，就成对删除即count--，最终剩下的一定就是所求的。

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

### 面试题2：查把数组排成最小的数
#### 题目
```java<br>
输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。
例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323
```
#### 思路
```java<br>
思路：
我们需要定义一种新的比较大小规则，数组根据这个规则可以排成一个最小的数字。

排序规则：
两个数字m和n，我们比较mn和nm的大小，来确定在新的比较规则下n和m的大小关系，来确定哪个应该排在前面

步骤：
将整型数组转换为String数组。在新的规则下对数组进行排序（本例使用了选择排序）。
```
#### 代码
```java<br>
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
