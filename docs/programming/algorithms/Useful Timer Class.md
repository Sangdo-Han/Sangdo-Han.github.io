---
layout: default
title: Useful Timer Class
parent: Algorithms and Data structures
nav_order: 1
tags: 
  - single-process
  - C++
  - python
---

# Timer class
When we compare the algorithms, we use Big-O notation for the time-space efficiency.   
However, making a timer is often more practical.  

For the algorithm section, we will use a simple Timer class both in python and C++.

## C++ version Timer    

By using constructor and destructor, we can make a simple Timer as followings:

`Timer.h`
```cpp
// Timer.h
#pragma once
#include <chrono>

struct Timer {
    std::chrono::time_point<std::chrono::steady_clock> m_Start;
    Timer();
    ~Timer();
};
```

`Timer.cpp`
```cpp
// Timer.cpp
#include <iostream>
#include "Timer.h"

Timer::Timer()
    : m_Start(std::chrono::high_resolution_clock::now())
{
}
Timer::~Timer()
{
    auto end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<float> duration = end - m_Start;
    std::cout << "Process Ended in " << duration.count() << " [s].\n";
}
```
When we use this timer, it is only to instantiate the struct as the following `Main.cpp`.    


`Main.cpp`
```cpp
#include <iostream>
#include "Timer.h"

int main()
{
    Timer timer;

    // ... do something
} // code block ends 
```
At the end of code block, timer's destructor `Timer::~Timer()` would be called, printing the duration of the code block.


## Python version Timer  

In python, it is recommended using `__enter__` and `__exit__` methods with `with` statement because of robustness, however, I prefer the C++ way (cosntructor / destructor) because of contextmanager with `with` statement makes additional indentation.

`utils.py`
```python
import time

class Timer(object):
    def __init__(self):
        self._start_time = time.perf_counter()
    def __del__(self):
        print(
            "Process Ended in "
            f"{time.perf_counter()-self._start_time} [s]"
        )

class Timer2(object):
    def __init__(self):
        self._start_time = None

    def __enter__(self):
        self._start_time = time.perf_counter()
        return self

    def __exit__(self, *exc_info):
        print(
            "Process Ended in "
            f"{time.perf_counter()-self._start_time} [s]"
        )
```

`main.py`
```python
from utils import Timer, Timer2

def main1():
    timer = Timer()
    # do something ...

def main2():
    with Timer2() as timer2:
        #do something ... 
```


