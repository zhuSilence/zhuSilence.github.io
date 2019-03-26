---
layout: post
title: The 8th Week of ARTS:144-Binary Tree Preorder Traversal
date: 2018-08-21
tag: ARTS
---

### Introduction
- Algorithm  - Learning Algorithm
- Review  - Learning English
- Tip - Learning Techniques
- Share - Learning Influence

## Let's do it!!!
### Algorithm
#### Binary Tree Preorder Traversal
1. [Description](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)
- Given a binary tree, return the preorder traversal of its nodes' values.
Example:
Input: [1,null,2,3]
   1
    \
     2
    /
   3
Output: [1,2,3]
2. My Solution

```java
package com.silence.arts.leetcode.second;

import java.util.ArrayList;
import java.util.List;

/**
 * <br>
 * <b>Function：</b><br>
 * <b>Author：</b>@author Silence<br>
 * <b>Date：</b>2018-08-15 22:49<br>
 * <b>Desc：</b>无<br>
 */
public class PreorderTraversal_144 {

    /**
     * return list
     */
    private List<Integer> integerList = new ArrayList<>();

    /**
     * treeNode list
     */
    private List<TreeNode> treeNodeList = new ArrayList<>();


    /**
     * Binary Tree Preorder Traversal
     *
     * @param root
     * @return
     */
    public List<Integer> preorderTraversal(TreeNode root) {
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
        integerList.add(root.val);
        if (root.right != null) {
            treeNodeList.add(root);
        }
        if (root.left != null) {
            getVal(root.left);
        } else {
            while (treeNodeList.size() > 0) {
                TreeNode temp = treeNodeList.get(treeNodeList.size() - 1);
                treeNodeList.remove(treeNodeList.size() - 1);
                if (temp.right != null) {
                    getVal(temp.right);
                }
            }

        }
    }


    public static void main(String[] args) {
        PreorderTraversal_144 object = new PreorderTraversal_144();

//        TreeNode root = new TreeNode(2);
//        TreeNode r1 = new TreeNode(3);
//        TreeNode l1 = new TreeNode(1);
////
//        root.left = r1;
//        root.right = null;
////
//        r1.left = l1;
//        r1.right = null;
////
//        l1.left = null;
//        l1.right = null;
////
//        List<Integer> list = object.preorderTraversal(root);
//        System.out.println(list);

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
//        List<Integer> list1 = object.preorderTraversal(n1);
//        System.out.println(list1);

        TreeNode n1 = new TreeNode(4);
        TreeNode n2 = new TreeNode(1);
        TreeNode n3 = new TreeNode(2);
        TreeNode n4 = new TreeNode(3);
        n1.left = n2;
        n1.right = null;

        n2.left = n3;
        n2.right = null;

        n3.left = n4;
        n3.right = null;

        n4.left = null;
        n4.right = null;

        List<Integer> list1 = object.preorderTraversal(n1);
        System.out.println(list1);

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

### Review
#### [The 3 Keys to Becoming Irresistible](https://medium.com/personal-growth/the-3-keys-to-becoming-irresistible-d2f689ea4bf1)
- Humility
- Curiosity
- Empathy

### Tips


### Share
#### Sharing


#### One more thing
- Personal Medium Home Page: [https://medium.com/@zhuxiang134](https://medium.com/@zhuxiang134)
- Personal Website: [https://zxsilence.cn/](https://zxsilence.cn/)