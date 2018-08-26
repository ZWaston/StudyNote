## 1、题目描述

输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。 

## 2、思路分析

**思路一：**逆转链表，但会修改链表结构。

**思路二**：利用栈的数据结构，后进先出。

**思路三**：既然想到了栈，我们知道递归本质就是一个栈结构，可以用递归来实现，只是可能造成栈溢出。

## 3、代码实现

**思路二代码如下**：

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Deque;

/**
 * Created by WASTON on 2018/8/14 19:32
 */
public class _6 {
    public static class ListNode {
        int val;
        ListNode next = null;
        ListNode(int val) {
            this.val = val;
        }
    }
    public static ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        Deque<ListNode> stack = new ArrayDeque<ListNode>();
        ListNode curNode = listNode;
        while(curNode != null){
            stack.offerFirst(curNode);
            curNode = curNode.next;
        }

        ArrayList<Integer> result = new ArrayList<Integer>();

        while(!stack.isEmpty()){
            curNode = stack.pollFirst();
            result.add(curNode.val);
        }
        return result;
    }
}
```

**思路三代码如下**：

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Deque;

/**
 * Created by WASTON on 2018/8/14 19:32
 */
public class _6 {
    public static class ListNode {
        int val;
        ListNode next = null;
        ListNode(int val) {
            this.val = val;
        }
    }
    
    private static ArrayList<Integer> result = new ArrayList<Integer>();
    
    /**
     * 递归实现
     * @param listNode
     * @return
     */
    public static ArrayList<Integer> printListFromTailToHead_Recursively(ListNode listNode){
        //递归出口：listNode为null
        if(listNode != null){
            if(listNode.next != null){
                printListFromTailToHead_Recursively(listNode.next);
            }
            result.add(listNode.val);
        }
        return result;
    }
}
```

