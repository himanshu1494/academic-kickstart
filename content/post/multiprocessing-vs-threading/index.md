---
title: "MULTIPROCESSING VS. THREADING "
subtitle: WHAT YOU NEED TO KNOW.
date: 2020-09-12T21:32:05.158Z
summary: >-
  <!--StartFragment-->




  As with threading, there are still drawbacks with multiprocessing ... you've got to pick your poison:


  1. There is I/O overhead from data being shuffled around between processes

  2. The entire memory is copied into each subprocess, which can be a lot of overhead for more significant programs


  ## [What Should You Use?](https://timber.io/blog/multiprocessing-vs-multithreading-in-python-what-you-need-to-know/#what-should-you-use-)


  If your code has a lot of I/O or Network usage:


  * Multithreading is your best bet because of its low overhead


  If you have a GUI


  * Multithreading so your UI thread doesn't get locked up


  If your code is CPU bound:


  * You should use multiprocessing (if your machine has multiple cores)


  <!--EndFragment-->
draft: false
featured: false
tags:
  - Parallel Processing
categories:
  - ML
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
<!--StartFragment-->

# Multiprocessing Vs. Threading In Python: What You Need To Know.

***TLDR:*** If you don't want to understand the under-the-hood explanation, here's what you've been waiting for: you can use `threading` if your program is network bound or `multiprocessing` if it's CPU bound.

We're creating this guide because when we went looking for the difference between threading and multiprocessing, we found the information out there unnecessarily difficult to understand. They went far too in-depth, without really touching the meat of the information that would help us decide what to use and how to implement it.

## [What Is Threading? Why Might You Want It?](https://timber.io/blog/multiprocessing-vs-multithreading-in-python-what-you-need-to-know/#what-is-threading-why-might-you-want-it-)

By nature, Python is a linear language, but the threading module comes in handy when you want a little more processing power. While threading in Python cannot be used for parallel CPU computation, it's perfect for I/O operations such as web scraping because the processor is sitting idle waiting for data.

Threading is game-changing because many scripts related to network/data I/O spend the majority of their time waiting for data from a remote source. Because downloads might not be linked (i.e., scraping separate websites), the processor can download from different data sources in parallel and combine the result at the end. For CPU intensive processes, there is little benefit to using the threading module.

![threadingSameDataspace](https://images.ctfassets.net/h6vh38q7qvzk/6RnMqNoKacW2OAAOqw0QW4/a390ea001017ee3492c1c814fa0a7659/threadingSameDataspace.jpeg)

Fortunately, threading is included in the standard library:

```
import threading
from queue import Queue
import time
```

You can use `target` as the callable object, `args` to pass parameters to the function, and `start` to start the thread.

```
def testThread(num):
    print num

if __name__ == '__main__':
    for i in range(5):
        t = threading.Thread(target=testThread, arg=(i,))
        t.start()

```

If you've never seen `if __name__ == '__main__':` before, it's basically a way to make sure the code that's nested inside it will only run if the script is run directly (not imported).

### [The Lock](https://timber.io/blog/multiprocessing-vs-multithreading-in-python-what-you-need-to-know/#the-lock)

You'll often want your threads to be able to use or modify variables common between threads but to do that you'll have to use something known as a `lock`. Whenever a function wants to modify a variable, it locks that variable. When another function wants to use a variable, it must wait until that variable is unlocked.

![lockExplanation](https://images.ctfassets.net/h6vh38q7qvzk/3PLJjBuuYMuuc6UUy8U2qW/b4a0d411cdc1c31c5841230885a3b055/lockExplanation.jpeg)

Imagine two functions which both iterate a variable by 1. The lock allows you to ensure that one function can access the variable, perform calculations, and write back to that variable before another function can access the same variable.

When using the threading module, this can also happen when you're printing because the text can get jumbled up (and cause data corruption). You can use a print lock to ensure that only one thread can print at a time.

```
print_lock = threading.Lock()

def threadTest():
    # when this exits, the print_lock is released
    with print_lock:
        print(worker)

def threader():
  while True:
    # get the job from the front of the queue
    threadTest(q.get())
    q.task_done()

q = Queue()
for x in range(5):
    thread = threading.Thread(target = threader)
    # this ensures the thread will die when the main thread dies
    # can set t.daemon to False if you want it to keep running
    t.daemon = True
    t.start()

for job in range(10):
    q.put(job)
```

Here, we've got 10 jobs that we want to get done and 5 workers that will work on the job.

### [Multithreading Is Not Always The Perfect Solution](https://timber.io/blog/multiprocessing-vs-multithreading-in-python-what-you-need-to-know/#multithreading-is-not-always-the-perfect-solution)

I find that many guides tend to skip the negatives of using the tool they've just been trying to teach you. It's important to understand that there are both pros and cons associated with using all these tools. For example:

1. There is overhead associated with managing threads, so you don't want to use it for basic tasks (like the example)
2. Increases the complexity of the program, which can make debugging more difficult

## [What Is Multiprocessing? How Is It Different Than Threading?](https://timber.io/blog/multiprocessing-vs-multithreading-in-python-what-you-need-to-know/#what-is-multiprocessing-how-is-it-different-than-threading-)

Without multiprocessing, Python programs have trouble maxing out your system's specs because of the `GIL` (Global Interpreter Lock). Python wasn't designed considering that personal computers might have more than one core (shows you how old the language is), so the GIL is necessary because Python is not thread-safe and there is a globally enforced lock when accessing a Python object. Though not perfect, it's a pretty effective mechanism for memory management. *What can we do?*

Multiprocessing allows you to create programs that can run concurrently (bypassing the GIL) and use the entirety of your CPU core. Though it is fundamentally different from the threading library, the syntax is quite similar. The multiprocessing library gives each process its own Python interpreter and each their own GIL.

Because of this, the usual problems associated with threading (such as data corruption and deadlocks) are no longer an issue. Since the processes don't share memory, they can't modify the same memory concurrently.

### [Let's Get Started:](https://timber.io/blog/multiprocessing-vs-multithreading-in-python-what-you-need-to-know/#let-s-get-started-)

```
import multiprocessing
def spawn():
  print('test!')

if __name__ == '__main__':
  for i in range(5):
    p = multiprocessing.Process(target=spawn)
    p.start()
```

If you have a shared database, you want to make sure that you're waiting for relevant processes to finish before starting new ones.

```
for i in range(5):
  p = multiprocessing.Process(target=spawn)
  p.start()
  p.join() # this line allows you to wait for processes
```

If you want to pass arguments to your process, you can do that with args

```
import multiprocessing
def spawn(num):
  print(num)

if __name__ == '__main__':
  for i in range(25):
    ## right here
    p = multiprocessing.Process(target=spawn, args=(i,))
    p.start()
```

Here's a neat example because as you notice, the numbers don't come in the order you'd expect (without the `p.join()`).

<!--EndFragment-->