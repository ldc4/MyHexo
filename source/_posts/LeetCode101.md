title: LeetCode No.101 Symmetric Tree
date: 2016-03-17 13:21:14
categories:
- Arithmetic
tags:
- leetcode
- DFS
---

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).
For example, this binary tree is symmetric:
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
<!--more-->
But the following is not:
```
    1
   / \
  2   2
   \   \
   3    3
```

Note:
Bonus points if you could solve it both recursively and iteratively.
confused what "{1,#,2,3}" means? > read more on how binary tree is serialized on OJ.

OJ's Binary Tree Serialization:The serialization of a binary tree follows a level order traversal, where '#' signifies a path terminator where no node exists below.
Here's an example:
```
   1
  / \
 2   3
    /
   4
    \
     5
```
The above binary tree is serialized as "{1,2,3,#,#,4,#,#,5}".


直接贴代码吧，这题我思考甚少，看的Discuss

```
public class Solution {
	
	//如果想用广度优先搜索的话，这题得绑定左右2个节点加入到队列中
	
	//DFS
	public static boolean isSymmetric(TreeNode root){
		if(root==null) return true;
		
		Queue<TreeNode> p = new LinkedList<TreeNode>();
		Queue<TreeNode> q = new LinkedList<TreeNode>();
		
		p.add(root);
		q.add(root);
		
		while(!q.isEmpty()&&!p.isEmpty()){
			int sizeq = q.size();
			int sizep = p.size();
			
			if(sizep != sizeq)
				return false;
			
			for(int i=0;i<sizep;i++){
				TreeNode np = p.poll();
				TreeNode nq = q.poll();
				
				if(np==null&&nq==null) continue;
				if(np==null||nq==null) return false;
				if(np.val!=nq.val) return false;
				p.add(np.left);
				p.add(np.right);
				q.add(nq.right);
				q.add(nq.left);
			}
			
		}
		
		return true;
	}
	
	/*递归
    public static boolean isSymmetric(TreeNode root) {
        return isSymmetric(root,root);
    }

    public static boolean isSymmetric(TreeNode p, TreeNode q){
        if(p==null && q==null) return true;
        if(p==null || q==null) return false;
    
        return p.val ==q.val&&isSymmetric(p.left,q.right)&&isSymmetric(p.right,q.left);
    }*/
    
}
```
源代码，出门右转：[github/LeetCode](https://github.com/ldc4/LeetCode)
<!-- 信心被打击了一点点 -->