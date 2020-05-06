# Hashing 
## Contents
  - [Overview](#overview)
  - [Hash Functions](#hash-functions)
    - [Integer data](#integer-data)
    - [Variable-length data](#variable-length-data)
  - [Collision resolution techniques](#collision-resolution-techniques)
    - [Open hashing](#open-hashing)
    - [Closed hashing](#closed-hashing)
## Overview
- Feature
  - The process of finding a record using some computation to map its key value to a position in the array is called **hashing**.
  - The function that maps key values to position is called a **hash function** and will be denoted by **h**.
  - The array that holds the records is called the **hash table** and will be denoted by **HT**.
  - A position in the hash table is also known as a **slot**.
  - The number of slots in hash table HT will be denoted by the vairable M, with slots numbered from 0 to M-1.
  - **Load factor** for the table as &alpha; = N/M, where N is the number of records currently in the table.

- Pros and Cons
  - Cons
    - Not good for multiple records with the same key value (duplicate).
    - Not good for answering range searches.
    - Not good for visiting the records in key order.
  - Pros
    - Good for accessing involves only exact-match queries.
    - Good for both in-memory and disk-based searching.
    - One of the two most widely used methods for organizing large databases stored on disk (the other is B-tree).
 
- Collision

  Typically, there are many more values in the key range than ther are slots in the hash table. At least some of the slots must be mapped to from multiple key values. Given a hash fucntion **h** and two keys k<sub>1</sub> and k<sub>2</sub>, if **h**(k<sub>1</sub>) = &beta; = **h**(k<sub>2</sub>), where &beta; is a slot in the table, then we say that k<sub>1</sub> and k<sub>2</sub> have a collision at slot &beta; under hash function **h**.  
  Finding a record with the key value K in a database orgranized by hasing follows a two-step procedure:
  1. Compute the table location **h**(K).
  2. Starting with slot **h**(K), licate the recored containing key K (if necessary) using a collision resolution policy.

  Perfect hashing is a system in which records are hased such that there are no collisions. Perfect hashing is efficient but selecting perfect hash function can be expensive. It might be worthwhile when exremely efficient search performance is required. (Searching for data on a read-only CD, where database will never be changed and time for each access is expensive.)

## Hash Functions 
  Goal: Storing the actual records in the collection such that each slot in the hash table has equal probability of being filled.
  However input data values might be prooly distributed.
  - Natural frequency distributions tends to follow a common pattern where a few of the entities occur frequently while most entities occur relatively rarely. (population in country)
  - Collected data are likely to be skewed in some ways. (price might be end in 0 5 or 9)
  - If the inpout is a colleciton of common English words, the begninning letter will be poorly distributed.
  
 Hence, two situations faced when we designed hash functions
 - Knowing nothing about hte distribution of the incoming keys. 
    - Aim to desing evenly ditribution hash function.
 - Knowing something about the distribution of the incoming keys. 
    - Distribution-dependent hash function is requried to aviod assigning clusters of related key values to the same hash table slot.
  
 [Algorithm for hashing integer and variable-length data](https://en.wikipedia.org/wiki/Hash_function "hash function in wikepedia")
 
  ### Integer data:
  - Identity hash function\
    If data is small enough, data itself (reinterpreted as an integer) can be used as hash value.
    - "small enough" denpends on the size of the type that is used as the hashed value. For example, in Java, the hash code is a 32-bit integer. Thus the 32-bit integer Integer and 32-bit floating-point Float objects can simply use the value directly.
    
  - Trivial hash function\
    If keys are uniformly or sufficiently uniformly distributed over the key space, so that the key values are essentially random, they may be considered to be already 'hashed'.
    
  - Folding \
    There are 2 types of folding methods called **Fold shift** and **Fold boundary**. Following contents based on [answer found in stackoverlow](https://stackoverflow.com/questions/36565101/what-is-folding-technique-in-hashing-and-how-to-implement-it) and on website [TechMight](http://techmightsolutions.blogspot.com/2012/07/hash-function-folding.html)
    - Fold Shift
      - In fold shift, the key value is divided into parts whose size matches the size of the required address. Then the left and right parts are shifted and added with the middle part.
      - For example: Key: 123456789 and size of required address is 3 digits.
        - 123 + 456 + 789 = 1368. To reduce the size to 3, either 1 or 8 is removed and accordingly the key would be 368 or 136 respectively.
    - Fold boundary
      - Again dividing the key in parts whose size matches the size of required address. But, size of middle part can be smaller than the required address.
      - In fold boundary, the left and right numbers are folded on a fixed boundary between them and the center number. The two outside values are thus reversed.
      - For example: Key: 123456789 and size of required address is 3 digits
        - 321 (reversed)+ 456 + 987 (reversed) = 1764(discard 1 or 4)
        
  - Mid-squares\
    Squaring the input and extracting an appropriate number of middle digits or bits. 
    - For example: 
      - If the input is 123,456,789 and the hash table size 10,000
      - Squaring the key produces 1.524157875019e16, so the hash code is taken as the middle 4 digits of the 17-digit number (ignoring the high digit) 8750.
    - Good if there are not a lot of leading or trailing zeros in the key. 
    - Not good, if an arbitrary key is not a good multiplier. 
   
   - Division hashing\
    A standard technique is to use a modulo function on the key, by selecting a divisor M which is a prime number close to the table size, so h (K) = K mod M. The table size is usually a power of 2. This gives a distribution from {0, M − 1}.
    - Good for large number of key sets and sufficent randomly.
    - Not good due to division might be 10 times slower than multiply.
    - Not good due to clustered keys (e.g. 123000, 456000)
   
   - Zobrist hashing\
     Zobrist Hashing is a hashing function that is widely used in 2 player board games. It is the most common hashing function used in transposition table. Transposition tables basically store the evaluated values of previous board states, so that if they are encountered again we simply retrieve the stored value from the transposition table. [Source](https://www.geeksforgeeks.org/minimax-algorithm-in-game-theory-set-5-zobrist-hashing/)
     
   - Other hashings\
    Other hashing function for integer data shown in wikipedia: Algebraic coding, Unique permutation hashing, Multiplicative hashing, Fibonacci hashing, Customized hash function 
    
### Variable-length data
  When data values are long (or variable-length) character strings, their distribution is usually very uneven, with complicated dependencies. For such data, it is prudent to use a hash function that depends on all characters of the string.
  - Middle and ends\
    Adding the first and last n characters of a string along with the length, or forming a word-size hash from the middle 4 characters of a string.
    - Saving iterating over the (potentially long) string.
    - Good as a custom hash function if the structure of the keys is such that either the middle, ends or other field(s) are zero or some other invariant constant that doesn't differentiate the keys; then the invariant parts of the keys can be ignored.  
    - Not good when clustering or other pathologies in the key set.
   
   - Character folding\
   Adding up the integer values of all the characters in the string. A better idea is to multiply the hash total by a constant, typically a sizeable prime number, before adding in the next character, ignoring overflow. Using exclusive 'or' instead of add is also a plausible alternative. The final operation would be a modulo, mask, or other function to reduce the word value to an index the size of the table.
    - Not good when information may cluster in the upper or lower bits of the bytes, which clustering will remain in the hashed result and cause more collisions than a proper randomizing hash.
    
   - Word length folding\
     If 8-bit character strings are not hashed by processing one character at a time, but by interpreting the string as an array of 32 bit or 64 bit integers and hashing/accumulating these "wide word" integer values by means of arithmetic operations (e.g. multiplication by constant and bit-shifting). The final word, which may have unoccupied byte positions, is filled with zeros or a specified "randomizing" value before being folded into the hash. The accumulated hash code is reduced by a final modulo or other operation to yield an index into the table. (Mentioned in example 9.8 in Page 329)
     
   - Other hashings\
     Other hashing function for variable-length data shown in wikipedia: Radix conversion hashing, Rolling hash.
    
## Collision resolution techniques 
  Collision resolution techniques can be broken into two classes: **open hashing** (also called **separete chaining**) and closed hashing (also called **open addressing**). The difference between the two has to do with wheather collision are stored outside the table (open hashing), or whether collisions result in storing one of the records at another slot in the table (closed hashing).
  Overview of hash table (Open hashing and Closed hashing) can be found in [wikipedia](https://en.wikipedia.org/wiki/Hash_table#Separate_chaining "hash table").
### Open hashing
  Each bucket is independent, and has some sort of list of entries with the same index. The time for hash table operations is the time to find the bucket (which is constant) plus the time for the list operation.\
  Each bucket has zero or one entries, and sometimes two or three, but rarely more than that. Therefore, structures that are efficient in time and space for these cases are preferred.\
  Illustration of open hash shown in below ![Open hash](https://raw.githubusercontent.com/KV152/Data-Structures-and-Algorithm/master/figures/openhash.png)\
  The most popular implenation technique of open hash is by using linked list store data. All records that hash to a particular slot are placed on that slot's linked list. Records within a slot's list can be ordered in several ways: by **insertion order**, by **key value order**, or by **frequency-of-access order**. Ordering the list by key value provides an advantage in the case of an unsuccessful search, because we know to stop searching the list once we encounter a key taht is greater than the one being searched for.\
  Open hashing is most appropriate when the hash table is kept in main memory, with the lists implemented by a standard in-memory linked list. For open hashing, the worst-case scenario is when all entries are inserted into the same bucket, in which case the hash table is ineffective and the cost is that of searching the bucket data structure. Open hashing also inherit the disadvantages of linked lists. When storing small keys and values, the space overhead of the next pointer in each entry record can be significant. An additional disadvantage is that traversing a linked list has poor cache performance, making the processor cache ineffective.
  
  Other implemation techniques: Open hashing with list head cells, self-balancing binary search tree, array hash table, dynamic perfect hashing.
  
### Closed hashing
  Closed hashing stores all records directly in the hash table. Each record R with key value k<sub>R</sub> has a **home position** that is **h**(k<sub>R</sub>), the slot computed by the hash function. If R is to be insertede and another record already occupies R's home position, then R will be stored at some other slot in the talbe.\
  With this method, a hash collision is resolved by **probing**, or searching through alternate locations in the array (the probe sequence) until either the target record is found, or an unused array slot is found, which indicates that there is no such key in the table.
  - Bucket Hashing\
    The M slots of the hash table are divided into B buckets, with each buket consisting of M/B slots. And an overflow bucket of infinite capacity at the end of the table.
    - The hash function assigns each record to the first slot within one of the buckets.
    - If this slot is already occupied, then the bucket slots are searched sequentially unitl an open slot is found.
    - If a bucket is entirely full, then the record is stored in an overflow bucket of infinite capacity at the end of table.
    - All buckets share the same overflow bucket.
    - Good for implementing hash tables stored on disk, because the bucket size can be set to the size of a disk block. (Entire bucket is read into memory once for inserting or searching.  Minimize the size of overflow to avoid unnecessary )\
    Illustration of bucket hashing ![bucket hashing](https://raw.githubusercontent.com/KV152/Data-Structures-and-Algorithm/master/figures/bucket_hashing.PNG)
  - Variaation of bucket hashing
    Hashing a key value to some slot in the hash table as though bucketing were not being used. If home position is full, then the collison resolution process is to move down through the table toward the end of the bucket while searching for a free slot in which to store the record. If the bottom of the bucket is reached, then the collision resolution routine wraps around to the top of the bucket to continue the search for an open slot.
    - For example, assuming that buckets contain eight records, with the first bucket consisting of slots 0 through 7. If a record is hashed to slot 5, the collision resolution process will attempt to insert the record into the table in the order 5, 6, 7, 0, 1, 2, 3, and finally 4. If all slots in this bucket are full, then the record is assigned to the overfolow bucket.
    - Advantage: Inital collisions are reduced, beacasuse any slotcan be a home position rather than just the first slot in the bucket.
  - Probe sequence\
    With this method a hash collision is resolved by probing, or searching through alternate locations in the array (the probe sequence) until either the target record is found, or an unused array slot is found, which indicates that there is no such key in the table.\
    This sequence of slots is known as **probe sequence**, and it is generated by some **probe fucntion** that we call **p**. probe function **p** has two parameter, the *k* and a count *i* for where we wish to be. For example, to get the first position in the probe sequence after the home slot for key K, we call **p**(K, 1) and get a return which is a offset fron the original home position, rather than a slot in the hash table.  
    - Linear probing\
    In which the interval between probes is fixed — often set to 1. (**p**(K, *i*) = c*i*)
      - Once the bottom of the table is reached, the probe sequence wraps around to the beginning of the table. Linear probing has the virtue that all slots in the table will be candidates for inserting a new record before the probe sequence returns to the home position.
      - The tendency of linear probing to cluster items together is known as **primary clustering**. Small clusters thend to merge into big clusters, making the problem wrose. The objection to primary clustering is that it leads to long probe sequences.
      - Linear probing with a value c > 1 does not solve the problem of primary clusterin.
    - Pseudo-random probing\
      The *i*th slot inthe probe sequence is (**h**(K) + r<sub>*i*</sub>) mod M where r<sub>*i*</sub> is the*i*th value in a random permutation of the numbers from 1 to M - 1. All insertion and search operations use the same random permutation. The probe function is **p**(K, *i*) = Perm[*i* - 1] where **Perm** is an array of length M-1 containg a random permutation of the values from 1 to M - 1.
    - Quadratic probing\
      Interval between probes is increased by adding the successive outputs of a quadratic polynomial to the starting value given by the original hash computation. The probe function is some quadratic funciton: **p**(K, *i*) = c<sub>1</sub>*i*<sup>2</sup> + c<sub>2</sub>*i* + c<sub>3</sub>
      - Under quadratic probing, two keys with different home positions will have diverging probe sequence.
      - Example of Good match of table size and probe function (half of table size is in probe sequence)
        - If the hash table size is a prime number and the probe function is **p**(K, *i*) = *i*<sup>2</sup>
        - If the hash table size is a power of two and the probe function is **p**(K, *i*) = (*i*<sup>2</sup> + *i*) / 2
      - Advantage: Both Pseudo-random probing and Quadratic probing eliminate primary clustering.
      - Disvantage: Not all hash table slots will be on the probe sequence. (Cycle through a relatively small number of slots in some size of hash table.)
      - Disvantage: Both Pseudo-random probing and Quadratic probing can't slove **secondary clustering**. (If the home position is the same, then probe sequence is the same as wll in these two probing algorithm)
      - secondary clustering: Tendency for a collision resolution scheme such as quadratic probing to create long runs of filled slots away from the hash position of keys. 
    - Double hashing\
       Interval between probes is computed by a second hash function. probe seqeunce would be of the form **p**(K, *i*) = *i* * **h**<sub>2</sub>(K).
       - Ensure that all of the probe sequence constants are relatively prime to the table size M. For example:
         - M to be a prime number, and have **h**<sub>2</sub> return a value in the range 1 &le; **h**<sub>2</sub>(K) &le; M -1.
         - M = 2<sup>*m*</sup> for some value *m* and have **h**<sub>2</sub> return an odd value between 1 and 2<sup>*m*</sup>.
       - Eliminate primary clustering and can be combined with Quadratic probing or Pseudo-random probing.
       
Summary of closed hashing (Source from [open addressing in wikipedia](https://en.wikipedia.org/wiki/Open_addressing "open addressing") and [hash table in wikipedia](https://en.wikipedia.org/wiki/Hash_table#Open_addressing "hash table")):
  - The main tradeoffs between these methods are that linear probing has the best cache performance but is most sensitive to clustering, while double hashing has poor cache performance but exhibits virtually no clustering; quadratic probing falls in-between in both areas. Double hashing can also require more computation than other forms of probing. 
  - A drawback of all these open addressing schemes is that the number of stored entries cannot exceed the number of slots in the bucket array. In fact, even with good hash functions, their performance dramatically degrades when the load factor grows beyond 0.7 or so. For many applications, these restrictions mandate the use of dynamic resizing, with its attendant costs
