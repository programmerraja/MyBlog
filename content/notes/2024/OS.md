+++
title = 'OS'
date = 2024-05-11T08:01:04.044+05:30
draft = true
tags =['notes']
+++ 


## Virtual Memory

  A CPU register that can store 32 bits can theoretically address 2 pow (32) different memory locations which 0.5 GB. let say a two program need 200 MB of space to run and 3rd one need 300MB to run if close one 200MB process 
  
```
------------------------------------
| Memory Address | Content         | 
|----------------|-----------------|
| 0x20000000     | proc1 (200MB)   |
| 0x40000000     | proc2 (200MB)   |
| ...            |                 |
| 0xFFFFFFFF     | Unused (100MB)  |
------------------------------------

After  closing proc1
------------------------------------
| Memory Address | Content         | 
|----------------|-----------------|
| 0x20000000     | unused (200MB)  |
| 0x40000000     | proc2 (200MB)   |
| ...            |                 |
| 0xFFFFFFFF     | Unused (100MB)  |
------------------------------------

```

Now we have 300MB space to allocate for porc3 but we cannot split the program and store 200 above and 100 below this problem is called `Memory Fragementaion` Virtual memory are used to solve these kind of problem

Virtual Memory: Each process running on a system has its own virtual memory space. This space is typically divided into pages or segments, which are fixed-size blocks of memory. The size of a page can vary but is often 4KB or 8KB.

 **Page Table**: To manage the mapping between virtual memory and physical memory, the OS maintains a data structure called a page table for each process. The page table contains entries that map virtual addresses to physical addresses. When a process accesses a memory location, the CPU uses the page table to translate the virtual address into a physical address.

why we grouped the page table because if we map single byte for each byte in RAM we need more space to store the page table in memory to avoid we grouped as pages

**Page Offset:** when we grouped the Page table by 4kb or 8KB to find excat memory location in RAM we use page offset
- Let's say we have a page size of 4KB, which is 212212 bytes. This requires 12 bits to represent all possible byte positions within a page.
- If we have a 32-bit virtual address, the lower 12 bits would represent the offset within the page.
- To calculate the offset, we create a bitmask with 12 bits set to 1 (0b00000000000000000000111111111111 in binary) and perform a bitwise AND operation with the virtual address.

Paging divides memory into sections or paging files. When the computer runs out of available memory, unused pages are flushed to disk using the paging file. when we try to access the page that in disk it will throw page fault os will handle and move the page from disk to memory

A **virtual memory buffer** is a region of virtual memory that is used to temporarily store data that is being transferred between different parts of a system, such as between a process and a device, or between a process and the operating system.

Note:the virtual address space dedicated to a process by the OS is [128TB on Linux](https://www.kernel.org/doc/html/latest/x86/x86_64/mm.html?ref=blog.meilisearch.com) and [8TB on Windows](https://techcommunity.microsoft.com/t5/windows-blog-archive/pushing-the-limits-of-windows-virtual-memory/ba-p/723750?ref=blog.meilisearch.com).



TIP: IF you want to cache some file on RAM and move all other from cache cat the huge file it will be moved cache 