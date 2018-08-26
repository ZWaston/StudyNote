## 1、题目描述

用两个队列来实现一个栈，完成栈的add和remove操作。 栈中的元素为int类型。 

## 2、思路分析

两个队列queue1和queue2，

add操作：将新元素插入到有元素的那一个队列中

remove操作：将有元素的那个队列（设有n个元素），删除其n-1个元素保存至另一个队列，然后在删除第n个元素，并返回。

## 3、代码实现

```java
import java.util.ArrayDeque;
import java.util.Queue;

/**
 * 两个队列实现栈
 * Created by WASTON on 2018/8/15 11:24
 */
public class _9_1 {
    Queue<Integer> queue1 = new ArrayDeque<Integer>();
    Queue<Integer> queue2 = new ArrayDeque<Integer>();

    public void add(int node){
        if(queue1.isEmpty()){
            queue2.add(node);
        }else{
            queue1.add(node);
        }
    }

    public int remove() {
        if(queue1.isEmpty() && queue2.isEmpty())
            return -1;
        int result = -1;
        Queue<Integer> notEmptyQueue;
        Queue<Integer> emptyQueue;
        if(!queue1.isEmpty()){
            notEmptyQueue = queue1;
            emptyQueue = queue2;
        }else {
            notEmptyQueue = queue2;
            emptyQueue = queue1;
        }
        int curNode = notEmptyQueue.remove();
        while(!notEmptyQueue.isEmpty()){
            emptyQueue.add(curNode);
            curNode = notEmptyQueue.remove();
        }
        result = curNode;
        return result;
    }

    public static void main(String[] args) {
        _9_1 stack = new _9_1();
        stack.add(2);
        stack.add(3);
        stack.add(4);
        stack.add(5);
        System.out.println(stack.remove());
        System.out.println(stack.remove());
        System.out.println(stack.remove());
        System.out.println(stack.remove());
        System.out.println(stack.remove());
    }
}
}
```

