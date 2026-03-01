Nicholas Guevara
CS 351 Shreiner
Project 1
2/28/26

For project 1, several experiments were conducted using the provided code. The results are summarized in the included tables below. 

---

## 1) Which program is fastest? Is it always the fastest?
    Across the experiments using `-O2` optimization, `malloc` was the generally the fastest, with `alloca` close in performance. It was not always the fastest. In some small workloads, `alloca` had slightly lower runtimes. When the payload size became large, all the implementations showed nearly identical performance. Without optimization, `malloc` was clearly the fastest, and other implementations slowing down significantly when compared to the optimized run.  

---

## 2) Which program is slowest? Is it always the slowest?
    `new` was generally the slowest implementation. `list` performed similarly and slightly faster in some of the configurations. The additional overhead of heap allocation by `operator new` contributed to its slower runtime than `malloc` or `alloca`. 

---

## 3) Was there a trend in program execution time based on the size of data in each node? If so, what and why?
    Yes, there was an obvious trend. As the size of data in each node increased, program execution time increased. With small payload sizes, runtimes were very low and differences between the implementations were noticeable. As payload size increased, differences between the implementations were smaller and the performance was similar. When the payload size is small, memory allocation overhead has a large impact on total runtime. As the size increases, the cost of hashing dominates the execution time. In that case, all the implementations converge in performance, making the choice of strategy less significant.

---

## 4) Was there a trend in program execution time based on the length of the block chain?
    Again there was a clear trend. As the number of nodes in the block chain increased, program execution time increased proportionally. Small chain lengths produce short runtimes, larger chain lengths resulted in significantly longer runtimes. This is expected because building the list and hashing require processing each node. Each node adds an amount of work, so we expect that runtime will grow linearly with the number of nodes. The relative performances of the implementations remained consistent as chain length increased.

---

## 5) Consider heap breaks, what's noticeable? Does increasing the stack size affect the heap? Speculate on any similarities and differences in programs?
    The most noticeable result was that `alloca` produced significantly fewer heap breaks compared to the other implementations. Further, for this implementation the number of breaks did not increase as the chain length grew. The other implementations showed an increase in breaks as the chain length grew. The reason is that `alloca` allocates memory for the nodes on the stack rather than the heap, so increasing the nodes did not require growth of the heap. The other implementations do use heap allocations for each node, causing the heap to expand as memory is requested. The allocator increases the heap in batches, which is why the breaks do not increase one-to-one with the number of nodes. 

---

## 6) Considering either the malloc.c or alloca.cpp versions of the program, generate a diagram showing two nodes included in the diagram
    The relationship of head tail and node next pointers
    Show the size in bytes and structure of a node that allocated six bytes of data
    include the bytes pointer, and indicate using an arrow which byte in the allocated memory it points to
```
    Node Diagram:
        
         Head             Tail
           |               |   
           V               V
         NODE A --NEXT--> Node B ---NEXT--> Null

    Node Structure:
        Next (pointer)      : 8 Bytes
        Size (unsigned int) : 4 bytes
        padding (unused)    : 4 bytes
        bytes (pointer)     : 8 bytes
        allocated for data  : 6 bytes

    
      NEXT    SIZE pad  BYTES    DATA       
    |--------|----|----|--------|------|
                                 ^
                                 | bytes pointer poits to first byte of the payload
```

7) There's an overhead to allocating memory, initializing it, and eventually processing (in our case, hashing it). For each program, were any of these tasks the same? Which ones were different


8) As the size of data in a Node increases, does the significance of allocating the node increase or decrease?

# Experimental Results
## Baseline Configuration
OPT="-O2 -g2"
MIN_BYTES = 64
MAX_BYTES = 64
NUM_BLOCKS = 100000
NUM_TRIALS = 10

### Runtime Results

| Program | Min (s) | Avg (s) | Max (s) |
|----------|---------|---------|---------|
| alloca   | 0.020 | 0.027 | 0.030 |
| list     | 0.020 | 0.035 | 0.040 |
| malloc   | 0.020 | 0.026 | 0.030 |
| new      | 0.030 | 0.037 | 0.040 |

---

## Optimization Comparison (No Optimization)
OPT="-g"
MIN_BYTES = 64
MAX_BYTES = 64
NUM_BLOCKS = 100000

| Program | Min (s) | Avg (s) | Max (s) |
|----------|---------|---------|---------|
| alloca   | 0.070 | 0.070 | 0.070 |
| list     | 0.200 | 0.217 | 0.230 |
| malloc   | 0.060 | 0.060 | 0.060 |
| new      | 0.200 | 0.208 | 0.230 |

---

## Payload Size Experiment
OPT="-O2 -g2"  
NUM_BLOCKS=100000  

### Payload = 10 bytes

| Program | Avg (s) |
|----------|---------|
| alloca | 0.010 |
| list   | 0.019 |
| malloc | 0.013 |
| new    | 0.020 |

### Payload = 64 bytes

| Program | Avg (s) |
|----------|---------|
| alloca | 0.027 |
| list   | 0.035 |
| malloc | 0.026 |
| new    | 0.037 |

### Payload = 1024 bytes

| Program | Avg (s) |
|----------|---------|
| alloca | 0.285 |
| list   | 0.287 |
| malloc | 0.271 |
| new    | 0.279 |

---

## Chain Length Experiment
OPT="-O2 -g2"  
MIN_BYTES=64  
MAX_BYTES=64  

### 10,000 Nodes

| Program | Avg (s) |
|----------|---------|
| alloca | 0.006 |
| list   | 0.008 |
| malloc | 0.008 |
| new    | 0.009 |

### 100,000 Nodes

| Program | Avg (s) |
|----------|---------|
| alloca | 0.027 |
| list   | 0.035 |
| malloc | 0.026 |
| new    | 0.037 |

### 500,000 Nodes

| Program | Avg (s) |
|----------|---------|
| alloca | 0.111 |
| list   | 0.144 |
| malloc | 0.122 |
| new    | 0.139 |

---

## Heap Break Experiment
OPT="-O2 -g2"  
MIN_BYTES=64  
MAX_BYTES=64  

### 100,000 Nodes

| Program | Heap Breaks |
|----------|-------------|
| alloca | 69 |
| list   | 187 |
| malloc | 169 |
| new    | 187 |

### 500,000 Nodes

| Program | Heap Breaks |
|----------|-------------|
| alloca | 69 |
| list   | 657 |
| malloc | 568 |
| new    | 657 |