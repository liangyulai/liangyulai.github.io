---
layout:     post
title:      "Boost Multi-index Containers Library"
subtitle:   "Boost Multi-index Containers Library"
date:       2018-12-13 19:00
author:     "Liang Yulai"
header-img: "img/post-bg-2018.jpg"
tags:
    - boost
    - c++
    - MultiIndex
---

### overview
* introduced in Boost 1.32, released in 2004
* 一个用户自定义的容器，用户可以对其中的数据提供多组访问接口
* 容器定义由 boost::multi_index::multi_index_container 来实现
* 接口的定义由模板类 boost::multi_index::indexed_by 来实现
* 
 
### Why we need multi-index container?


```c++
struct employee
{
  int         id;
  std::string name;
  employee(int id,const std::string& name):id(id),name(name){}
  bool operator<(const employee& e)const{return id<e.id;}
};

std::set<employee> employees;

```
#### 如何实现以 姓名（name）的字母序输出 employees ？
* Copy the employee set into a vector or similar and sort this by a comparison functor dependent upon employee::name.
* Keep a secondary data structure of pointers to the elements of the set, appropriately sorted by name. 

#### 用 boost::multi_index 如何实现？

```c++
// define a multiply indexed set with indices by id and name
useing employee_set = multi_index_container<
  employee,
  indexed_by<
    // sort by employee::operator<
    ordered_unique<identity<employee> >,
    
    // sort by less<string> on name
    ordered_non_unique<member<employee,std::string,&employee::name> >
  > 
>;

void print_out_by_name(const employee_set& es)
{
  // get a view to index #1 (name)
  const employee_set::nth_index<1>::type& name_index=es.get<1>();
  // use name_index as a regular std::set
  std::copy(
    name_index.begin(), name_index.end(),
    std::ostream_iterator<employee>(std::cout));
}
```


### 

```c++
#include <boost/multi_index_container.hpp>
#include <boost/multi_index/ordered_index.hpp>
#include <boost/multi_index/random_access_index.hpp>
#include <boost/multi_index/sequenced_index.hpp>
#include <boost/multi_index/identity.hpp>
#include <boost/multi_index/member.hpp>
#include <iostream>

using namespace boost::multi_index;

struct employee
{
  int         id;
  std::string name;
  employee(int id,const std::string& name):id(id),name(name){}
  bool operator<(const employee& e)const{return id<e.id;}
};

using employee_set = multi_index_container<
    employee,
    indexed_by<
        // sort by employee::operator<
        ordered_unique<identity<employee> >,
        // sort by less<string> on name
        ordered_non_unique<member<employee, std::string, &employee::name> >,

        // sequenced<>,
        random_access<>
    >
>;

int main() {
    employee_set es;
    es.insert({112, "Charles"});
    es.insert({142, "Daniel"});
    es.insert({127, "Edward"});
    es.insert({123, "Alexander"});
    es.insert({122, "Bernard"});
    es.insert({23, "Frank"});
    es.emplace(110, "Henry");
    // es.push_back({100, "James"});
    
    // Henry, James, Kenneth, Leonard, Melvin, Norman

    const employee_set::nth_index<0>::type& id_index=es.get<0>();
    const employee_set::nth_index<1>::type& name_index=es.get<1>();
    const employee_set::nth_index<2>::type& random_index=es.get<2>();

    std::cout << "Print the list by id:" << std::endl;
    for (auto e:id_index) {
        std::cout << e.id << ", " << e.name << std::endl;
    }

    std::cout << "Print the list by name:" << std::endl;
    for (auto e:name_index) {
        std::cout << e.id << ", " << e.name << std::endl;
    }

    std::cout << "Print the list randomly:" << std::endl;
    for (auto e:random_index) {
        std::cout << e.id << ", " << e.name << std::endl;
    }

    return 0;
}

```

####
```bash
kirin@kawagarbo:~/workspace/repo/test$ make multi_index_2 && ./multi_index_2
g++     multi_index_2.cpp   -o multi_index_2
Print the list by id:
23, Frank
110, Henry
112, Charles
122, Bernard
123, Alexander
127, Edward
142, Daniel
Print the list by name:
123, Alexander
122, Bernard
112, Charles
142, Daniel
127, Edward
23, Frank
110, Henry
Print the list randomly:
112, Charles
142, Daniel
127, Edward
123, Alexander
122, Bernard
23, Frank
110, Henry
```


### reference
[boost:multi-index](https://www.boost.org/doc/libs/1_69_0/libs/multi_index/doc/index.html)
[boost:multi-index example](https://www.boost.org/doc/libs/1_69_0/libs/multi_index/example/basic.cpp)
[Why you should use Boost.MultiIndex (Part I)](http://david-grs.github.io/why_boost_multi_index_container-part1/)
[Why you should use Boost.MultiIndex (Part II)](http://david-grs.github.io/why_boost_multi_index_container-part2/)
[The Boost C++ Libraries](https://theboostcpplibraries.com/boost.multiindex)
[Boost C++ 库](http://zh.highscore.de/cpp/boost/containers.html#containers_multiindex)