## 16.1 给出实例的定义
>当编译器实例化一个模板时，它使用实际的模板参数代替对应的模板参数来创建出模板的一个新"实例"

## 16.2 编写并测试自己的compare函数

```
#include <iostream>
#include <vector>
using namespace std;


template <typename T>
int compare(const T& lhs, const T& rhs) {
  if (lhs < rhs) return -1;
  if (lhs > rhs) return 1;
  return 0;
}

int main() {
  cout << compare(1, 0) << endl;
  vector<int> vec1{1, 2, 3}, vec2{4, 5, 6};
  cout << compare(vec1, vec2) << endl;
  return 0;
}
```
## 16.3 对两个Sales_data对象调用你的compare函数，观察编译器在实例话过程中如果处理错误
```
#include <iostream>
#include <vector>
using namespace std;

class Sales_data {};

template <typename T>
int compare(const T& lhs, const T& rhs) {
  if (lhs < rhs) return -1;
  if (lhs > rhs) return 1;
  return 0;
}

int main() {
  Sales_data data1, data2;
  cout << compare(data1, data2) << endl;
  return 0;
}
```

- 会出现errorno match for ‘operator<’ (operand types are ‘const Sales_data’ and ‘const Sales_data’)

## 16.4 编写行为类似标准库find算法的模板，函数需要两个模板类的参数，一个表示函数的迭代器参数，另一个表示值的类型。使用你的函数在一个vector<int>和list<string>中查找给定值
- use c++14 compile

```
#include <iostream>
#include <list>
#include <vector>
#include <string>
using namespace std;

namespace ch16 {
template <typename Iterator, typename Value>
auto find(Iterator first, Iterator last, Value const& value) {
  for (; first != last && *first != value; ++first) {
    ;
  }
  return first;
}
}  // namespace ch16

int main() {
  std::vector<int> v = {1, 2, 3, 4, 5};
  auto is_in_vector = v.cend() != ch16::find(v.cbegin(), v.cend(), 5);
  std::list<std::string> l = {"aa", "bb", "cc", "dd", "ee", "ff", "gg"};
  auto is_in_list = l.cend() != ch16::find(l.cbegin(), l.cend(), "zz");
  std::cout << (is_in_vector ? "found\n" : "not found\n");
  std::cout << (is_in_list ? "found\n" : "not found\n");
  return 0;
}
```

## 16.5 编写一个print函数模板，它能接受一个数组的引用，能处理任意大小、任意元素类型的数组
 
```
#include <iostream>
#include <string>
#include <vector>

template <typename Arr>
void print(Arr const& arr) {
  for (auto const& elem : arr) {
    std::cout << elem << std::endl;
  }
}

int main() {
  std::string s[] = {"ssss", "aaa", "bb"};
  char c[] = {'a', 'b', 'c'};
  std::vector<int> v = {1, 2, 3, 4, 5};
  int i[] = {1};
  print(i);
  print(c);
  print(v);
  print(s);
  return 0;
}


```

## 16.6 你认为接受一个数组实参的标准库函数begin和end是如何工作的？ 定义你自己版本的begin和end

```
#include <iostream>
#include <string>
#include <vector>

template <typename T, unsigned size>
T* begin_def(T (&arr)[size]) {
  return arr;
}

template <typename T, unsigned size>
T* end_def(T (&arr)[size]) {
  return arr + size;
}
int main() {
  std::string s[] = {"sss", "ss", "ssssz"};
  std::cout << *(begin_def(s) + 1) << std::endl;
  std::cout << *(end_def(s) - 1) << std::endl;
  return 0;
}
```

## 16.7 编写一个constexpr模板，返回给定数组的大小
 
```
#include <iostream>
#include <string>

template <typename T, unsigned size>
constexpr unsigned getSize(const T (&arr)[size]) {
  return size;
}

int main() {
  std::string s[] = {"sss"};
  std::cout << getSize(s) << std::endl;  // 1

  char c[] = "s";
  std::cout << getSize(c) << std::endl;  // 2

  return 0;
}
```

## 16.8 c++程序员为什么喜欢使用!=而不喜欢使用<

- 因为大多数类只定义!=操作而没有定义<操作，使用!=可以降低对要处理的类型的要求


## 16.9 什么是函数模板，什么是类模板
- 一个函数模板就是一个公式，可以用来生成针对特定类型的函数版本
- 类模板是用来生成类的蓝图的，与函数模板的不同之处是，编译器不能为类模板推断模板参数类型

## 16.10 当一个类模板被实例化时，会发生什么？
- 一个类模板的每个实例都形成一个独立的类
## 16.11 下面List的定义是错误的，应该如何修正它
```
template<typename elemType> 
class ListItem;
template<typename elemType> 
class List {
 public:
  List<elemType>();
  List<elemType>(const List<elemType> &);
  List<elemType>& operator=(const List<elemType> &);
  ~List();
  void insert(ListItem *ptr, elemType value);  
 private:
  ListItem *front, *end;
};
```
- 模板需要模板参数，修改如下：
```

template <typename elemType>
class ListItem;
template <typename elemType>
class List {
 public:
  List<elemType>();
  List<elemType>(const List<elemType> &);
  List<elemType> &operator=(const List<elemType> &);
  ~List();
  void insert(ListItem<elemType> *ptr, elemType value);

 private:
  ListItem<elemType> *front, *end;
};
```
## 16.12 编写你自己版本的Blob和BlobPtr模板，包含书中未定义的多个const成员
- Blob class

