## 数组相关算法
### 数组简介
```java<br>
数组也是一种极其常见的数据结构，在面试中和数组相关的算法题出现频率贼高。
对数组的操作，一般会要求时间复杂度和空间复杂度。所以，最常用的方法就是设置两个指针，
分别指向不同的位置，不断调整指针指向来实现O（N）时间复杂度内实现算法。
```
常见的面试题有：
拼接一个最大/小的数字、
合并两个有序数组、
调整数组顺序使奇数位于偶数前面、
查找多数元素、
数组中的重复元素
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

### 面试题3：在排序数组中查找数字出现的次数
#### 题目一：数字在排序数组中出现的次数
```java<br>
统计一个数字在排序数组中出现的次数。例如，输入排序数组{1,2,3,3,3,3,4,5}和数字3，由于3在这个数组中出现了4次，所以输出4.
```
##### 思路1
```java<br>
最直观的想法，遍历一遍该数组，统计出现的次数，时间复杂度为O(N)，
可以先和面试官说一下，就说这是最直观的解法（面试官会觉得这小伙子思维比较敏捷，哈哈~）
```
##### 思路1代码
```java<br>
思路一代码：
public int GetNumberOfK(int [] array , int k) {
	       if(array==null||array.length==0)
	           return 0;
	        int count = 0;
	        for(int i =0;i<array.length;i++){
	            if(array[i]==k)
	                count++;
	        }
	        return count;
    }
```
##### 思路2
```java<br>
看到排序数组，必须敏锐的想到可以使用二分查找算法。
二分算法，我们先比较中间的值和目标target的关系，然后分区间找出该数组中第一次出现目标target和最后一次出现target的位置，
两者相减即为该目标出现的次数。整体的时间复杂度为O(logN)
```
##### 思路2代码
```java<br>
思路二代码：
public int GetNumberOfK(int[] array , int target) {
		if(array==null||array.length==0)
			return 0;
		 return getCount(array,0,array.length-1,target);
	}
 
	private int getCount(int[] array, int start, int end, int target) {
		if(array==null||array.length==0)
			return -1;
		if(start>end) // 先判断start和end的关系，防止mid大于数组长度，出现空指针异常
			return 0;
		int mid = start+(end-start)/2;
		int midValue = array[mid];
		
		if(midValue>target)
			return getCount(array, start, mid-1, target);
		else if (midValue<target) 
			return getCount(array, mid+1, end, target);
		else
			return 1+getCount(array, start, mid-1, target)+getCount(array, mid+1, end, target);
	}
```

#### 题目二：0~n-1中缺失的数字
```java<br>
一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0~n-1之内。
在范围0~n-1范围内的n个数字中有且只有一个数字不在该数组中，请找出该数字。
```
#### 题目三：数组中数值和下标相等的元素
```java<br>
假设一个单调递增的数组里边的每个元素都是整数并且是唯一的。请实现一个函数，找出数组中任意一个数值等于其下标的元素。
例如在数组{-3，-1,1,3,5}中，数字3和它的下标相等。
```

