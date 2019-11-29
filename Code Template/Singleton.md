* 代码 

```c++
#pragma once

#include <pthread.h>

namespace pcdn_index_server {
    
template <class T>
class Singleton {
public:
    static T* Instance() {
      if(NULL == _instance) {
        pthread_mutex_lock(&_mutex);
        if(NULL == _instance) {
          _instance = new T;
        }
        pthread_mutex_unlock(&_mutex);
      }
      
      return _instance;
    }
    
    static void Destroy() {
      if(NULL != _instance) {
        pthread_mutex_lock(&_mutex);
        delete _instance;
        _instance = NULL;
        pthread_mutex_unlock(&_mutex);
      }
    }

private: //method
    Singleton();
    Singleton(const Singleton&);
    Singleton& operator =(const Singleton&);

private: //properties
    static T* _instance;
    static pthread_mutex_t _mutex;
};

template <class T>
pthread_mutex_t Singleton<T>::_mutex = PTHREAD_MUTEX_INITIALIZER;

template <class T>
T* Singleton<T>::_instance = NULL;

} // namespace pcdn_index_server

```

* 使用方式

```C++
Singleton<index_manager>::Instance()->get_fileinfo(temp_req, err_str);
```

