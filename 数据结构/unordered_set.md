## 1 . 容器定位
- 头文件：`<unordered_set>`
- 命名空间：`std`
- 本质：基于哈希表实现的无序唯一集合（数学意义上的 set）。
- C++ 标准：C++11 引入，后续标准仅做微小增补。

---

## 2 . 模板原型

```cpp
template<
    class Key,
    class Hash    = std::hash<Key>,
    class KeyEq   = std::equal_to<Key>,
    class Alloc   = std::allocator<Key>
> class unordered_set;
```

- Key：元素类型（键即值）。
- Hash：哈希函数对象，默认使用 `std::hash<Key>`。
- KeyEq：键相等判定，默认使用 `std::equal_to<Key>`（即 `operator==`）。
- Alloc：分配器，默认使用 `std::allocator<Key>`。

---

## 3 . 复杂度

|操作|平均|最坏|说明|
|--|--|--|--|
|插入/删除|O (1)|O (n)|最坏发生在大量冲突|
|查找|O (1)|O (n)|同上|
|遍历|O (n)|O (n)|与元素个数线性相关|

---

## 4 . 构造与初始化

```cpp
#include <unordered_set>
#include <string>

std::unordered_set<int> s1;                          // 空集合
std::unordered_set<int> s2 = {1, 2, 3, 4};           // 列表初始化
std::unordered_set<int> s3(s2);                      // 拷贝构造
std::unordered_set<int> s4(s2.begin(), s2.end());    // 迭代器区间

// 自定义哈希与相等函数
struct MyHash {
    size_t operator()(const std::string& key) const {
        return std::hash<std::string>()(key) ^ 0x9e3779b9;
    }
};
struct MyEq {
    bool operator()(const std::string& a, const std::string& b) const {
        return a.size() == b.size();  // 仅长度相等即视为相等（示例用）
    }
};
std::unordered_set<std::string, MyHash, MyEq> s5;
```

---

## 5 . 常用成员函数速查

| 类别   | 函数                                                                            | 说明                         |     |
| ---- | ----------------------------------------------------------------------------- | -------------------------- | --- |
| 容量   | `empty()` / `size()` / `max_size()`                                           | 判空 / 元素个数 / 最大元素数          |     |
| 迭代器  | `begin()` / `end()` / `cbegin()` / `cend()`	|正向迭代器                          |     |
| 元素访问 | `find(key)`                                                                   | 返回迭代器，找不到返回 `end()`        |     |
| 修改   | `insert(value)` / `emplace(args...)` / `erase(it)` / `erase(key)` / `clear()` | 插入 / 就地构造 / 删除 / 清空        |     |
| 桶接口  | `bucket_count()` / `bucket_size(n)` / `bucket(key)`                           | 桶数量 / 第 n 个桶大小 / key 所在桶索引 |     |
| 负载因子 | `load_factor()` / `max_load_factor()`                                         | 当前负载因子 / 设置最大负载因子          |     |
| 重新哈希 | `rehash(n)` / `reserve(n)`                                                    | 手动重哈希 / 预留空间               |     |

---
## 6 . 代码示例合集

### 6 .1 基本增删查

```cpp
#include <unordered_set>
#include <iostream>

int main() {
    std::unordered_set<int> us;

    // 插入
    auto [it, inserted] = us.insert(42);  // C++17 结构化绑定
    std::cout << "42 inserted? " << inserted << '\n';

    us.insert({10, 20, 30});              // 批量插入 initializer_list
    us.emplace(100);                      // 就地构造

    // 查找
    if (auto pos = us.find(30); pos != us.end()) {
        std::cout << "Found 30\n";
    }

    // 删除
    us.erase(10);                         // 按键删除
    for (int v : us) std::cout << v << ' ';
}
```

### 6 .2 自定义类型 + 自定义哈希

```cpp
#include <unordered_set>
#include <string>
#include <functional>

struct Point {
    int x, y;
    bool operator==(const Point& p) const { return x == p.x && y == p.y; }
};

// 为 Point 实现 std::hash 特化
namespace std {
    template<>
    struct hash<Point> {
        size_t operator()(const Point& p) const noexcept {
            return hash<int>()(p.x) ^ (hash<int>()(p.y) << 1);
        }
    };
}

int main() {
    std::unordered_set<Point> ps;
    ps.insert({1, 2});
    ps.insert({3, 4});

    for (auto [x, y] : ps) std::cout << "(" << x << "," << y << ") ";
}
```

### 6 .3 观察桶分布

```cpp
#include <unordered_set>
#include <iostream>

int main() {
    std::unordered_set<int> us = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    std::cout << "bucket_count = " << us.bucket_count() << '\n';
    std::cout << "load_factor  = " << us.load_factor() << '\n';

    for (size_t i = 0; i < us.bucket_count(); ++i) {
        std::cout << "bucket[" << i << "]: ";
        for (auto it = us.begin(i); it != us.end(i); ++it)
            std::cout << *it << ' ';
        std::cout << '\n';
    }
}
```

### 6 .4 使用 `reserve` 避免多次重哈希

```cpp
std::unordered_set<int> us;
us.reserve(10000);  // 预先分配桶，减少后续插入时的 rehash 次数
for (int i = 0; i < 10000; ++i) us.insert(i);
```

---
## 7 . 迭代器失效规则
- 插入：不会使任何迭代器失效。
- 删除：仅被删除元素的迭代器失效，其余保持有效。
- 重哈希（`rehash` / `reserve` / 自动扩容）：所有迭代器失效。

---

## 8 . 与 `std::set` 的抉择

|场景|推荐|
|--|--|
|需要有序遍历或有序区间查询	| `std::set`	|
|仅需要快速存在性检查、去重	| `unordered_set`	|
|自定义类型且哈希不好写|如果排序可行，用 `set` 更简单	|

---

## 9 . 常见陷阱
- 哈希冲突过多：自定义类型哈希函数设计不佳导致退化到 O (n)。
- 相等函数不一致：`operator==` 与哈希函数不满足“相等对象哈希值相同”原则。
- 误用 `[]` 运算符：`unordered_set` 没有 `operator[]`，那是 `unordered_map` 的专属。

---

10. 小结口诀

	无序去重哈希表，插入查找常数好；
	若要顺序红黑树，set 才是你依靠。