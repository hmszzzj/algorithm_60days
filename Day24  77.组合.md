## 代码随想录算法训练营第十四天 | 二叉树的递归遍历、迭代遍历、迭代统一
### LeetCode 144 二叉树的前序遍历
#### 题目链接
    https://leetcode.cn/problems/binary-tree-preorder-traversal/
#### 思路
    递归三步曲
    - 确定传参和返回值
      - 传参 ： 当前节点cur，以及存结果的vector
      - 返回值 ： 输出val，将其写入vector中，则需要将vector传入，不需要返回值
    - 确定递归结束条件
      - cur为nullptr时直接返回
    - 单层处理逻辑
      - 遍历顺序 ： 前序遍历
      - 将当前节点val保存至vec即可
#### 代码实现
~~~cpp
class Solution {
public:
    /*
    递归三部曲：
    1、确定递归函数的参数和返回值
    2、确定终止条件
    3、确定单层递归的逻辑
    */
    // 1、确定递归的参数和返回值
    // 输出val，将其写入vector中，则需要将vector传入，不需要返回值
    void traversal(TreeNode* cur, vector<int>& vec) {
        // 2、确定终止条件
        if (cur == nullptr) return;

        // 3、确定单层递归的逻辑
        vec.push_back(cur->val);
        traversal(cur->left, vec);
        traversal(cur->right, vec);
    }

    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        traversal(root, res);
        return res;
    }
};
~~~

#### 代码实现过程中遇到的问题
    无

### LeetCode 94 二叉树的后序遍历
#### 题目链接
https://leetcode.cn/problems/binary-tree-postorder-traversal/
#### 思路
    迭代法，利用栈将当前节点压入栈中，弹出处理即可
#### 代码实现
~~~cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if (node->left) st.push(node->left); 
            if (node->right) st.push(node->right); 
        }
        reverse(result.begin(), result.end()); // 将结果反转之后就是左右中的顺序了
        return result;
    }
};
~~~
#### 代码实现过程中遇到的问题
    无

### LeetCode 94 二叉树的后序遍历
#### 题目链接
https://leetcode.cn/problems/binary-tree-inorder-traversal/
#### 思路
    中序遍历中，当前遍历和处理的不统一，导致中序遍历的写法与前/后序遍历不统一，可以添加标记位，使三者统一。
#### 代码实现
~~~cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            if (node != NULL) {
                st.pop(); // 将该节点弹出，避免重复操作，下面再将右中左节点添加到栈中
                if (node->right) st.push(node->right);  // 添加右节点（空节点不入栈）

                st.push(node);                          // 添加中节点
                st.push(NULL); // 中节点访问过，但是还没有处理，加入空节点做为标记。

                if (node->left) st.push(node->left);    // 添加左节点（空节点不入栈）
            } else { // 只有遇到空节点的时候，才将下一个节点放进结果集
                st.pop();           // 将空节点弹出
                node = st.top();    // 重新取出栈中元素
                st.pop();
                result.push_back(node->val); // 加入到结果集
            }
        }
        return result;
    }
};
~~~
