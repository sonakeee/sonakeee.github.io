---
defaults:
- scope:
  path: ""
  type: posts
  values:
  layout: single
  author_profile: true
  comments: true
  related: true

title: "[Programmers] 길 찾기 게임"
excerpt: "[Programmers] 길 찾기 게임"
toc: true
toc_sticky: true
toc_label: "목차"
categories:
  - Coding Test 
tags:
  - [Coding Test, Programmers, 이진 트리, 전위 순회, 후위 순회]
date: 2021-10-10
last_modified_at: 2021-10-10
---
# [Programmers] 길 찾기 게임

Problem URL : [길 찾기 게임](https://programmers.co.kr/learn/courses/30/lessons/42892)

```cpp
#include <vector>
#include <algorithm>

using namespace std;

struct Node{
    int x;
    int y;
    int idx;
    Node *left;
    Node *right;
};

bool cmp(Node a, Node b){
    if(a.y == b.y) return a.x < b.x;
    return a.y > b.y;
}

void makeTree(Node *parent, Node *child){
    Node * next;
    if(child->x < parent->x) {
        if(parent->left == NULL)
            parent->left = child;
        else
            makeTree(parent->left, child);
    }else{
        if(parent->right == NULL)
            parent->right = child;
        else
            makeTree(parent->right, child);
    }
}

void preorder(vector<int> &answer, Node *node){
    if(node == NULL) return;
    answer.push_back(node->idx);
    preorder(answer, node->left);
    preorder(answer, node->right);
}

void postorder(vector<int> &answer, Node *node){
    if(node == NULL) return;
    postorder(answer, node->left);
    postorder(answer, node->right);
    answer.push_back(node->idx);
}

vector<vector<int>> solution(vector<vector<int>> nodeinfo){
    vector<vector<int>> answer = { {}, {} };
    vector<Node> nodes;
    Node *root;
    
    for(int i = 0; i < nodeinfo.size(); i++){
        Node node;
        node.x = nodeinfo[i][0];
        node.y = nodeinfo[i][1];
        node.idx = i + 1;
        nodes.push_back(node);
    }
    
    sort(nodes.begin(), nodes.end(), cmp); // y기준 내림차순, x기준 오름차순으로 정렬
    
    root = &nodes[0];
    
    for(int i = 1; i < nodes.size(); i++)
        makeTree(root, &nodes[i]);
    
    preorder(answer[0], root);
    postorder(answer[1], root);
    
    return answer;
}
```