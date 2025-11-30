# 任务学习目标

## 本次任务需要掌握的知识点

1. **模板（Templates）**
   - 函数模板和类模板的使用
   - 模板特化和实例化
   - 在Trie中，`TrieNodeWithValue<T>`是一个模板类，`Get<T>`和`Put<T>`是模板方法

2. **多态（Polymorphism）**
   - 虚函数和虚函数表
   - 继承和多态的实现
   - `TrieNode`和`TrieNodeWithValue<T>`的继承关系，通过`Clone()`虚函数实现多态

3. **Trie树（前缀树）**
   - Trie树的基本概念和结构
   - 如何存储和检索键值对
   - Copy-on-write（写时复制）的实现方式

4. **智能指针（Smart Pointers）**
   - `std::shared_ptr`的使用：共享所有权，自动管理内存
   - `std::unique_ptr`的使用：独占所有权
   - 在Trie中，节点通过`shared_ptr`共享，实现copy-on-write

5. **std::move**
   - 移动语义和右值引用
   - 如何避免不必要的拷贝
   - 在Trie中，使用`std::move`传递值，特别是对于不可拷贝的类型（如`std::unique_ptr`）

## 题目背景

本次题目来源于卡内基梅隆大学公开课的课后习题，需要实现一个基于copy-on-write trie的键值存储系统。

### 核心要求

- **Get(key)**: 获取键对应的值
- **Put(key, value)**: 设置键对应的值，如果键已存在则覆盖。注意值可能是不可拷贝的类型（如`std::unique_ptr`）。返回新的trie。
- **Delete(key)**: 删除键对应的值。返回新的trie。

所有操作都不应该直接修改trie本身，而是创建新的trie节点并尽可能重用现有的节点。

### 实现要点

1. 使用`std::shared_ptr`共享未修改的节点
2. 使用`Clone()`方法创建需要修改的节点副本
3. 删除操作后需要清理不必要的空节点
4. 空trie用`nullptr`表示
5. 不要存储C字符串终止符`\0`

