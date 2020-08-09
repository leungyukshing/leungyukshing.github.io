/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int pathSum(TreeNode* root, int sum) {
        vector<TreeNode*> path;
        int result = 0;
        int currentSum = 0;
        
        helper(root, path, currentSum, sum, result);
        return result;
    }
    
    void helper(TreeNode* node, vector<TreeNode*> path, int currentSum, const int sum, int& result) {
        if (!node) {
            return;
        }
        
        currentSum += node->val;
        path.push_back(node);
        
        int size = path.size();
        int temp = currentSum;
        
        if (temp == sum) {
            result++;
        }
        for (int i = 0; i < size - 1; i++) {
            temp -= path[i]->val;
            if (temp == sum) {
                result++;
            }
        }
        
        helper(node->left, path, currentSum, sum, result);
        helper(node->right, path, currentSum, sum, result);
        path.pop_back();
    }
};
