## 16.1 ����ʵ���Ķ���
>��������ʵ����һ��ģ��ʱ����ʹ��ʵ�ʵ�ģ����������Ӧ��ģ�������������ģ���һ����"ʵ��"

## 16.2 ��д�������Լ���compare����

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
## 16.3 ������Sales_data����������compare�������۲��������ʵ��������������������
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

- �����errorno match for ��operator<�� (operand types are ��const Sales_data�� and ��const Sales_data��)

## 16.4 ��д��Ϊ���Ʊ�׼��find�㷨��ģ�壬������Ҫ����ģ����Ĳ�����һ����ʾ�����ĵ�������������һ����ʾֵ�����͡�ʹ����ĺ�����һ��vector<int>��list<string>�в��Ҹ���ֵ
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

## 16.5 ��дһ��print����ģ�壬���ܽ���һ����������ã��ܴ��������С������Ԫ�����͵�����
 
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

## 16.6 ����Ϊ����һ������ʵ�εı�׼�⺯��begin��end����ι����ģ� �������Լ��汾��begin��end

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

## 16.7 ��дһ��constexprģ�壬���ظ�������Ĵ�С
 
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

## 16.8 c++����ԱΪʲôϲ��ʹ��!=����ϲ��ʹ��<

- ��Ϊ�������ֻ����!=������û�ж���<������ʹ��!=���Խ��Ͷ�Ҫ��������͵�Ҫ��


## 16.9 ʲô�Ǻ���ģ�壬ʲô����ģ��
- һ������ģ�����һ����ʽ������������������ض����͵ĺ����汾
- ��ģ�����������������ͼ�ģ��뺯��ģ��Ĳ�֮ͬ���ǣ�����������Ϊ��ģ���ƶ�ģ���������

## 16.10 ��һ����ģ�屻ʵ����ʱ���ᷢ��ʲô��
- һ����ģ���ÿ��ʵ�����γ�һ����������
## 16.11 ����List�Ķ����Ǵ���ģ�Ӧ�����������
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
- ģ����Ҫģ��������޸����£�
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
## 16.12 ��д���Լ��汾��Blob��BlobPtrģ�壬��������δ����Ķ��const��Ա
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

## 16.13 ������ΪBlobPtr����Ⱥ͹�ϵ�����ѡ���������͵��Ѻù�ϵ��



## 16.14 ��дScreen��ģ�壬�÷����Ͳ�������Screeen�ĸߺͿ�


## 16.15 Ϊ���Screenģ��ʵ�����������������Screen����Ҫ��Щ��Ԫ(�����Ҫ�Ļ�)
�������������������ȷ������ ����ÿ����Ԫ����(����еĻ�)Ϊʲô�Ǳ�Ҫ��

## 16.16 ��StrVec����дΪģ�壬����ΪVec