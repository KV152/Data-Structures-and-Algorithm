# Hashing 
## Contents
  - [Overview](#overview)
  - [Hash Functions](#hash-functions)
    - [Integer data](#integer-data)
    - [Variable-length data](#variable-length-data)
## Overview
- Feature
  - The process of finding a record using some computation to map its key value to a position in the array is called **hashing**.
  - The function that maps key values to position is called a **hash function** and will be denoted by **h**.
  - The array that holds the records is called the **hash table** and will be denoted by **HT**.
  - A position in the hash table is also known as a **slot**.
  - The number of slots in hash table HT will be denoted by the vairable M, with slots numbered from 0 to M-1.

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
     
   - Other hashing\
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
     
     - Other hashing\
    Other hashing function for variable-length data shown in wikipedia: Radix conversion hashing, Rolling hash.
    
## Collision resolution techniques 
  Collision resolution techniques can be broken into two classes: **open hashing** (also called **separete chaining**) and closed hashing (also called **open addressing**). The difference between the two has to do with wheather collision are stored outside the table (open hashing), or whether collisions result in storing one of the records at another slot in the table (closed hashing).
### Open hashing
### Closed hashing
     
