# pthread 详解

## 参考链接
https://blog.csdn.net/networkhunter/article/details/100218945

## 代码
```
// thread.cpp
#include <iostream>
#include <pthread.h>

using namespace std;

void *thread(void *arg) {
    cout << "This is a thread and arg = " << *(int *)arg << "\n";
    *(int *)arg = 0;
    return arg;
}

int main() {
    pthread_t th;
    int ret;
    int arg = 10;
    int *thread_ret = NULL;
    ret = pthread_create(&th, NULL, thread, &arg);
    if (ret != 0) {
        cout << "Create thread error!\n";
        return -1;
    }
    cout << "This is the main process.\n";
    pthread_join(th, (void **)&thread_ret);
    cout << "thread_ret = " << *thread_ret;
    return 0;
}

./thread
This is the main process.
This is a thread and arg = 10
```

pthread_join() 接口会阻塞主进程的执行，直到合并的线程执行结束。

线程的合并：回收线程资源。线程的合并是一种主动回收线程资源的方案。当被合并线程结束，pthread_join()接口就会回收这个线程的资源，并将这个线程的返回值返回给合并者。
