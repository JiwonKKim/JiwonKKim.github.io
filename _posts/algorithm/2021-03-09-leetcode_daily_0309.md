---
layout: post
title:  "[LeetCode] Add One Row to Tree"
category: Algorithm
tags: [Algorithm, LeetCode]
comments: true  
---

#### [LeetCode] Daily 문제
##### [Add One Row to Tree](https://leetcode.com/problems/add-one-row-to-tree/)

Given the root of a binary tree, then value v and depth d, you need to add a row of nodes with value v at the given depth d. The root node is at depth 1.

The adding rule is: given a positive integer depth d, for each NOT null tree nodes N in depth d-1, create two tree nodes with value v as N's left subtree root and right subtree root. And N's original left subtree should be the left subtree of the new left subtree root, its original right subtree should be the right subtree of the new right subtree root. If depth d is 1 that means there is no depth d-1 at all, then create a tree node with value v as the new root of the whole original tree, and the original tree is the new root's left subtree.

Example 1:
Input: 
A binary tree as following:
       4
     /   \
    2     6
   / \   / 
  3   1 5   

v = 1

d = 2

Output: 
       4
      / \
     1   1
    /     \
   2       6
  / \     / 
 3   1   5   

Example 2:
Input: 
A binary tree as following:
      4
     /   
    2    
   / \   
  3   1    

v = 1

d = 3

Output: 
      4
     /   
    2
   / \    
  1   1
 /     \  
3       1
Note:
1. The given d is in range [1, maximum depth of the given tree + 1].
2. The given binary tree has at least one tree node.

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

using namespace std;

class Solution {
public:
    TreeNode* addOneRow(TreeNode* root, int v, int d) {
        queue<TreeNode*> nodes{{root}};
        int curr_depth = 1;
        // ** Case when the target depth is 1
        if (d == 1) {
            TreeNode *newNode = new TreeNode(v);
            newNode->left = root;
            
            return newNode;
        }

        // ** Level Order Search
        while (!nodes.empty()) {
            if (curr_depth == d - 1) {
                TreeNode *node = nodes.front();
                nodes.pop();
                
                insertNode(node, v);
                
                continue;
            }

            int length = nodes.size();
            for (int i = 0; i < length; i++) {
                TreeNode* node = nodes.front();
                nodes.pop();
                if (node->left != NULL) {
                    nodes.push(node->left);
                }
                if (node->right != NULL) {
                    nodes.push(node->right);
                }
            }
            curr_depth++;
        }
        return root;
    }
    
    void insertNode(TreeNode *node, int v) {
        TreeNode *newNode_l = new TreeNode(v);
        TreeNode *newNode_r = new TreeNode(v);

        newNode_l->left  = node->left;
        newNode_r->right = node->right;

        node->left  = newNode_l;
        node->right = newNode_r;
    }
    
};

```

오랜만에 C++ 하다보니 #include를 #define으로 썼다. 컴파일 에러를 보고 소스라치게 놀랐다. 이걸 까먹네

C++ 문법 너무 헷갈린다 봐도봐도 모르겠다

Tree의 Node Address를 Queue에 넣어 Level Order Search를 한다. Target Depth를 찾으면 Node를 Insert하고, Target Depth에서의 Node Insert가 완료되면 코드를 종료한다.

##### 출처:
- [Add One Row to Tree](https://leetcode.com/problems/add-one-row-to-tree/)
