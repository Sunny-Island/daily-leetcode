```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        Stack<TreeNode> S = new Stack<>();
        if(preorder == null || preorder.length == 0)return null;

        TreeNode root = new TreeNode(preorder[0]);
        int index = 0;
        S.push(root);
        for(int i = 1;i<preorder.length;i++) {
            TreeNode tmpNode = new TreeNode(preorder[i]);
            if(inorder[index] == S.peek().val) {
                TreeNode tmpRoot = new TreeNode(-1);
                while(!S.empty() && S.peek().val == inorder[index]) {
                    index ++;
                    tmpRoot = S.pop();
                }
                tmpRoot.right = tmpNode;
            } else {
                TreeNode tmpRoot = S.peek();
                tmpRoot.left = tmpNode;
            }
            S.push(tmpNode);
        }
        return root;
    }
}
```
常规重建二叉树是使用递归的方法，找到根节点把两个数组分开重建子树。还有一种更为优雅的复杂度同样为On的迭代方法，借助一个栈和一个指针完成重建。

我们用一个栈 stack 来维护「当前节点的所有还没有考虑过右儿子的祖先节点」，栈顶就是当前节点。也就是说，只有在栈中的节点才可能连接一个新的右儿子。同时，我们用一个指针 index 指向中序遍历的某个位置，初始值为 0。index 对应的节点是「当前节点不断往左走达到的最终节点」，

我们用一个栈和一个指针辅助进行二叉树的构造。初始时栈中存放了根节点（前序遍历的第一个节点），指针指向中序遍历的第一个节点；

我们依次枚举前序遍历中除了第一个节点以外的每个节点。如果 index 恰好指向栈顶节点，那么我们不断地弹出栈顶节点并向右移动 index，并将当前节点作为最后一个弹出的节点的右儿子；如果 index 和栈顶节点不同，我们将当前节点作为栈顶节点的左儿子；

无论是哪一种情况，我们最后都将当前的节点入栈。