### 面试题4：数组中数字出现的次数
#### 题目一
```java<br>
在一个数组中除了一个数字只出现一次之外，其他数字都出现了2次，请找出那个只出现了一次的数字。
要求：线性时间复杂度O(N)，空间复杂度为O(1)
```
##### 思路
```java<br>
思路：用位运算来解决XOR异或来解决该问题。由于两个相同的数字的异或结果是0，
我们可以把数组中的所有数字进行异或操作，结果就是唯一出现的那个数字。
```
##### 代码
```java<br>
public int singleNumber(int[] nums) {
        int ans =0;
    
        int len = nums.length;
        for(int i=0;i!=len;i++)
            ans ^= nums[i];
    
        return ans;
 
    }
```
#### 题目二
```java<br>
在一个数组中除了一个数字只出现一次之外，其他数字都出现了3次，请找出那个只出现了一次的数字。
要求：线性时间复杂度O(N)，空间复杂度为O(1)
```
##### 思路
```java<br>
思路：三个相同的数字异或之后还是本身。转换思路：如果一个数字出现三次，
那么他的二进制表示的每一位(0或者1)也出现3次。如果把所有出现3次的数字的二进制表示的每一位都分别相加起来，那么每一位的和都能被3整除！！！
```
##### 代码
```java<br>
java代码如下：

/**
 * 题目：数组中唯一只出现一次的数字
 * 在一个数组中除了一个数字只出现一次之外，其他数字都出现了3次，请找出那个只出现了一次的数字。
 * 要求：线性时间复杂度O(N)，空间复杂度为O(1)
 * @author ywq
 *
 *
 */
public class Main {
	public static void main(String[] args) {
		int[] nums = {2,3,3,3,4,4,4};
		System.out.println(singleNumber(nums));
	}
	public static int singleNumber(int[] nums) {
		// bitSum数组用来存储二进制加和之后各位的值
        int bitSum[] = new int[32];
        
        for(int i =0;i<nums.length;i++){
            int bitMask = 1;
            for(int j = 31;j>=0;j--){
                int bit = nums[i]&bitMask;
                if(bit!=0)
                    bitSum[j]+=1;
                bitMask = bitMask<<1;
            }
        }
        // 得出result
        int result = 0;
        for(int i = 0;i<32;i++){
            result = result<<1;
            result +=bitSum[i]%3;
        }
        return result;
    }
}
该算法的时间复杂度为O(N)，因为内层循环和后边的循环都是常数级别的时间复杂度。
空间复杂度O(1)，因为数组长度为32的是常数级别的。
```
#### 题目三：数组中唯一出现一次的两个数字（medium）
```java<br>
在一个数组中除了2个数字只出现一次之外，其他数字都出现了2次，请找出两个只出现了一次的数字。
要求：线性时间复杂度O(N)，空间复杂度为O(1)
```
##### 思路
```java<br>
思路：从头到尾异或数组中的每个数字，可以得到只出现1次的两个数字的异或结果。
从异或结果中，找到右边开始第一个不为0的位数，记为n。我们将数组中所有的数字，
按照第n位是否为0，分为两个数组。每个子数组中都包含一个只出现一次的数字，其它数字都是两两出现在各个子数组中。
那么结合题目一，我们已经得出了答案。

举例：假设输入数组{2,4,3,6,3,2,5,5} 异或结果为0010，我们按照倒数第二位是否为1将数组分为了两组，
即{2,3,6,3,2}和数组{4,5,5}接下来只需要按照题目一中的方法即可求出这两个数。
```
##### 代码
```java<br>

/**
 * 题目：数组中唯一只出现一次的两个数字
 * 在一个整型数组中除了两个数字只出现一次之外，其他数字都出现了2次，请找出这两个只出现了一次的数字。
 * 要求：线性时间复杂度O(N)，空间复杂度为O(1)
 * @author ywq
 *
 *
 */
public class Main {
	public static void main(String[] args) {
		int[] nums = {2,3,3,4,4,5,5,7};
		int[] number = singleNumber(nums);
		for (int i : number) {
			System.out.println(i);
		}
	}
	public static int[] singleNumber(int[] nums) {
		
		int ans = 0;
		// 得到数组中所有元素的异或结果
		for(int i = 0;i<nums.length;i++){
			ans^=nums[i];
		}
		int index = findFirstBitIs1(ans);
		
		int first = 0;
		int second = 0;
		for(int j = 0;j<nums.length;j++){
			if(isBit1(nums[j],index)){
				first ^= nums[j];
			}else {
				second ^= nums[j];
			}
		}
		
		int[] result = {first,second};
		return result;
    }
	/**
	 * 判断num的从右边算起的第index位是否为1
	 * @param num
	 * @param index
	 * @return
	 */
	private static boolean isBit1(int num, int index) {
		num = num>>index;
		return (num&1)==1?true:false;
	}
	/**
	 * 得到一个数字num的最右边为1的位数
	 * @param num
	 * @return
	 */
	private static int findFirstBitIs1(int num) {
		int index = 0;
		while((num&1)==0){
			num = num>>1;
			index++;
		}
		return index;
	}
}
```

### 面试题5：调整数组顺序使奇数位于偶数前面
#### 题目一
```java<br>
输入一个整数数组，实现一个函数来调整该数组中数字的顺序，
使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。
```
##### 思路
```java<br>
思路：使用双指针，left和right；left从前往后移动，直到遇到偶数；
right指针向前移动，直到遇到一个奇数；交换两个指针所指向的元素。
```
##### 代码
```java<br>

/**
	 * 可以满足奇数位于偶数前面的算法，但是奇数和奇数、偶数和偶数的相对位置不能保证。
	 * 时间复杂度O(N)，空间O(1)
	 * @param arr
	 */
	private static void reOrderArray(int[] arr){
		if(arr==null||arr.length<2)
			return ;
		int left = 0;
		int right = arr.length-1;
		while(left<right){
			while((arr[left]&1)==1){
				left++;
			} 
			while((arr[right]&1)==0){
				right--;
			}
			// 如果不加此处的if判断语句，会导致right已经在left前面了，但是依然进行了交换。
			// 即将已经在前面的奇数和后面的偶数进行了置换！！！
			if(left<right){
				int temp = arr[left];
				arr[left] = arr[right];
				arr[right] = temp;
			}
		}
	}
```
#### 扩展题
```java<br>
扩展：输入一个整数数组，实现一个函数来调整该数组中数字的顺序，
使得所有的奇数位于数组的前半部分，所有的偶数位于位于数组的后半部分，
并保证奇数和奇数，偶数和偶数之间的相对位置不变
```
##### 思路
```java<br>
首先统计奇数的个数；新建一个等长数组；设置两个指针；遍历旧数组，将各个元素填入新数组中；将新数组赋值给旧数组
```
##### 代码
```java<br>
/**
	 * 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，
	 * 所有的偶数位于位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。
	 * 时间复杂度O(N),空间复杂度O(N)
	 * @param arr
	 */
	private static void reOrderArray2(int[] arr){
		if(arr==null||arr.length<2)
			return ;
		int count = 0;
		// 统计数组中奇数的个数
		for (int i = 0; i < arr.length; i++) {
			if((arr[i]&1)==1){
				count++;
			}
		}
		// 新建一个等长数组
		int[] newArr = new int[arr.length];
		// 设置两个指针，指向新数组
		int left = 0;
		int right = count;
		// 遍历旧数组，将元素移入新数组
		for (int i = 0; i < arr.length; i++) {
			if((arr[i]&1)==1)
				newArr[left++] = arr[i];
			else
				newArr[right++] = arr[i];
		}
		// 新数组代替旧数组
		for(int i = 0; i < newArr.length; i++) {
			arr[i] = newArr[i];
		}
		
	}
```
#### 自我总结
```java<br>
自我总结：
做了一些算法题，感觉只要涉及到数组，大都可以通过设置两个指针指向数组的不同位置，遍历一次数组，在O(N)的时间复杂度中得出结果。
```
