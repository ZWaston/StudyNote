## 1、题目描述

​	在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。 

如：

```java
1 2 8 9
2 4 9 12
4 7 10 13
6 8 11 15
//例如查找数字7，返回true
```

## 2、思路分析

**思路一**：直接遍历整个数组，不可取，时间复杂度O(m*n)。

**思路二**：一般都会这么想，在数组中选一个数字，分三种情况来分析查找的过程：

![面试题4](assets/面试题4.jpg)

基于这种分析，会显得十分复杂，因为选取的位置可能在两个区域出现，而且两个区域还有重叠。

**思路三：**具体问题入手，寻找普遍的规律。

我们首先选取**数组中右上角**的数字，

- 如果该数字 = 要查找的数字，则查找结束；
- 如果该数字 > 要查找的数字，则剔除这个数字所在的列；
- 如果该数字 < 要查找的数字，剔除这个数字所在的行。

实例分析，假设查找数字7，过程如下：

![面试题4_1](assets/面试题4_1.jpg)

## 3、代码实现

```java
/**
 * Created by WASTON on 2018/8/14 17:23
 */
public class _4 {
    public static boolean Find(int target, int [][] array) {
        if(array == null || array.length <= 0 || array[0].length < 0)
            return false;
        //记录数组的行数和列数
        int rows = array.length;
        int columns = array[0].length;
        //记录数组右上角的数字的下标
        int row = 0;
        int column = columns - 1;
        while(row < rows && column >= 0){
            if(array[row][column] == target){
                return true;
            }else if(array[row][column] > target){
                //如果该数字 > 要查找的数字，则剔除这个数字所在的列
                column--;
            }else {
                //如果该数字 < 要查找的数字，剔除这个数字所在的行
                row++;
            }
        }
        return false;
    }

    public static void main(String[] args) {
        int[][] array = {{1,2,8,9},{2,4,9,12},{4,7,10,13},{6,8,11,15}};
        System.out.println(Find(0,array));
    }
}
```

