# coro_wait
Suspending function execute by time and use only one thread for work

## Requirenments
Boost.Context 1.71.0

## Example
```cpp
#include <iostream>
#include <chrono>
#include <thread>
#include <functional>

#include "coro_wait.h"

using namespace std::chrono_literals;

void foo() {
  while(true){
    this_coro::wait(5s);
    std::cout << "Hi, i loop every 5 seconds from thread id: "
            << std::this_thread::get_id() << std::endl;
  }
}

void bar(int time) {
  while (true) {
    this_coro::wait(std::chrono::seconds(time));
    std::cout << "Hi, i loop every " << time << " seconds from thread id: "
            << std::this_thread::get_id() << std::endl;
  }
}

int main() {
  coro_wait instance;

  instance.add_to_queue(foo);
  instance.add_to_queue(std::bind(bar, 2)); /* you can use bind for pass params */

  while (true) {
    instance.process();
   }
}
```
