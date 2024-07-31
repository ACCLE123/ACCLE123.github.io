---
title: lru
date: 2024-07-31 20:58:22
tags:
---

leetcode lru 也是 莉莉丝的笔试题

### 正解

```c++
class LRUCache {
public:
    int n;
    struct Node {
        int key, val;
        Node *l, *r;
        Node(int _key, int _val): key(_key), val(_val), l(nullptr), r(nullptr) {}
    };
    Node *head, *tail;
    unordered_map<int, Node*> M;

    void init() {
        head = new Node(-1, -1);
        tail = new Node(-1, -1);
        head->r = tail;
        tail->l = head;
    }

    void insert(Node* node) {
        head->r->l = node;
        node->r = head->r;
        head->r = node;
        node->l = head;
    }

    void remove(Node* node) {
        node->l->r = node->r;
        node->r->l = node->l;
    }

    LRUCache(int capacity) {
        n = capacity;
        init();
    }
    
    int get(int key) {
        if (!M.count(key)) return -1;
        else {
            auto node = M[key];
            remove(node);
            insert(node);
            return node->val;
        }
    }
    
    void put(int key, int value) {
        Node* node;
        if (!M.count(key)) {
            node = new Node(key, value);
            if (M.size() == n) {
                auto last = tail->l;
                // cout << last->key << ' ' << last->val << endl;
                remove(last);
                M.erase(last->key);
            } 
            M[key] = node;
            insert(node);
        } else {
            node = M[key];
            node->val = value;
            remove(node);
            insert(node);
        }
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

### bug

```c++

class LRUCache {
public:
    int n;
    struct Node {
        int key, val;
        Node *l, *r;
        Node(int _key, int _val): key(_key), val(_val), l(nullptr), r(nullptr) {}
    };
    Node *head, *tail;
    unordered_map<int, Node*> M;

    void init() {
        head = new Node(-1, -1);
        tail = new Node(-1, -1);
        head->r = tail;
        tail->l = head;
    }

    void insert(Node* node) {
        head->r->l = node;
        node->r = head->r;
        head->r = node;
        node->l = head;
    }

    void remove(Node* node) {
        node->l->r = node->r;
        node->r->l = node->l;
    }

    LRUCache(int capacity) {
        n = capacity;
        init();
    }
    
    int get(int key) {
        if (!M.count(key)) return -1;
        else {
            auto& node = M[key];
            remove(node);
            insert(node);
            return node->val;
        }
    }
    
    void put(int key, int value) {
        Node* node;
        if (!M.count(key)) {
            node = new Node(key, value);
            if (M.size() == n) {
                auto& last = tail->l;
                // cout << last->key << ' ' << last->val << endl;
                remove(last);
                M.erase(last->key);
            } 
            M[key] = node;
            insert(node);
        } else {
            node = M[key];
            node->val = value;
            remove(node);
            insert(node);
        }
    }
};
```

### auto&

目前还在思考原因...

