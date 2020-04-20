1. TCP/UDP
2. Tell me how a syscall works on x86 ?  
3. Write a function to compute and return the number of ways in which to climb a flight of n steps, taking 1, 2 or 3 steps at a time.  
4. Using Python, write a one-liner function to return a list containing only the unique elements of the given list. Order doesn't matter. Next, write a function to return an order-preserving list containing only the unique elements of the given list.  
```
list(unique_everseen(items))
```
5. Explain the Python GIL.  
https://realpython.com/python-gil/#what-problem-did-the-gil-solve-for-python
6. In Python, what is the difference between a generator expression and a list comprehension?  
7. Tech questions: What is an Unordered_map? What is a pure virtual function
8. Hackerrank question: Calculate a checksum for a string.  
9. coding exercise, with a key value map. the maps had limited memory 4096kB and I had to write set and get method for a key value pair.  
10. Find the Kth largest number in a shuffled array.  
11. code a fibonacci  
12. merge two dicts recursively  
```
def merge(a, b):
    for key in b:
        if key in a:
            if isinstance(a[key], dict) and isinstance(b[key], dict):
                merge(a[key], b[key], path + [str(key)])
            elif a[key] == b[key]:
                pass # same leaf value
            else:
                raise Exception('Conflict at %s' % '.'.join(path + [str(key)]))
        else:
            a[key] = b[key]
    return a
```
13. find where two intervals intersect
14. shared_ptr vs unique_ptr, 
* Use unique_ptr when you want a single pointer to an object that will be reclaimed when that single pointer is destroyed.
* Use shared_ptr when you want multiple pointers to the same resource.
15. difference between a thread and a process, virtual memory in an OS 
* The computer's operating system, using a combination of hardware and software, maps memory addresses used by a program, called virtual addresses, into physical addresses in computer memory. Main storage, as seen by a process or task, appears as a contiguous address space or collection of contiguous segments. The operating system manages virtual address spaces and the assignment of real memory to virtual memory. Address translation hardware in the CPU, often referred to as a memory management unit (MMU), automatically translates virtual addresses to physical addresses. Software within the operating system may extend these capabilities to provide a virtual address space that can exceed the capacity of real memory and thus reference more memory than is physically present in the computer.
16. LRU cache
17.Write a C++ code that writes all the sequence formed by X 0's and Y 1's.  
18. What is the system call to create new processes or threads in Linux.  
* Processes are usually created with fork, threads are usually created with clone nowadays. Both fork and clone map to the same kernel function do_fork internally. This function can create a lightweight process that shares the address space with the old one, or a separate process (and many other options), depending on what flags you feed to it.

* The important difference is that fork guarantees that a complete, separate copy of the address space is made. This, as Basil points out correctly, is done with copy-on-write nowadays and therefore is not nearly as expensive as one would think.
When you create a thread, it just reuses the original address space and the same memory.

* However, one should not assume that creating processes is generally "lightweight" on unix-like systems because of copy-on-write. It is somewhat less heavy than for example under Windows, but it's nowhere near free.One reason is that although the actual pages are not copied, the new process still needs a copy of the page table. This can be several kilobytes to megabytes of memory for processes that use larger amounts of memory. Another reason is that although copy-on-write is invisible and a clever optimization, it is not free, and it cannot do magic. When data is modified by either process, which inevitably happens, the affected pages fault. Redis is a good example where you can see that fork is everything but lightweight (it uses fork to do background saves).

* copy on write: If a resource is duplicated but not modified, it is not necessary to create a new resource; the resource can be shared between the copy and the original. Modifications must still create a copy, hence the technique: the copy operation is deferred to the first write. 

* Address space:  is the amount of memory allocated for all possible addresses for a computational entity, such as a device, a file, a server, or a networked computer.