```
template <typename T>
class Blob {
 public :
  typedef T value_type;
  typedef typename std::vector<T>::size_type size_type;

  Blob();
  Blob(std::initializer_list<T> il);

  size_type size() const { return data_->size(); }
  bool empty() const { data_.empty();}

  void push_back(const T& t) { data_->push_back(t); }
  void pop_back();

  T& back();
  T& operator[](size_type i);

  const T& back() const;
  const T& operator[](size_type i) const;

 private:
  std::shared_ptr<std::vector<T>> data_;
  void check(size_type i, const std::string& msg) const;
};

template <typename T>
Blob<T>::Blob() 
    : data(std::make_shared<std::vector<T>>()) {}

template <typename T>
Blob<T>::Blob(std::initializer_list<T> il)
    : data(std::make_shared<std::vector<T>>(il)) {}

template <typename T>
void Blob<T>::check(size_type i, const std::string& msg) const {
  if (i >= data->size()) {
    std::throw std::out_of_range(msgs);
  }
}

template <typename T>
void Blob<T>::pop_back() {
  check(0, "pop_back on empty Blob");
  data->pop_back();
}

template <typename T>
T& Blob<T>::back() {
  check(0, "back on empty Blob");
  return data->back();
}

template <typename T>
const T& Blob<T>::back() const {
  check(0, "back on emptu const Blob");
  return data->back();
}

template <typename T>
T& Blob<T>::operator[](size_type i) {
  check(i, "subscript out of range);
  return (*data)[i];
}

template <typename T>
const T& Blob<T>::operator[](size_type i) const {
  check(i, "const subscripe out of range);
  return (*data)[i];
}
```
- BlobPtr class
```
template <typename T>
bool operator==(const BlobPtr<T> &lhs, const BlobPtr<T> &rhs);
bool operator < (const BlobPtr<T> & lhs, const BlobPtr<T> &rhs);

template <typename T>
class BlobPtr {

 friend bool operator==<T>(const BlobPtr<T> &lhs, const BlobPtr<T> & rhs);
 friend bool operator < <T>(const BlobPtr<T> &lhs, const BlobPtr<T> & rhs); 

 public:
  BlobPtr() : curr(0) {}
  BlobPtr(Blob<T>& a, std::size_t sz = 0)
      : wptr(a.data), curr(sz) {} 
  
  T* operator*() const {
    auto p = check(curr, "dereference past end);
    return (*p)[curr];
  }

  BlobPtr& operator++();
  BlobPtr& operator--();

  BlobPtr& operator++(int);
  BlobPtr& operator--(int);

 private:
  std::shared_ptr<std::vector<T>> check(std::size_t, const std::string&) const; 
  std::weak_ptr<std::vector<T>> wptr;
  std::size_t curr;
};

template <typename T>
BlobPtr<T>& BlobPtr<T>::operator++() {
  check(curr, "increment pass end of StrBlob);
  ++curr;
  return *this;
} 

template <typename T>
BlobPtr<T>& BlobPtr<T>::operator--() {
  check(curr, "decrement pass begin of BlobPtr");
  --curr;
  return *this;
}

template <typename T>
BlobPtr<T>& BlobPtr<T>::operator++(int) {
  BlobPrt ret = *this;
  ++*this;
  return ret;
}

template <typname T>
BlobPtr<T>& BlobPtr<T>::operator--(int) {
  BlobPtr ret = *this;
  --*this;
  return ret;
}

template <typename T>
bool operator==(const BlobPtr<T> &lhs, const BlobPtr<T> &rhs) {
  if (lhs.wptr.lock() != rhs.wptr.lock()) {
    std::throw runtime_error("ptrs to different Blobs!);
  }
  return lhs.i = rhs.i;
}

template <typename T>
bool operator<(const BlobPtr<T> &lhs, const BlobPtr<T> & rhs) {
  if (lhs.wptr.lock() != rhs.wptr.lock()) {
    std::throw runtime_error("ptrs to different Blobs!);
  }
  return lhs.i < rhs.i;
}

```

## 16.13 解释你为BlobPtr的相等和关系运算符选择哪种类型的友好关系？



## 16.14 编写Screen类模板，用非类型参数定义Screeen的高和宽


## 16.15 为你的Screen模板实现输入和输出运算符，Screen类需要哪些友元(如果需要的话)
来令输入和输出运算符正确工作？ 解释每个友元声明(如果有的话)为什么是必要的

## 16.16 将StrVec类重写为模板，命名为Vec