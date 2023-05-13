---
date : 2023-05-11 13:50
priority : 1
---
# Metadata
Status ::
Type ::
# Question
How to create a thread pool to use
# Answer
# Note
1. We will need to create a thread pool.
2. We will be able to create mutex by semaphore
3. In advance we will need to be about to check result from thread
```cpp
#include <future>
#include <boost/asio/post.hpp>
#include <boost/asio/thread_pool.hpp>
#include <boost/interprocess/sync/interprocess_semaphore.hpp>
class thread_pool
{
private:
	boost::asio::thread_pool workers; // thread pool to use
	boost::interprocess::interprocess_semaphore queue; // semaphore to use
public:
    thread_pool(std::size_t threads, std::size_t capacity)
      : workers(threads), queue(capacity)
    {
    }
    
    template <typename F>
    void post(F&& f)
    {
      queue.wait(); // semophore wait
      // submit job
      boost::asio::post(workers, [this, f = std::forward<F>(f)] // lambada functiom
                        {
                          f();
                          queue.post(); // semophore post
                        });
    }

    template <typename F>
    auto submit(F&& f) -> std::future<decltype(f())> // return type  
    {
      queue.wait(); // semophre wait
      std::promise<decltype(f())> promise; // create promise
      auto future = promise.get_future();
      boost::asio::post(workers, [this, promise = std::move(promise), f = std::forward<F>(f)] () mutable
                        {
                          promise.set_value(f()); // work with function f()
                          queue.post(); // semophore post
                        });
      return future; // return future
    }
    
    void wait()
    {
      workers.join(); // wait for finish
    }
     
  };

```