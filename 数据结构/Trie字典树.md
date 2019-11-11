[TOC]

# Trie 字典树 前缀树

如果有n个条目，使用树结构，查询的时间复杂度是O(logn)

如果有100万个条目logn大约为20

Trie:

查询每个条目的时间复杂度和字典中一共有多少条目无关！

时间复杂度为O(W),W为查询单词的长度

## 什么是Trie

每个节点有若干个指向下个节点的指针

考虑不同的语言，不同情境（大小写，字符等）

```java
class Node{
    //访问到此，是否能形成一个单词
    boolean isWord;
    //可以用TreeMap(红黑树)
	Map<char,Node> next;
}
```

##  Trie的代码实现

```Java
package com.practice.tree;

import java.util.TreeMap;

public class Trie {
    private class Node{
        public boolean isWord;
        public TreeMap<Character,Node> next;
        public Node(boolean isWord){
            this.isWord = isWord;
            next = new TreeMap<>();
        }
        public Node(){
            this(false);
        }
    }

    private Node root;
    private int size;

    public Trie(){
        root = new Node();
        size = 0;
    }

    //获得Trie中存储的单词的数量
    public int getSize(){
        return size;
    }

    //向Trie中添加一个新的单词word
    public void add(String word){
        Node cur = root;
        for (int i = 0; i < word.length();i++){
            char c = word.charAt(i);
            if (cur.next.get(c) == null){
                cur.next.put(c,new Node());
            }
            cur = cur.next.get(c);
        }
        if (!cur.isWord){
            cur.isWord = true;
            size++;
        }
    }
    //递归实现Trie中添加一个新单词word
    public void recursionAdd(String word){
        recursionAdd(root,0,word);
    }
    private void recursionAdd(Node cur,int index, String word){
        char c = word.charAt(index);
        index++;
        if (cur.next.get(c) == null){
            cur.next.put(c,new Node());
        }
        cur = cur.next.get(c);
        if (index==word.length()){
            if (!cur.isWord){
                cur.isWord = true;
                size++;
                return;
            }
        }
        recursionAdd(cur,index,word);
    }
    //查找单词word是否在Trie中
    public boolean contains(String word){
        Node cur = root;
        for (int i = 0;i<word.length();i++){
            char c = word.charAt(i);
            if (cur.next.get(c) == null){
                return false;
            }
            cur = cur.next.get(c);
        }
        return cur.isWord;
    }
    //查询是否在Trie中有单词以prefix为前缀
    public boolean isPrefix(String prefix){
        Node cur = root;
        for(int i = 0; i < prefix.length();i++){
            char c = prefix.charAt(i);
            if (cur.next.get(c) == null)
                return false;
            cur = cur.next.get(c);
        }
        return true;
    }

}

```

