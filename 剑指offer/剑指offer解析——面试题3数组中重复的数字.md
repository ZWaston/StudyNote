## 1、题目描述

​	在一个长度为n的数组里的所有数字都在**0到n-1的范围**内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。 

## 2、思路分析

**思路一：**

将输入的数组排序，然后从排序的数组中找重复数组，排序时间复杂度为O(nlogn)。

**思路二：**

利用哈希表，用空间换时间。从头到尾扫描数组，每扫描一个数字，用O(1)判断哈希表中是否有该数字，若没有，将这个数字加入哈希表，否则输出这个重复数字。

空间复杂度：O(n)，时间复杂度O(n)。

**思路三：**

从题目中找出特点：数组里的所有数字都在**0到n-1的范围**内。

- 如果数组没有重复的数字，排序之后数字 i 将出现在下标为 i 的位置；

- 若存在重复数字，排序之后有些下标i就没有数字i。我们重排这个数组，从头扫描数组中的每个数字。当扫描到下标为 i 的数字，比较这个数字m是否等于i，
  - 如果等于，则接着扫描下一个数字；
  - 如果不等于，则拿它与第 m 个数字进行比较，
    - 如果它与第m个数字相等，就找到第一个重复的数字了；
    - 如果不相等，将 第i个数字 和 第m个数字 进行交换。接下来重复第i个数字的比较、交换的过程。

其实，思路很简单，就是一句话，将数字 i 放在下标为 i 的位置，如果发现下标为 i 的位置已经有数字 i 了，就说明有重复数字。 

**举例演示算法流程**

如{2,3,1,0,2,5,3}，第一次扫描下标为0的数字，

会经过以下变换：{1,3,2,0,2,5,3} -> {3,1,2,0,2,5,3} -> {0,1,2,3,2,5,3}，此时下标为0的数字为0，扫描数组的下一个下标的数字。

## 3、Java代码实现

```java
/**
 * 面试题3：
 * Created by WASTON on 2018/8/14 11:50
 */
public class _3 {
    /**
     *
     * @param numbers
     * @param length
     * @param duplication 若有重复数字，将其任意一个复制给duplication[0]
     * @return
     */
    public static boolean duplicate(int numbers[],int length,int [] duplication) {
        if(numbers == null || numbers.length <= 0){
            return false;
        }
        //判断数组是否在0-n的范围
        for(int i = 0; i < length; i++){
            if(numbers[i] < 0 || numbers[i] > length - 1)
                return false;
        }

        //从头扫描数组中的每个数字
        for(int i = 0; i < length; i++){
            //当扫描到下标为 i 的数字，比较这个数字m是否等于i,如果等于，则接着扫描下一个数字
            while(numbers[i] != i){
                //若不等于，则拿它与第 m 个数字进行比较
                if(numbers[i] == numbers[numbers[i]]){
                    //若相等，说明有重复数字
                    duplication[0] = numbers[i];
                    return true;
                }
                //若不等于，将第i个数字 和 第m个数字 进行交换，重复下标i的比较、交换
                int temp = numbers[i];
                numbers[i] = numbers[temp];
                numbers[temp] = temp;
            }
        }
        //若输入数组没有重复，返回false
        return false;
    }
    public static void main(String[] args) {
        int[] array = {2,3,1,0,2,5,3};
        int[] result = new int[1];
        boolean flag = duplicate(array,array.length,result);
        if(flag){
            System.out.println(result[0]);
        }else{
            System.out.println("无重复数字！");
        }

    }
}
```

