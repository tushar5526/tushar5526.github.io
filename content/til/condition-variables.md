---
title: Condition Variables - A cool synchronization technique!
date: "2024-04-23T16:34:25+05:30"
type: "post"
description: "Condition Variables to write efficient code"
in_search_index: true
tags: ["system-programming", "sparse-files"]
---

I was a bit taken aback to come to know about `condition-variables` this late in my journey. Condition variables allow you to `wait` and `notify` other threads when a certain event occurs. I understood it best through an example. 

Following is a code sample for `Producer-Consumer` problem using mutexes. 

> **producer-consumer-just-mutex.py**

```python
from threading import Thread, Lock
import time, random

mutex = Lock()

items = []

def timer(t):
    while t:
        time.sleep(1)
        print(f"Time: {t}")
        t -= 1

def producer():
    while True:
        with mutex:
            print("Acquired lock in producer")
            items.append(["say - woooo", "I won't let u gooo"] * random.randint(1, 10))
        print("Released lock in producer")
        timer_thread = Thread(target=timer, args=(10,))
        timer_thread.start()   
        timer_thread.join()

def consumer():
    empty_loop_count = 0
    while True:
        empty_loop_count += 1
        with mutex:
            if items:
                print(f"Got items after {empty_loop_count} empty loops")
                empty_loop_count = 0
                print(items)
                items.clear()

producer_thread = Thread(target=producer)
consumer_thread = Thread(target=consumer)
producer_thread.start()
consumer_thread.start()
    
```

As you can see below, we are wasting a lot of CPU cycles to continuously check the value of mutex while the producer is intentionally made slow to produce after `n` seconds. 

```bash
tusharsamagra@Tushars-MacBook-Pro system-programming-go % python condition-variables/producer-consumer-just-mutex.py 
Acquired lock in producer
Released lock in producer
Got items after 1 empty loops
[['say - woooo', "I won't let u gooo", 'say - woooo', "I won't let u gooo"]]
Time: 10
Time: 9
Time: 8
Time: 7
Time: 6
Time: 5
Time: 4
Time: 3
Time: 2
Time: 1
Acquired lock in producer
Released lock in producer
# 👉 CPU cyles wasted here in the while loop
Got items after 77431958 empty loops
[['say - woooo', "I won't let u gooo", 'say - woooo', "I won't let u gooo", 'say - woooo', "I won't let u gooo", 'say - woooo', "I won't let u gooo", 'say - woooo', "I won't let u gooo"]]
Time: 10
Time: 9
Time: 8
Time: 7
Time: 6
Time: 5
Time: 4
Time: 3
Time: 2
Time: 1
Acquired lock in producer
Released lock in producer
Got items after 77685267 empty loops
[['say - woooo', "I won't let u gooo", 'say - woooo', "I won't let u gooo"]]
```



> **producer-consumer-cond-variables.py**

Here is the same code with some minor modifications to use `Condition Variables`. 


```python
from threading import Thread, Lock, Condition
import time, random

# PS: Condition variables are used in conjuction with Mutexes. You can specify a mutex else Python creates one by default. 
condition = Condition()

items = []

def timer(t):
    while t:
        time.sleep(1)
        print(f"Time: {t}")
        t -= 1

def producer():
    while True:
        # Take lock on mutex
        with condition:
            print("Acquired lock in producer")
            items.append(["work work work"] * random.randint(5, 10))
            # Send a signal
            condition.notify()
        print("Released lock in producer")
        timer_thread = Thread(target=timer, args=(random.randint(5, 10),))
        timer_thread.start()   
        timer_thread.join()

def consumer():
    empty_loop_count = 0
    while True:
        # Take lock on the mutex
        with condition:
            if not items:
                empty_loop_count += 1
                # Free the lock on the mutex, and wait for signal
                condition.wait()
            print(f"Got items after {empty_loop_count} empty loops")
            empty_loop_count = 0
            print(items)
            items.clear()

producer_thread = Thread(target=producer)
consumer_thread = Thread(target=consumer)
producer_thread.start()
consumer_thread.start()
```

Running the above program generates the following logs. 

```bash
Acquired lock in producer
Released lock in producer
Got items after 0 empty loops
[['work work work', 'work work work', 'work work work', 'work work work', 'work work work', 'work work work', 'work work work']]
Time: 7
Time: 6
Time: 5
Time: 4
Time: 3
Time: 2
Time: 1
Acquired lock in producer
Released lock in producer
Got items after 1 empty loops
[['work work work', 'work work work', 'work work work', 'work work work', 'work work work', 'work work work', 'work work work', 'work work work']]
Time: 7
Time: 6
Time: 5
Time: 4
Time: 3
Time: 2
Time: 1
Acquired lock in producer
Released lock in producer
# 👉 No CPU cyles wasted here, as we wait for signal
Got items after 1 empty loops
[['work work work', 'work work work', 'work work work', 'work work work', 'work work work', 'work work work']]
```

As we can see, using condition variables we can pause the execution of code until we receive the signal from a producer that helps in saving a LOT of CPU cycles. 