# FTPL (Fork of CTPL Thread pool library)

Forked version of [CTPL Thread pool library](https://github.com/vit-vit/CTPL)


## Reasons to fork

* The library was not maintained for some time
* Need simple library for thread pooling
* When I want to restart thread pool after the stop operation, there is no function for that and init function is private
* Want to target that only for STL not maintaining both boost and STL version, boost version will be removed
* Target architecture for testing will be GCC 6/8 on Linux

## Description

A thread pool is a programming pattern for parallel execution of jobs, http://en.wikipedia.org/wiki/Thread_pool_pattern.

More specifically, there are some threads dedicated to the pool and a container of jobs. The jobs come to the pool dynamically. A job is fetched and deleted from the container when there is an idle thread. The job is then run on that thread.

A thread pool is helpful when you want to minimize time of loading and destroying threads and when you want to limit the number of parallel jobs that run simultanuasly. For example, time consuming event handlers may be processed in a thread pool to make UI more responsive.

Features:
- standard c++ language, tested to compile on MS Visual Studio 2013 (2012?), gcc 4.8.2 and mingw 4.8.1(with posix threads)
- simple but effiecient solution, one header only, no need to compile a binary library
- query the number of idle threads and resize the pool dynamically
- one API to push to the thread pool any collable object: lambdas, functors, functions, result of bind expression
- collable objects with variadic number of parameters plus index of the thread running the object
- automatic template argument deduction
- get returned value of any type with standard c++ futures
- get fired exceptions with standard C++ futures
- use for any purpose under Apache license


# Sample usage

* Put the FTPL directory into your include directories



```c++

#include "ftpl.h"

void first(int id) {
    std::cout << "hello from " << id << '\n';
};

struct Second {
    void operator()(int id) const {
        std::cout << "hello from " << id << '\n';
    }
} second;


void third(int id, const std::string & additional_param) {}

int main () {

    ctpl::thread_pool p(2 /* two threads in the pool */);

    p.push(first);  // function

    p.push(third, "additional_param");

    p.push( (int id)
    {
        std::cout << "hello from " << id << '\n';
    });  // lambda

    p.push(std::ref(second));  // functor, reference

    p.push(const_cast const Second (second));  // functor, copy ctor

    p.push(std::move(second));  // functor, move ctor

}
```
