# Dynamic Memory Allocator

A simple memory allocator written in C, simulating how an allocator manages a heap.

The "heap" is a character buffer where `_` marks a free byte and `@` marks the end of an allocated block, so the state of memory can be printed and inspected after every operation.

## How it works

- **`initialize`** creates the pool (10 bytes to start) and marks it all free.
- **`scanMemory`** does a first-fit search for the first run of free bytes large enough for the request.
- **`allocator`** returns the start offset of a block, or `-1` if nothing fits. The caller writes its data at that offset, ending with `@`.
- **`allocatorNew`** does the same but never fails: if no block fits, it grows the pool by another 10 bytes (`addMemory`, which reallocates and copies) and tries again.
- **`deallocator`** frees a block by marking its bytes free up to and including the `@` terminator.
- Allocated blocks are tracked in a singly linked list (`addNode` / `removeNode`), and `clear` releases the pool.

## Build

The file provides the allocator functions only. Write a `main` that calls them, then:

```bash
gcc -Wall -o allocator Dynamic_memory_allocator.c your_main.c
```
