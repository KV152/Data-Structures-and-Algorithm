Searching a pair of elements in an array with size n like two-sum problem, the searching techniques are summarised in the following table.</br>

Note: time complexity and space comlexity is not included in the two pointer appraoch and binaray search.
| Searching  | Time Complexity | Space Complexity |
| --- | --- | --- |
| **Unsorted** |-|-|
| Brute force | O(n^2) | O(1) |
| Hashing | O(n) | O(n) |
| **Sorted** |-|-|
| Two Pointer Approach | O(n) | O(1) |
| Binary Search | O(log(n)) | O(1) |

Then complexity of frequenlty used comparison sorting techniques are summarised in the following talbe. ([From wikipedia](https://en.wikipedia.org/wiki/Sorting_algorithm))
| Sorting | Best | Average | Worst | Memory | Stable | Method | Other Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Insertion Sort | n | n^2 | n^2 | 1 | Yes | Insertion | O(n + d), in the worst case over sequences that have d inversions. |
| Bubble Sort | n | n^2 | n^2 | 1 | Yes | Exchange | Tiny code size. |
| Selection Sort | n^2 | n^2 | n^2 | 1 | No | Selection | Stable with O(n) extra space or when using linked lists. |
| Shell Sort | nlog(n) | n^(4/3) | N^(3/2) | 1 | No| Insertion | Small code size.|
| Mergesort | nlog(n) | nlog(n) | nlog(n) | n | Yes | Merging | Highly parallelizable (up to O(log n) using the Three Hungarians' Algorithm)| 
| Quicksort | nlog(n) | nlog(n) | n^2 | log(n) |No | Partitioning | Quicksort is usually done in-place with O(log n) stack space.|
| Heapsort | nlog(n) | nlog(n) | nlog(n) | 1 | No | Selection | |

    
