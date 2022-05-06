Complexity of algorithms
========================

> id: f0baa25a-4cff-4d3d-b758-5e03f4a8c69c
> slug:
> 	- null
> 	en: complexity-of-algorithms
> 
> cs: slozitost-algoritmu
> publicationDate: '2021-08-03 20:40:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '392a0151a8e00c167f7a27f2081decab'

Each algorithm has its own complexity, which can be expressed in mathematical notation. This overview shows the typical complexity of algorithms according to the size of the input data (i.e. the number of elements they work with) and which types of algorithms are suitable for which type of task.

In general, there is a best specialized algorithm for each type of problem. No algorithm is universally best, and you always need to know your context.

Big O Notation
--------------

The *Big O Notation* is used to classify algorithms according to how their runtime or memory requirements increase with increasing input size.

The following chart shows the most common growth orders of algorithms specified in Big O Notation.

Below is a list of some of the most commonly used Big O notations and a comparison of their performance with respect to different input data sizes.

| Big O Notation | Complexity for 10 elements | Complexity for 100 elements | Complexity for 1000 elements |
| -------------- | ---------------------------- | ----------------------------- | ------------------------------- |
| **O(1)** | 1 | 1 | 1 |
| **O(log N)** | 3 | 6 | 9 |
| **O(N)** | 10 | 100 | 1000 |
| **O(N log N)** | 30 | 600 | 9000 |
| **O(N^2)** | 100 | 10000 | 1000000 |
| **O(2^N)** | 1024 | 1.26e+29 | 1.07e+301 |
| **O(N!)** | 3628800 | 9.3e+157 | 4.02e+2567 |

### Complexity of data structure operations

| Data Structure | Access | Search | Insert | Subtract | Comment |
| ----------------------- | :-------: | :-------: | :-------: | :-------: | :-------- |
| **Array** | 1 | n | n | n | n | |
| **Stack** | n | n | n | 1 | 1 | |
| **Queue** | n | n | n | 1 | | |
| **Linked List** | n | n | n | 1 | n |
| **Hash Table** | - | n | n | n | n | In the case of a perfect hash function, the complexity will be O(1) |
| **Binary Search Tree** | n | n | n | n | In the case of a balanced tree, the complexity will be O(log(n)). |
| **B-Tree** | log(n) | log(n) | log(n) | log(n) | log(n) | |
| **Red-Black Tree** | log(n) | log(n) | log(n) | log(n) | |
| **AVL Tree** | log(n) | log(n) | log(n) | log(n) | log(n) | |
| **Bloom Filter** | - | 1 | 1 | - | When searching for `false positives` |

### Complexity of sorting algorithms

| Algorithm name | Best | Average | Worst | Memory | Stable? | Comment |
| --------------------- | :-------------: | :-----------------: | :-----------------: | :-------: | :-------: | :-------- |
| **Bubble sort** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | Yes | |
| **Insertion sort** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | | |
| **Selection sort** | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | 1 | | |
| **Heap sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | 1 | | No |
| **Merge sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n | Yes | |
| | **Quick sort** | n&nbsp;log(n) | n&nbsp;log(n) | n<sup>2</sup> | log(n) | No | Quicksort is usually performed with O(log(n)) stack complexity. |
| **Shell sort** | n&nbsp;log(n) | depends on the sequence | n&nbsp;(log(n))<sup>2</sup> | 1 | | No |
| **Counting sort** | n + r | n + r | n + r | n + r | Yes | r - the largest number in the array |
| **Radix sort** | n * k | n * k | n * k | n + k | Yes | k - length of the longest key |
