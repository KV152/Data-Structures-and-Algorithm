Searching a pair of elements in an array with size n like two-sum problem, the searching techniques are summarised in the following table.</br>

Note: time complexity and space complexity is not included in the two pointers appraoch and binaray search.
| Searching  | Time Complexity(average) | Space Complexity |
| --- | --- | --- |
| **Unsorted** |-|-|
| Brute force | O(n^2) | O(1) |
| Hashing | O(n) | O(n) |
| **Sort** |-|-|
| Two Pointers Approach | O(n) + Sorting | O(1) + Sorting |
| Binary Search | O(log(n)) + Sorting | O(1) + Sorting |

Then complexity of frequenlty used comparison sort techniques are summarised in the following talbe. ([From wikipedia](https://en.wikipedia.org/wiki/Sorting_algorithm))
| Sorting | Best | Average | Worst | Memory | Stable | Method | Other Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Insertion Sort | n | n^2 | n^2 | 1 | Yes | Insertion | O(n + d), in the worst case over sequences that have d inversions. Actually it's faster than bubble and selection sort because less swap. Considering cost to implemnt sorting algorithm, it's the best sorting algorithm for small array (N<7).|
| Bubble Sort | n | n^2 | n^2 | 1 | Yes | Exchange | Tiny code size. |
| Selection Sort | n^2 | n^2 | n^2 | 1 | No | Selection | Stable with O(n) extra space or when using linked list
| Shell Sort | nlog(n) | n^(4/3) | N^(3/2) | 1 | No| Insertion | Small code size.|
| Mergesort | nlog(n) | nlog(n) | nlog(n) | n | Yes | Merging | Highly parallelizable (up to O(log n) using the Three Hungarians' Algorithm)| 
| Quicksort | nlog(n) | nlog(n) | n^2 | log(n) |No | Partitioning | Quicksort is usually done in-place with O(log n) stack space.|
| Heapsort | nlog(n) | nlog(n) | nlog(n) | 1 | No | Selection | |

The K-Sum problem can be reduced to two sum problem with (K-2) additional for loop. Therefore, by considering complexity of sort we can summarized the time and space complexity of K-sum problem in the follwoing table.

| Searching  | Time Complexity(average) | Space Complexity |
| --- | --- | --- |
| **Unsorted** |-|-|
| Brute force | O(n^K) | O(1) |
| Hashing | O(n^(K-1)) | O(n)) |
| **Sort** |nlog(n) - n^2|1 - n|
| Two Pointers Approach | O(n^(K-1)) + Sorting | O(1) + Sorting |
| Binary Search | O(n^(K-1)log(n)) + Sorting | O(1) + Sorting |

Summary of K-sum problem:</br>
Becasue the cost of sorting is constat for all value of K, the two pointers approach is the best techqnuie for solving K-sum problem when K>2. The hashing techqniue is best in both time and space for two-sum problem. The combination of two poiners and sorting algorithm is determined by the space complexity for K-sum problem (K>2). If the space complexity is required, insertion sort with space complexity O(1) are the optimal options. Otherwise, quicksort or mergesort are faster in sorting. 
When considering duplicates problem, the staightfoward method to avoid duplicats is by sorting the array and skipping the conunies consecutive same value. Therefore, two pointers approach is the best technique to solve K-sum problem with duplicates problem.

The general solution written in C++ for K-sum can be found in [4-Sum](https://github.com/KV152/LeetCode-Solution/blob/master/4-Sum.md). 


