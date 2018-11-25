---
layout: post
title: The 7th Week of ARTS:94-Binary Tree Inorder Traversal
date: 2018-08-11
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm
#### Binary Tree Inorder Traversal
1. [Description](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)
- Given a binary tree, return the inorder traversal of its nodes' values.
Example:
Input: [1,null,2,3]
```
   1
    \
       2
    /
   3
```

Output: [1,3,2]
2. My Solution

```java
package com.silence.arts.leetcode.second;

import java.util.ArrayList;
import java.util.List;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-08-07 23:21<br>
 * <b>Desc：</b>无<br>
 */
public class Binary_Tree_Inorder_Traversal_94 {
    private List<Integer> integerList = new ArrayList<>();
    private List<TreeNode> treeNodeList = new ArrayList<>();
    /**
     * Binary Tree Inorder Traversal
     * 二叉树中序遍历
     *
     * @param root
     * @return
     */
    public List<Integer> inorderTraversal(TreeNode root) {
        if (root != null) {
            getVal(root);
            return integerList;
        }
        return integerList;
    }

    /**
     * Recursion 递归
     *
     * @param root
     */
    private void getVal(TreeNode root) {
        if (root.left != null) {
            treeNodeList.add(root);
            getVal(root.left);
        } else {
            integerList.add(root.val);
            if (root.right != null) {
                getVal(root.right);
            } else {
                while (treeNodeList.size() > 0) {
                    TreeNode temp = treeNodeList.get(treeNodeList.size() - 1);
                    treeNodeList.remove(treeNodeList.size() - 1);
                    integerList.add(temp.val);
                    if (temp.right != null) {
                        getVal(temp.right);
                    }
                }
            }
        }
    }

    public static void main(String[] args) {
        Binary_Tree_Inorder_Traversal_94 object = new Binary_Tree_Inorder_Traversal_94();
//
//        TreeNode root = new TreeNode(2);
//        TreeNode r1 = new TreeNode(3);
        TreeNode l1 = new TreeNode(1);
//
//        root.left = r1;
//        root.right = null;
//
//        r1.left = l1;
//        r1.right = null;
//
        l1.left = null;
        l1.right = null;
//
        List<Integer> list = object.inorderTraversal(l1);
        System.out.println(list);

//        TreeNode n1 = new TreeNode(1);
//        TreeNode n2 = new TreeNode(2);
//        TreeNode n3 = new TreeNode(3);
//        TreeNode n4 = new TreeNode(4);
//        TreeNode n5 = new TreeNode(5);
//        TreeNode n6 = new TreeNode(6);
//        TreeNode n7 = new TreeNode(7);
//        TreeNode n8 = new TreeNode(8);
//        TreeNode n9 = new TreeNode(9);
//
//        n1.left = n2;
//        n1.right = n3;
//        n2.left = n4;
//        n2.right = n5;
//        n3.left = n6;
//        n3.right = n7;
//        n4.left = n8;
//        n4.right = null;
//        n5.left = n9;
//        n5.right = null;
//        n6.left = null;
//        n6.right = null;
//        n7.left = null;
//        n7.right = null;
//        n8.left = null;
//        n8.right = null;
//
//        List<Integer> list1 = object.inorderTraversal(n1);
//        System.out.println(list1);

//        TreeNode n1 = new TreeNode(4);
//        TreeNode n2 = new TreeNode(1);
//        TreeNode n3 = new TreeNode(2);
//        TreeNode n4 = new TreeNode(3);
//        n1.left = n2;
//        n1.right = null;
//
//        n2.left = n3;
//        n2.right = null;
//
//        n3.left = n4;
//        n3.right = null;
//
//        n4.left = null;
//        n4.right = null;
//
//        List<Integer> list1 = object.inorderTraversal(n1);
//        System.out.println(list1);

    }
}

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}
```
> Thinking
> 1. Traversing current Node, put it into treeNodeList if it has left subtree; then traverse its left subtree until the leaves node.
> 2. Putting current node's value into integerList if it doesn't have left subtree;
> 3. Traversing treeNoeList from last node until end.

---
> 思路
> 1. 先遍历当前节点，如果当前节点有左子树，则将当前节点存放到treeNodeList中，再依次遍历当前节点的左子树，直到叶子节点；
> 2. 如果当前节点没有左子树，则将当前节点的值存放到integerList中；
> 3. 遍历treeNodeList取出最后一个遍历其右子树，直到结束。

### Review
#### [Warning: Your programming career](https://medium.com/sololearn/warning-your-programming-career-b9579b3a878b)
The main idea of the article is How not to get lost in the dark forests of programming!
There are many skills we need to learn in order to improve ourselves.

### Tips
1. We should use `CONCAT()` to combine sql and variable when we need to dynamic tableName
`CONCAT(" select * from ",toTableName)`

### Share
#### Sharing
1. Neo4j
- Need A graph-like structure (Facebook friends graph, Google knowledge graph, etc.)? Choose Neo4j.
2. ArangoDB - Wikipedia
- ArangoDB is a native multi-model database system[1] developed by triAGENS GmbH. The database system supports three important data models (key/value, documents, graphs) with one database core and a unified query language AQL (ArangoDB Query Language). The query language is declarative and allows the combination of different data access patterns in a single query. ArangoDB is a NoSQL database system but AQL is similar in many ways to SQL.
> Cool!

#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [http://zxsilence.cn/](http://zxsilence.cn/)
