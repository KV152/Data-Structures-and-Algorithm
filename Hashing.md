# Hashing 
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
    - Not good to visiting the records in key order.
  - Pros
    - Good for accessing involves only exact-match queries.
    - Good for both in-memory and disk-based searching.
    - One of two most widely used methods for organizing large databases stored on disk (the other is B-tree).
 
- Collision

  Typically, there are many more values in the key range than ther are slots in the hash table. At least some of the slots must be mapped to from multiple key values. Given a hash fucntion **h** and two keys k<sub>1</sub> and k<sub>2</sub>, if **h**(k<sub>1</sub>) = &beta; = **h**(k<sub>2</sub>), where &beta; is a slot in the table, then we say that k<sub>1</sub> and k<sub>2</sub> have a collision at slot &beta; under hash function **h**.  
  Finding a record with the key value K in a database orgranized by hasing follows a two-step procedure:
  1. Compute the table location **h**(K).
  2. Starting with slot **h**(K), licate the recored containing key K (if necessary) using a collision resolution policy.

  Perfect hashing is a system in which records are hased such that there are no collisions. Perfect hashing is efficient but selecting perfect hash function can be expensive. It might be worthwhile when exremely efficient search performance is required. (Searching for data on a read-only CD, where database will never be changed and time for each access is expensive.)

## Hash Functions 
  Goal: Sotres the actual records in the collection such that each slot in the hash table has equal probability of being filled.
  However input data values might be prooly distributed.
  - Natural frequency distributions tends to follow a common pattern where a few of the entities occur frequently while most entities occur relatively rarely. (population in country)
  - Collected data are likely to be skewed in some ways. (price might be end in 0 5 or 9)
  - If the inpout is a colleciton of common English words, the begninning letter will be poorly distributed.
  
 Hence, two situations faced when we designed hash functions
 - Knowing nothing about hte distribution of the incoming keys. 
    - Aim to desing evenly ditribution hash function.
 - Knowing something about the distribution of the incoming keys. 
    - Distribution-dependent hash function is requried to aviod assigning clusters of related key values to the same hash table slot.
