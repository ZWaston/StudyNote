## 1、题目描述

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。 

## 2、思路分析

栈是后进先出，队列是先进先出。那么很容易想到用两个栈，两次后进先出就是先进先出了。

使用两个栈stack1、stack2，

插入操作：直接压入栈stack1

pop操作：先判断stack2是否有元素，

- 若没有，将stack1的元素全部压入stack2，stack2再进行pop；
- 若有，stack2直接进行pop操作。

## 3、代码实现

```java
import java.util.Stack;

/**
 * Created by WASTON on 2018/8/15 11:08
 */
public class _9 {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();

    public void push(int node) {
        //push直接放在stack1中即可
        stack1.push(node);
    }

    public int pop() {
        int result = -1;//若为空，返回-1
        if(!stack2.empty()){
            result = stack2.pop();
        }else if(!stack1.empty()){
            //若栈2为空且栈1不为空，将栈1全部压入栈2，然后再弹出
            while(!stack1.empty()){
                stack2.push(stack1.pop());
            }
            result = stack2.pop();
        }
        return result;
    }
}
```

