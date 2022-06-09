---
title: Recursion and iteration
date: 2022-04-10 02:14:23
tags: Computer Science
---

```
156. 给你一个二叉树的根节点 root ，请你将此二叉树上下翻转，并返回新的根节点。

你可以按下面的步骤翻转一棵二叉树：

    原来的左子节点变成新的根节点
    原来的根节点变成新的右子节点
    原来的右子节点变成新的左子节点

上面的步骤逐层进行。题目数据保证每个右节点都有一个同级节点（即共享同一父节点的左节点）且不存在子节点。

```

```
class Solution {
    public TreeNode upsideDownBinaryTree(TreeNode root) {
        if(root==null)
            return root;
        TreeNode res=new TreeNode(root.val);
        while(root.left!=null){
            res=root.right!=null?new TreeNode(root.left.val, new TreeNode
                (root.right.val), res):new TreeNode(root.left.val, null,
                res);
            root=root.left;
        }
        return res;
    }
}
```
受本题启发在这里总结关于迭代和递归过程中问题规模递减的顺序和迭代/递归过程顺序的关系。
1. 对于本而言考虑这组输入：`root = [1,2,3,4,5]`；
2. 本题问题规模递减的顺序是：
   1. 生成子树`[2,3,1]`；
   2. 生成子树`[4,5,2,null,null,3,1]`；
3. 由(2)可知，问题解决的顺序与遍历的顺序是相同的，都是从前往后=>迭代解决；

在来考虑递归的解决顺序：

1. 自顶向下的递归：当前问题在被遍历到的时候就已经计算进答案中；
   
   1. 问题的解决顺序与遍历的顺序是相同的；
2. 自底向上的递归：当前问题在子问题解决的基础上解决，因此递归到更小的子问题然后根据更小子问题的返回值解决当前问题；
   
   1. 解决问题的顺序和遍历的顺序相反；
3. (1)和(2)两种递归的区别：
   1. 有返回值的情况：
        * 自顶向下return recursion(n-1)——问题规模递减到最小时可以得到最终的答案，返回上一层时不需要做进一步处理;
        * 自底向上return f(recursion(n-1));——问题规模减小到最小时可以得到规模最小的子问题答案，返回上一层需要处理后才能得到高层次的答案；
    2. 没有返回值的情况：
        * 自顶向下recursion(n-1);
        * 自底向上由于需要子问题的结果作为当前问题的结果因此必须有返回值；
        * 但是有一个特例，当传入引用类型的数据作为参数时，答案可以记录在这个引用变量里；
4. 分析二叉树深度优先搜索的递归过程：
    ```
    public void recursion(int[] nums, int depth, boolean[] isValid, Deque<Integer> path, List<List<Integer>> res){
        if(depth==nums.length){
            res.add(new ArrayList(path));
            return;
        }
        
        for(int i=0;i<nums.length;i++){
            if(!isValid[i]){
                path.addLast(nums[i]);
                isValid[i]=false;
                recursion(nums, depth+1, isValid, path, res);
                isValid[i]=true;
                path.removeLast();
            }
        }
    }
    ```
    1. 这个过程中不止涉及一个递归过程，循环的每一个分支都是一个递归过程，每个递归过程中递归到最后直接得到答案，因此每个分支的递归过程都是自顶向下的；
    2. 再看循环过程，循环过程的语义是选择，也就是操作树的分支；
    3. 本题是有分支的递归，两个分支具有相同的原问题，因此本题是自顶向下递归的变种，本质上还是自顶向下的递归；
 1. 回头考虑156题，根据其问题规模递减的顺序和遍历顺序应该采用自顶向下的递归，因此由当前问题进入下一问题后得到的应该是最终的答案，即采用return recursion(n-1);
   ```
   public TreeNode recursion(TreeNode root, TreeNode right){
       if(root==null)
        return right;
       
       TreeNode left=root.right!=null?new TreeNode(root.right.val):null;
       return recursion(root.left, new TreeNode(root.right.val, left, right));
   }
   ```
   本文中对于递归不同种类的代码形式是重要的，可以帮助我们很快的组织代码；