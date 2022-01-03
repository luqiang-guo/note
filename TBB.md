# TBB



## 任务调度器

任务调度器用来解决本地裸线程池(libgomp runtime)并行编程中常见的一些性问题。

- 超负荷

  正确的设定线程数是一个困难的工作（尤其是在有其他任务工作的的情况下），如果逻辑线程数少于可以使用的物理线程数那么就是低负荷，如果高于物理线程数就是超负荷。超负荷会导致线程通过时间片的方式来执行，从而来带一些切换开销。

  TBB为了解决上述问题采用了一种任务的方式。任务调度器让每个逻辑线程对应一个物理线程从而避免超负荷，每个任务可以看成是一个更轻量的线程，TBB调度这些任务在每个逻辑线程上。相比linux系统启动和结束一个任务要快18倍。

- 调度算法策略

  非实时操作系统（linux）采用的是公平调度思想的调度器，采用轮转的方式来分配时间片，它会使程序的性能变得更糟。

  TBB调度器采用非公平的方式，通过获取更多的信息动态的实现负载均衡（任务窃取的方式），从而提高执行效率。

- 负载不均衡

  基于线程的编程，需要处理负载均衡，这块又是非常麻烦的实现。TBB通过将程序拆分为许多小任务，调度器再将任务动态的分配到各个线程，保证了负载均衡。

## 内存分配

#### 内存对齐

为了提搞缓存命，采用64byte的内存对齐。（Cache Line 64byte）

- cache_aligned_allocator

  在讲述之前通过一个例子说明潜在的问题。线程A 和B各有一段内存空间a_array和b_array，假设a_array的结束位置和b_array起始位置在同一快速行中，当两个线程访问彼此临近的内存的时候，会在快存和操作系统上产生额外开销。因此TBB会将内存数组按照快存行对齐边界，从不会产生伪共享。

  [参考链接](https://en.cppreference.com/w/cpp/thread/hardware_destructive_interference_size)



#### 替换malloc  new delete

```
void * scalable_malloc (size_t size);
void scalable_free (void* ptr);
void * scalable_realloc (void* ptr, size_t size);
void * scalable_calloc (size_t nobj, size_t size);

void* operator new(std::size_t size) throw(std::bad_alloc);
void* operator new(std::size_t size, const std::nothrow_t&) throw( );
void* operator new[](std::size_t size) throw(std::bad_alloc);
void* operator new[](std::size_t size, const std::nothrow_t&) throw( );
void operator delete(void* ptr) throw( );
void operator delete(void* ptr, const std::nothrow_t&) throw( );
void operator delete[](void* ptr) throw( );
void operator delete[](void* ptr, const std::nothrow_t&) throw( );
```



## 容器

TBB提供了一个套高度并发的容器，这些容器允许多个线程在同一个容器上调用一个方法。

#### concurrent_queue

```
template<typename T> class concurrent_queue;
```

```c++
#include "tbb/concurrent_queue.h"
#include <iostream>
using namespace std;
using namespace tbb;
int main( ) {
    concurrent_queue<int> queue;
    for( int i=0; i<10; ++i )
    	queue.push(i);
    for( concurrent_queue<int>::const_iterator i(queue.begin()); i!=queue.end( ); ++i )
    cout << *i << " ";
    cout << endl;
    return 0;
}
```

#### concurrent_vector

```
void Append( concurrent_vector<char>& vector, const char* string ) {
    size_t n = strlen(string)+1;
    memcpy( &vector[vector.grow_by(n)], string, n+1 );
}
```

#### oncurrent_hash_map

并发操作：查找，插入，删除。



## 使用

### parallel_for

parallel for 接口分

TBB建议一次迭代中执行10k到100k条指令，（pytorch粒度值设置的是32768）

第三个参数指定切分的方式，auto_partitioner，static_partitioner，affinity_partitioner

```
void parallel_for( const Range& range, const Body& body )
void parallel_for( const Range& range, const Body& body, const simple_partitioner& partitioner )
void parallel_for( const Range& range, const Body& body, const auto_partitioner& partitioner )
void parallel_for( const Range& range, const Body& body, const static_partitioner& partitioner )
void parallel_for( const Range& range, const Body& body, affinity_partitioner& partitioner )
void parallel_for( const Range& range, const Body& body, task_group_context& context )
void parallel_for( const Range& range, const Body& body, const simple_partitioner& partitioner, task_group_context& context )
void parallel_for(Index first, Index last, Index step, const Function& f)
...
```

上述接口体现了TBB 提供了多种的并行策略。

```c++
int main()
{
   tbb::parallel_for(tbb::blocked_range<int64_t>(0, SIZE, 32768),
    [](const tbb::blocked_range<int64_t>& r) {
        std::cout << "begin: " << r.begin() << ", end: " << r.end() << std::endl;
    });

}
输出结果：
begin: 0, end: 22326
begin: 22326, end: 44652
begin: 44652, end: 66978
begin: 66978, end: 89304
begin: 89304, end: 111630
begin: 111630, end: 133956
begin: 133956, end: 156282
begin: 156282, end: 178608
```



### parallel_reduce

parallel_reduce模板在一个区域迭代，将由各个任务计算得到的部分结果合并，得到最终结果。

parallel_reduce对区域（range）类型的要求与parallel_for一样。body类型需要分割构造函数以及一个

join方法。body的分割构造函数拷贝运行循环体需要的只读数据，并分配并归操作中初始化并归变量

的标志元素。join方法会组合并归操作中各任务的结果。 

```c++
//naive  reduce
float SerialSumFoo( float a[], size_t n ) {
    float sum = 0;
    for( size_t i=0; i!=n; ++i )
        sum += Foo(a[i]);
    return sum;
}

//parallel reduce
int main() {
  std::vector<int> vec;
  for (int i = 0; i < 100; i++)
    vec.push_back(i);

  int result = tbb::parallel_reduce(
      tbb::blocked_range<std::vector<int>::iterator>(vec.begin(), vec.end()), 0,
      [](const tbb::blocked_range<std::vector<int>::iterator> &r,
         int init) -> int {
        for (auto a = r.begin(); a != r.end(); a++)
          init += *a;
        return init;
      },
      [](int x, int y) -> int { return x + y; });

  std::cout << "result:" << result << std::endl;
  return 0;
}
 
```



## 设置信息

Oneflow采用MultiClient模式，需要关心每个rank的计算核心数。

### 设置计算核心数

```c++
    // 设置线程数
    tbb::global_control global_thread_limit(tbb::global_control::max_allowed_parallelism, 4);

    // 获取线程数
    size_t num = tbb::global_control::active_value(
      tbb::global_control::max_allowed_parallelism);
```

### 设置栈大小

```c++
    //设置栈大小
    tbb::global_control s0(tbb::global_control::thread_stack_size, 1*MB);
    {
        tbb::global_control s1(tbb::global_control::thread_stack_size, 8*MB);

        printf("thread_stack_size = %ld \n", tbb::global_control::active_value(tbb::global_control::thread_stack_size));
    }
    printf("thread_stack_size = %ld \n", tbb::global_control::active_value(tbb::global_control::thread_stack_size));
```







