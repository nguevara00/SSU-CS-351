Report

1) Which program is fastest? Is it always the fastest?
2) Which program is slowest? Is it always the slowest?
3) Was there a trend in program execution time based on the size of data in each node? If so, what and why?
4) Was there a trend in program execution time based on the length of the block chain?
5) Consider heap breaks, what's noticeable? Does increasing the stack size affect the heap? Speculate on any similarities and differences in programs?
6) Considering either the malloc.c or alloca.cpp versions of the program, generate a diagram showing two nodes included in the diagram
    The relationship of head tail and node next pointers
    Show the size in bytes and structure of a node that allocated six bytes of data
    include the bytes pointer, and indicate using an arrow which byte in the allocated memory it points to
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