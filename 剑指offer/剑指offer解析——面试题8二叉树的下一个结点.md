## 1、题目描述

​	给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针 。

## 2、思路分析

主要考察分类讨论的思想。

分析一个结点，有3种情况：

- 若结点有右子树，则下一个结点就是右子树的最左结点；
- 若结点没有右子树，
  - 如果该结点是父结点的左结点，那么下一个结点就是父结点（父结点为空就返回空）；
  - 如果该结点是父结点的右结点，那么情况要复杂一点。我们需要沿着父结点的指针向上遍历，知道找到一个是节点，它是父结点的左子结点。

## 3、代码实现

```java
/**
 * Created by WASTON on 2018/8/15 10:26
 */
public class _8 {
    public class TreeLinkNode {
        int val;
        TreeLinkNode left = null;
        TreeLinkNode right = null;
        TreeLinkNode next = null;//这个指向父节点

        TreeLinkNode(int val) {
            this.val = val;
        }
    }
    public TreeLinkNode GetNext(TreeLinkNode pNode) {
        if(pNode == null) return null;
        TreeLinkNode pNext = null;
        if(pNode.right != null){
            //若节点有右子树，那么中序的下一个结点是右子树的最左结点
            TreeLinkNode curNode = pNode.right;
            while(curNode.left != null){
                curNode = curNode.left;
            }
            pNext = curNode;
        }else if(pNode.next != null){
            //若结点没有右子树，而且有父结点，分两种情况讨论
            //若该结点是父结点的左结点，那么pNext就是父结点，
            //否则，需要沿着父结点的指针向上遍历，知道找到一个是它父结点的左子结点的节点。
            TreeLinkNode curNode = pNode;
            TreeLinkNode parentNode = pNode.next;//当前节点的父结点
            while(parentNode != null && parentNode.right == curNode){
                //当该结点是父结点的右结点，或者父结点不为null时，进入循环
                curNode = parentNode;
                parentNode = parentNode.next;
            }

            //进入此步，说明当前结点curNode是父结点的左结点，下一个结点就是其父结点
            pNext = parentNode;

        }
        return pNext;
    }
}
```

