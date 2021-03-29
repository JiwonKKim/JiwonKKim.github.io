---
layout: post
title:  "[LeetCode] Flip Binary Tree To Match Preorder Traversal"
category: Algorithm
tags: [Algorithm, LeetCode]
comments: true  
---

#### [LeetCode] Daily 문제
##### [Flip Binary Tree To Match Preorder Traversal](https://leetcode.com/problems/flip-binary-tree-to-match-preorder-traversal/)

You are given the root of a binary tree with n nodes, where each node is uniquely assigned a value from 1 to n. You are also given a sequence of n values voyage, which is the desired pre-order traversal of the binary tree.

Any node in the binary tree can be flipped by swapping its left and right subtrees. For example, flipping node 1 will have the following effect:

Flip the smallest number of nodes so that the pre-order traversal of the tree matches voyage.

Return a list of the values of all flipped nodes. You may return the answer in any order. If it is impossible to flip the nodes in the tree to make the pre-order traversal match voyage, return the list [-1].

***문제에 대한 자세한 설명은 출처를 참조***<br>

Example 1:


Input: root = [1,2], voyage = [2,1]
Output: [-1]
Explanation: It is impossible to flip the nodes such that the pre-order traversal matches voyage.
Example 2:

Input: root = [1,2,3], voyage = [1,3,2]
Output: [1]
Explanation: Flipping node 1 swaps nodes 2 and 3, so the pre-order traversal matches voyage.
Example 3:

Input: root = [1,2,3], voyage = [1,2,3]
Output: []
Explanation: The tree's pre-order traversal already matches voyage, so no nodes need to be flipped.
 
Constraints:

- The number of nodes in the tree is n.
- n == voyage.length
- 1 <= n <= 100
- 1 <= Node.val, voyage[i] <= n
- All the values in the tree are unique.
- All the values in voyage are unique.


``` cpp
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
private: 
    int voyage_index = 0;
    bool is_available = true;
public:
    vector<int> flipMatchVoyage(TreeNode* root, vector<int>& voyage) {
        vector<int> flipped_nodes;
        
        DFS(root, voyage, flipped_nodes);
        
        if (is_available) {
            return flipped_nodes;
        }
        else {
            return vector<int> {-1};
        }
        
    }
    
    bool DFS(TreeNode *node, vector<int> & voyage, vector<int>& flipped_nodes) {        
        
        if (node == NULL) {
            return false;
        }
        
        if (node->val != voyage[voyage_index]) {
            is_available = false;
            return false;
        }

        voyage_index++;
        
        if ((node->left != NULL) && (node->left->val != voyage[voyage_index])) {
            flipped_nodes.push_back(node->val);
            DFS(node->right, voyage, flipped_nodes);
            DFS(node->left, voyage, flipped_nodes);
        }
        else {
            DFS(node->left, voyage, flipped_nodes);
            DFS(node->right, voyage, flipped_nodes);
        }
        
        return true;
    }
};
```

문제에 Flip이라는 표현이 있어서 헷갈렸다. Flip이면 뒤집는 개념이라서 Flip된 노드의 Child Nodes도 같이 Flip되는 건줄 알았는데 그렇진 않았다. 예시도 든 그림도 이상하다. Input이 왼쪽 노드부터 Levelorder Traversal하게 채워지는 거면 첫 번째 그림처럼 3의 Child Nodes로 4와 5가 있으면 안될텐데.. 아무튼 문제에 대한 설명이 안나와있어서 결국 짜증나서 답을 봤다. DFS 문제이고, Child Node값이 Voyage와 다르면 Preorder Traversal이 아닌 Postorder Traversal로 값을 찾는다. 코드가 예쁘진 않다.

##### 출처:
- [Flip Binary Tree To Match Preorder Traversal](https://leetcode.com/problems/flip-binary-tree-to-match-preorder-traversal/)
