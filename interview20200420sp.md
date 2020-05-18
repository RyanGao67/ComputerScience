[https://leetcode.com/problems/trapping-rain-water/](https://leetcode.com/problems/trapping-rain-water/)  
[https://leetcode.com/problems/airplane-seat-assignment-probability/](https://leetcode.com/problems/airplane-seat-assignment-probability/)  
[https://leetcode.com/problems/interval-list-intersections/](https://leetcode.com/problems/interval-list-intersections/)  
1. TCP/UDP  
[https://www.guru99.com/tcp-vs-udp-understanding-the-difference.html](https://www.guru99.com/tcp-vs-udp-understanding-the-difference.html)
2. What happened when enter a URL in search bar and hit enter?  
[https://medium.com/@maneesha.wijesinghe1/what-happens-when-you-type-an-url-in-the-browser-and-press-enter-bb0aa2449c1a](https://medium.com/@maneesha.wijesinghe1/what-happens-when-you-type-an-url-in-the-browser-and-press-enter-bb0aa2449c1a)
2. Tell me how a syscall works on x86 ?    
[https://en.wikibooks.org/wiki/X86_Assembly/Interfacing_with_Linux](https://en.wikibooks.org/wiki/X86_Assembly/Interfacing_with_Linux)
4. Using Python, write a one-liner function to return a list containing only the unique elements of the given list. Order doesn't matter. Next, write a function to return an order-preserving list containing only the unique elements of the given list.  
```
from  more_itertools import unique_everseen
list(unique_everseen(items))
```
5. Explain the Python GIL.  Process & Thread，  
[https://realpython.com/python-gil/#what-problem-did-the-gil-solve-for-python](https://realpython.com/python-gil/#what-problem-did-the-gil-solve-for-python)
6. In Python, what is the difference between a generator expression and a list comprehension?  
[https://code-maven.com/list-comprehension-vs-generator-expression](https://code-maven.com/list-comprehension-vs-generator-expression)
7. Tech questions: What is an Unordered_map? What is a pure virtual function   
[https://www.geeksforgeeks.org/unordered_map-in-cpp-stl/](https://www.geeksforgeeks.org/unordered_map-in-cpp-stl/)  
[https://en.wikipedia.org/wiki/Virtual_function](https://en.wikipedia.org/wiki/Virtual_function)
9. coding exercise, with a key value map. the maps had limited memory 4096kB and I had to write set and get method for a key value pair.  
 [https://leetcode.com/problems/lru-cache/](https://leetcode.com/problems/lru-cache/)
10. Find the Kth largest number in a shuffled array.  
[https://leetcode.com/problems/kth-largest-element-in-an-array/](https://leetcode.com/problems/kth-largest-element-in-an-array/)
12. merge two dicts recursively  
```
def merge(a, b):
    for key in b:
        if key in a:
            if isinstance(a[key], dict) and isinstance(b[key], dict):
                merge(a[key], b[key], path + [str(key)])
        else:
            a[key] = b[key]
    return a
```
13. find where two intervals intersect  
[https://leetcode.com/problems/interval-list-intersections/](https://leetcode.com/problems/interval-list-intersections/)  
14. shared_ptr vs unique_ptr,  
* Use unique_ptr when you want a single pointer to an object that will be reclaimed when that single pointer is destroyed.  
* Use shared_ptr when you want multiple pointers to the same resource.  

15.virtual memory in an OS   
* The computer's operating system, using a combination of hardware and software, maps memory addresses used by a program, called virtual addresses, into physical addresses in computer memory. Main storage, as seen by a process or task, appears as a contiguous address space or collection of contiguous segments. The operating system manages virtual address spaces and the assignment of real memory to virtual memory. Address translation hardware in the CPU, often referred to as a memory management unit (MMU), automatically translates virtual addresses to physical addresses. Software within the operating system may extend these capabilities to provide a virtual address space that can exceed the capacity of real memory and thus reference more memory than is physically present in the computer.   

17.Write a C++ code that writes all the sequence formed by X 0's and Y 1's.  
[https://www.geeksforgeeks.org/print-all-combinations-of-given-length/](https://www.geeksforgeeks.org/print-all-combinations-of-given-length/)  
18. What is the system call to create new processes or threads in Linux.  
* Processes are usually created with fork, threads are usually created with clone nowadays. Both fork and clone map to the same kernel function do_fork internally. This function can create a lightweight process that shares the address space with the old one, or a separate process (and many other options), depending on what flags you feed to it.

* The important difference is that fork guarantees that a complete, separate copy of the address space is made. This, as Basil points out correctly, is done with copy-on-write nowadays and therefore is not nearly as expensive as one would think.
When you create a thread, it just reuses the original address space and the same memory.

* However, one should not assume that creating processes is generally "lightweight" on unix-like systems because of copy-on-write. It is somewhat less heavy than for example under Windows, but it's nowhere near free.One reason is that although the actual pages are not copied, the new process still needs a copy of the page table. This can be several kilobytes to megabytes of memory for processes that use larger amounts of memory. Another reason is that although copy-on-write is invisible and a clever optimization, it is not free, and it cannot do magic. When data is modified by either process, which inevitably happens, the affected pages fault. Redis is a good example where you can see that fork is everything but lightweight (it uses fork to do background saves).

* copy on write: If a resource is duplicated but not modified, it is not necessary to create a new resource; the resource can be shared between the copy and the original. Modifications must still create a copy, hence the technique: the copy operation is deferred to the first write. 

* Address space:  is the amount of memory allocated for all possible addresses for a computational entity, such as a device, a file, a server, or a networked computer.

22. design a load balanced log in system
23. Kafka，MongoDB，how is your Linux Skill. Resume
24. padding, malloc, virtual, function stack,
[https://www.geeksforgeeks.org/structure-member-alignment-padding-and-data-packing/](https://www.geeksforgeeks.org/structure-member-alignment-padding-and-data-packing/)  
[https://www.design-reuse.com/articles/25090/dynamic-memory-allocation-fragmentation-c.html](https://www.design-reuse.com/articles/25090/dynamic-memory-allocation-fragmentation-c.html)  
25. Explain the Internet to an 8-Year Old in 3 Sentences   
```
-              What? Use your first sentence to establish a basic premise: “The Internet is a series of tubes.”

-              How? Your second sentence can describe the first: “The tubes connect information that is stored on computers throughout the world.”

-              Why? Finally, close by summing up the purpose of the Internet: “It helps people to access global information quickly and easily.”
```
•             What are you looking for in your next role? Where you see yourself in 2/3 years time and 10 years   
**senior software engineer, decide a direction, lay the foundation, learn how to refactoring the code to make the clean code, archetectureing**
•             Why you want to work for them  Why you’re looking to leave your current role ?What do you know about Squarepoint Capital?  
**As a technology and data-driven firm, we design and build our own cutting-edge systems, from high performance trading platforms to large scale data analysis and compute farms.**  
•             Have you initiated any projects? If so, what and why? Where have you made an impact on the business?   
**File landing system**
•             When you have tackled a key area/issue or solved a particularly tough problem?   
**Debugging the distributed system**
•             What aspects of software development technology or process are you most interested in and why?   
**Microservices**
•             What specific technology do you dislike the most and why?  
•             What does good software look like?  
**Good software is functional. 
Good software is measurable. 
Good software is debuggable. 
Good software is reusable. Low Coupling, High Cohesion
Good software is extensible.**
•             Any technical challenges/complexities that you have faced and how you overcame them?  
•             How do you get your tech news?  
•             why should the company employ you  
•             how would you deal with a tricky/demanding user? What did you do to communicate properly?  
•             Challenging others  
•             How you react under pressure  
•             If our competitor, X, released a n/////////////////////////////////////ew product, Y, how would you advise our team to respond?  
•             Ask insightful questions - Prepare these questions before the interview  
