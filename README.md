# RECRACKING THE CODING INTERVIEW

## Big(O) complexity
![alt text](https://danielmiessler.com/images/big-o-chart-tutorial-bazar-aymptotic-notations-1.png "Big O complexity")

>Do not use process of elimination. When confused about runtime, DERIVE, DONT GUESS!

>Runtime could be O(N*K) being for example N = length of array and K = number of pairs

### Master Method

When facing iterative algorithms that constantly subdivide the problem in constant sized subproblems we can infer the runtime complexity with a very accurate level of precission. *Here is the technique*:

> We first need to gather a set of variables


* A: Number of recursive calls we make inside our function.(*2 in merge sort*)
* B: Factor by which we shrink the problem.(*2 in merge sort*)
* D: Exponent in summing time of the "combine step".(*1 in merge sort*)

> Translate to big O

![](https://gdurl.com/DCUXq)

## Optimizations and Solve Techniques
### 1 BUD
#### Bottlenecks
* * Example of a two steps algorithm with a O(N log N) & O(n^2) 

> Given an array of distinct integer values, count the number of pairs of integers that have a difference of k. For example, given the array
```[1, 7, 5, 9, 2, 12, 3]``` and the difference ```k=2``` then  four pairs are possible (1,3) (3,5) (5,7) (7,9);
> > Throw everything into the hashtable, look up if x+k or x-k exists ---> O(n);

#### Unnecessary Work

> Print all the solutions to the equation ````a^3 + b^3 = c^3 + d^3 ````
> > Naive: O(n^4) VS Optimized = O(N^3) Just isolating d in the equation the following way

From:
```
for a from 1 to n
  for b from 1 to n
    for c from 1 to n
      for d from 1 to n
        if(a^3 + b^3 == c^3 + d^3 ) print a, b, c, d
        break
```
To:
```
for a from 1 to n
  for b from 1 to n
    for c from 1 to n
        d = power(a^3 + b^3 -c^3, 1/3)
        if(a^3 + b^3 == c^3 + d^3 ) 
        print a, b, c, d
```

#### Duplicated Work

We can compute for every c,d pair its sum(USE HASHTABLE), and because a,b will iterate through the same elements the solution will also be in the same hashtable

> Note that a,b & c,d all iterate through 1 to n

| c , d  | sum          
| ------ |:---:|
| 5,2    | 7   |  <<< For this pair A (c, d)
| ...| ...|
| 4,3 | 7 | <<< This (a,b) is one solution with A
| 6,1 | 7 | <<< This (a,b) is other solution with A

Keep doing this for every element in table and time will be reduced to O(n^2)


### 2 DIY

* Apply it to real life example 
* Reverse engineer your solution
* Bigger example will be easier
* Be aware of in-brain optimizations

### 3 Simplify & Generalize

* Multistep approach
* * Tweak some constraint 
* * Solve for the simplified version
* * Adapt it to more complex version

### 4 Base Case & Build

* Start for ```n = 1```
* Use prior result to build up

> Example: Print all permutation of a given string. Assume all characters are unique

Analysis:

| N  | String | POSSIBLE PERMUTATIONS | Step           
| ------|:-----:|:---: |:---: |
| 1      |  a   | ["a"] | Base case
| 2      |  ab   |["ab", "ba"] | Insert b into all locations of all strings in last step
| 3      |  a   | ["cab", "acb", "abc"] ["cba", "bca", "bac" ] | Insert b into all locations of all strings in last step

>For example if we wanted to know all the permutations of "hola"(Let's call it Sn) 
we would chop off the last character and generate all permutations of Sn-1
Once we have all the list of Sn-1 permutations we just add the first chopped off character of Sn to every position in every string of Sn-1.


### 5 Data Structure Brainstorm

* Run through a list of DS in a similar way to this:

The probles is the following

> Numbers randomly generated and stored in an array. Keep track of median.

| DS | ✅ |❌
|---|---|---|
| Array | · Quick element lookup | · Keep elements sorted 
| Linked list | · Flexible |  · Sorting & Accessing
| Binary Tree | · **If balanced the top might be median, if # of nodes is odd, average two middle elements **  | 
| Heap | Track Max and Mins |

--- 

### Best Concievable Runtime

* Think about what would be the minimum number of operations you would need to perform to achieve the desired output
* When doing so, **notice** that sometimes having 2 for arrays in which you should compare or look for elements in the arrays that match a certain characteristic, it does not necessarily mean that you have to go over the elements in both arrays.
In fact, look at the following example in which we can take advantage of the fact that arrays are the same length and ordered to find number of common elements in array A and array B:

```Array A[13, 27, 35, 40, 49, 55, 59]```

```Array B[17, 35, 39, 40, 55, 55, 59]```

We just need to do the following:

* "Point" at beginning of both arrays and pick one as *pivot*, lets say we pick ```A```
* * At this point 13 is pointed in Array A and 17 is pointed in Array B
* Next we check in B if the element to be compared, 17, **is bigger** than the currently pointed element in A, 13. Since it is bigger, it means that we will not find any other 13's after 17 since both lists are in ascending order.
* * Pointer is moved in Array A to point to 27.
* * Pointer in B stays the same 
* Next we repeat the **is bigger** comparisson between 27 from A and 17 from B. In this case the answer is negative so we move the pointer in B to keep looking for candidates that match the number 27 in Array B.
* We just repeat this process until we reach the end in array A.
* **Notice that when we reach the end of A, the value 60 from B will never be taken into account.**

# INTERVIEW QUESTIONS

> After the advice I got from my mentors and what I have read online, I would suggest to divide your time in the following way:

|First cover:|
|---|
|Arrays & Strings|Linked Lists|Stacks & Queues|
|Trees & Graphs|Bit manipulation| Math & Logic puzzles|
|Sorting and searching|Recursion and dynamic programming|

|If there is time cover:|
|---|
|Object Oriented Design| System Design and Scalability | Testing |
|Java|Databases|Threads & Locks|

## 1.Arrays & Strings

* Questions about Arrays and Strings are often **interchangeable**
* **Hash tables** are often used in these types of problems to make efficient lookups. 

Let's then recall how Hashtables work:

### Hashtables using an *array of linked lists*.

Inserting Peter...

| Key(Name)     | Value(Tel)     |  Key's Hashcode    | Key's hashcode maps to index...|
|---------------|----------------|--------------------|------------------------------|
|Sandra Dee     | 521-9655       | 356661085722046646 | 2

* This originates two levels of **Conflict**
* * Two different keys can have the same hashcode!
* *  Two different hashcodes could map to same index!

**After Sandra's insertion**, the possible Hashtable implemented using lists would look like the following:

![alt text]( http://colenak.ptkpt.net/_buku_manual/_baca_blob.php?book=lain&kodegb=500px-Hash_table_5_0_1_1_1_1_0_LL.png "Hashtables using linked lists example")

When trying to **look up** Sandra's telephone number:
* The **worst** lookup time is ***O(N)*** HAVING nothing else but collisions, It would be like looking for the value in a linked list.
* The **best** case scenario, which is by far more common is ***O(1)***. We would only need to compute the hashcode for the key, in this case *Sandra Dee* and then use that to retrieve the value from the bucket in position 001.

### Hashtables using a ***balanced binary search tree***
|Advantages ✅| Disadvantantages ❌|
|-------------|--------------------|
|Less space usage| The lookup time increases to **O(log(N))**|
|We can iterate through the keys in order|


In the Sandra´s insertion case example, because there was another element in the index mapped from the hash generated by **Sandra  Dee** the conflict
had been resolved with the linked list itself, just adding another entry after "John Smith".
However, there are some other ways in conflict resolution in hashtables can be achieved.
### Collision Resolution in Hash Tables

> **Collision** : Conflictive insertion situation that happens when there is already an item stored at the designated index

There are some techniques to avoid collision

#### Chaining with Linked list --> Seen in Sandra´s example.

#### Open Addressing with Linear Probing

When a collision happens for a particular index we just make the insertion in 
**another index at some distance from the index the element was first intended to be 
in or until we find a free space**

* ✅ If the number of collisions is low this is fast and space efficient.
* ❌ The total number of entries is limited to the array size 
* ❌ Clusterig can occur

> **Clustering** : When we apply the "next free spot strategy" we can encounter that adjacent indexes can be occupied and then increment the probability for the next potential next index to be occupied since when the insertion is done it will have more chances to happen to be the next adjacent having some latter occupied indexes. This is a problem because **It will take logarithmic time to search for the keys within that cluster**

#### Quadratic Probing and Double Hashing 

* If we choose the "some distance approach" we can see that this distance can be incremented quadratically.
* We could also use another hash function to determine the distance.
---

### ArrayLists 

> If we want in JAVA a dynamic array in which the lookup time is still O(1), as in an array. We will use arraylists

```ArrayList<Data type> name = new ArrayList<Same data type>();```

* An array list **resizes itself in O(N) time**, *but* is so uncommon that **insertion time is still O(1)**

#### String Builder

There is a very powerfull object in Java that allows us to build strings in very efficient ways, It's called String Builder.

Let's consider this ***example***. 
> We have an array of strings (of the same length let's say x, to simplify) and we want to return a unique string formed by the words in the array.

A common approach would be something like this:

```
String joinWords(String[] words, String[] more) {
  String sentence = "";
  for(String w: words) sentence = sentence + w;
  return sentence
}
```

* The 1st copying takes x characters
* The 2nd copying takes 2x characters
* The 3rd copying takes 3x characters
* ...
* The nth copying takes n*x characters

This **results in O(x*n^2)** because:

Taking out x as common factor:

```1 + 2 + ... + n``` 

is equal to:

```
n*(n-1)/2
```

 which getting rid of constants in O() notation results in:


```
O(n^2)
```

and replenishing the first taken out element x:

```
O(x*n^2)
```

This time is quite inefficient. This is where StringBuilder comes to be handy:

```
String joinWords(String[] words, String[] more) {
  StringBuilder sentence = new StringBuilder();
  for(String w: words) sentence.append(w);
  return sentence.toString();
}
```
This works because Stringbuilder makes a resizable array **copying it back to a String only when necessary**.

Some ***useful methods of Stringbuilder*** are:

|Method |Functionality|
|-------|-------------|
|**delete(int start, int end)**| Removes/delete the characters in a substring of this sequence.| 
|**deleteCharAt(int index)**   | Removes the char at the specified position in this sequence.
|**indexOf(String str)**    | Returns the index within this string of tReturns the index within this string of the first occurrence of the specified substring, starting at the specified index.he first occurrence of the specified substring.
|**indexOf(String str, int fromIndex)**|Returns the index within this string of the first occurrence of the specified substring, starting at the specified index.
|**lastIndexOf(String str)**|Returns the index within this string of the rightmost occurrence of the specified substring.
|**lastIndexOf(String str, int fromIndex)**|Returns the index within this string of the last occurrence of the specified substring.
|**reverse()**|Causes this character sequence to be replaced by the reverse of the sequence.
|**replace(int start, int end, String str)**|Replaces the characters in a substring of this sequence with characters in the specified String.

### Additional String Trick: Rabin-Karp Substring Search

Having two arrays B and S, big and small
The goal is to avoid building a looking window of size S going through all B because it would take O(s*(b - s))

> **The trick is the following: If two strings are the same, they must have the same Hash value.**

Imagining that our hashfunction was the  sum of each character (space = 0, a = 1, b = 2...) 

|CHAR: | d| o | e|   | a | r | e |   | h | e |a|  r | i | n| g|  | m | e 
|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|
|Code: | 4| 15 | 5| 0 | 1 | 18 | 5 | 0  | 8 | 5 | 1 | 18 | 9| 14| 7| 0| 13 | 5 
|Sum of next  3: |**24**|20|6|19|**24**|23|13|13|14|**24**|28|41|30|21|20|18| | |

Then we go through the hashes comparing it to the hash of the word we want to find which in this case is ear.
**And only when we have a match, only then we compare characters**, that is why RK algorithm is best than the naive approach.

## 2 Linked Lists

This guide will skip implementation, assuming familiarity with **single linked lists** and 
**double linked lists**, which in both cases can be circular if
the last node of the list points to the head of the list.

### Characteristics

|Advantages ✅|Disadvantages ❌|    
|----|------|
|Insertion | O(n) lookup time|
|Deletion  | 

### Runner Technique

The **idea** behind the runner technique is simple

> use two pointers that either move at different speeds or are a set distance apart and iterate through a list.

**Where do you use it?** 

If you get handed a linked list question, and you find yourself asking these questions:

* How do I figure out where these **two things meet**?
* How do I figure out the **midpoint**?
* How do I figure out the **length**?

**Examples** where this types of questions come up are:

* Given two lists of different lengths that merge at a point, determine the merge point
* Determine if a linked list contains a loop
* Determine if a linked list is a palindrome
* Determine the kth to *last* element of a linked list 

### Let's analyze the **MERGE problem**:

There are two ways to find the merge point.

*  ***Simultaneous rotating pointers***: Make simultaneous pointers in both lists and make them advance until they find null, when that happens make them start again but in the head of the other linked list. They will keep moving until they meet at mergepoint.

This solution makes sense if we think about it this way:

  * List A has size X with a size of a until mergepoint and distance of c until its end
  * List B has size X with a size of b until mergepoint and distance of c until its end
  
        X = a + c
        Y = b + c
  If we make the pointers go around once they have finished they both will cover the same lenght:

      //Distance covered by pointer in list A
      a + c + b
      //distance covered by pointer in list B
      b + c + a
  
  > We can see that both distances are the same, wo both pointers will meet at same node finding this way the merge point.


*  ***Find delay in lists***: The idea is to find how many extra nodes one of the lists has before reaching the merge point.

  Imagine we have:
  * a list A of size 5: [1]-[2]-[3]-[4]-[5]
  * a list B of size 6: [20]-[21]-[2]-[3]-[4]-[5]
  
  The lists merge in node holding number 2
        
---------[1]-[2]-[3]-[4]-[5]

  [20]-[21]-⬆️(merge)
  
  We can see that node 2 is the merge node.
  
  We can see that after the merge node the number of nodes in both lists is the same since they share the nodes.
  Using this we can find the delay in one of the pointers to correct it.
  
  In this case the delay is ```delay = 6 - 5  = 1```
  
  So we give a headstart the pointer in the longer list, list B so that both pointers will now meet at node holding number 2.
  If we didnt do this, pointer when pointer in shorter lists was in node [2] the pointer in b would be stil in [21] and the mergepoint would not be found.

### Let's analyze the LOOP detection

To answer **if there is a loop**:
  
  * We assign 2 pointers(p & q) to the start of the list 
  * We advance p by 1 node
  * We advance q by 2 nodes
  * IF THEY MEET, there is a loop

To anwer **Where is the start** of the loop:

  * We keep one poniter in node where they met in previous algorithm
  * We put other pointer in start of list again
  * We increment EACH pointer by 1. 
  * The node in which they meet will be the start of the loop
  
### Kth to LAST element

Now we try to find the nth to last element of a singly linked list.

[Link to Stack Overflow question](https://stackoverflow.com/questions/2598348/how-to-find-nth-element-from-the-end-of-a-singly-linked-list)

> If the elements are ```8->10->5->99->2->1->5->4->12->10``` then the result is  7th to last node is 99.

We just use two pointers, 
* one located in the first node with data value 8
* Other on 7th node with data value 4

* We increment both pointers by one until second pointers reaches end.
* When that happens the other pointer will be pointing to node [5] 
* And so result is node to [5]'s right which is 7 and is the expected result. 👍

## 3 Stacks & Queues

Assuming familiarity with both stack and queue data structures, applications not 
as intuitive as traditional academic ones will showed in this chapter.

### Stacks

***LIFO*** data structure. 

Non-naïve applications:

* Certain *recursive algorithms* : sometimes it is necessary to
 store data in order as we backtrack in the algorithm.

* Make an iterative algorithm from a recursive one.


### Queues

***FIFO*** data structure.

Non-naïve applications:

* **Breadth-first search**: A queue is used to store a list of tyhe nodes that we need to process. Every time a node is processed its adjacent nodes are added to the back of the queue. 
* Implementing a **cache**.
* Printing **tree level by level**.

## 4 Trees & Graphs

### Trees

> Make sure you can implement a tree from scratch

Composed of:

* **Nodes**
* **Children of each node**
* **No cycles**

Depending on the number of children that nodes can have we can have:

* binary trees
* ternary trees
* etc...

#### Binary Search Trees

Binary tree in which:

> Every node fits a specific order property.

***E.g***

* All **left** nodes have to be **<**
* All **right** nodes have to be **>**

> Note thatThis property applied to a node  **MUST BE TRUE NOT ONLY FOR IMMEDIATE CHILDREN BUT FOR ALL DESCENDENTS**

Definitions:

* **COMPLETE Binary Tree**: Every level is filled. Last level is the only level that can be unfilled as long as it is filled form left to right
* **FULL Binary Tree**: Every node has 0 ot 2 children.
* **PERFECT Binary Tree**: Both **FULL** & **COMPLETE**

##### Binary Tree Traversal

* **In order**: Left-Parent-Right
* **Pre order(Parent first)**: Parent-Left-Right
* **Post order(Parent last)**: Left-Right-Parent

##### Balancing trees

All of the ordering works if a tree is balanced, which means that we can assure O(log(N)) times for insertion and lookup.

**AVL trees**

Technique:

> Balanced node in AVL means: left to right to see if anumber of descendets to the right of a node differ in maximum 1 node with maximum descendets to the left.

* Check bottom up if the nodes are balanced.
* If a node is unbalanced make the necessary rotation 
* Recheck again for every node until all nodes are balanced.

#### Tries

> A trie is an n-ary tree in which **CHARACTERS** are stored at each node. Each path down the tree represents a word.

**Application** ➡️ Word prefix lookup. **O(K) time**

Same time as hashtable but this can also **TELL US IF A PREFIX IS A VALID ONE FOR ANY STORED WORD**.

### Graphs



## 5 Bit Manipulation

The first thing to take into account is that logical bit operators work bit by bit.

This are the ones we will combine and use:

* AND **&** 
* OR **|**
* XOR **^**
* NOT **~**

### Bit Tricks

* consider X to be a binary number
* consider 0s to be a chain of 0s
* consider 1s to be a chain of 1s

|   |   | AND | OR | XOR |
|---|---|-----|----|-----|
X| 0s| 0s|X|X
X| 1s| X|1s|~X
X| X| X|X|0s

### 2s Compliment

#### Positive number

Making -6 from 6

* 6

|Sign bit|
|--------|
|    0   | 1 | 1 | 0

the 2s compliment is done the following way:

* Change 0s for 1s and 1s for 0s

|Sign bit|
|--------|
|    1   | 0 | 0 | 1

* Then, add 1 add we obtain **the 2s compliment number of 6**.

|Sign bit|
|--------|
|    1   | 0 | 1 | 0

> **Fun Fact**: If we only look at 

### Arithmetic(>>) vs Logical Shift(>>>)

* The  logical shift shift values putting zeros in the spaces left blank.
* The arithmetic shift shifts values putting the ***sign*** in the spaces left blank.

| Arith >> | Logical >>> |
|----------|-------------|
|sign in blank spaces| 0s in blank spaces|
|dives by 2| It is just a logical op.|

There are several ways we can use this to affect or get info about a sequence of bits.

#### Get bit

We just want to know if the index in index i was a 1 or not 

*X = 101101001*

*i = 3*

X |1|0|1|1|0|1|0|0|1|
|-|-|-|-|-|-|-|-|-|-|
operation |&|&|&|&|&|**&**|&|&|&|
|mask|0|0|0|0|0|**1**|0|0|0|
|result|0|0|0|0|0|**1**|0|0|0|

* We obtain 1(TRUE) as a result which means there was a 1 in index i.
* We obtain 0(FALSE) as a result which means there was a 0 in index i.

        boolean getBit(int num, int i){
          return(num & (1 << i)) != 0); 
        }

#### Set bit

We just want to know "power on" the bit in index i

*X = 101101001*

*i = 3* 

X |1|0|1|1|0|1|0|0|1|
|-|-|-|-|-|-|-|-|-|-|
operation |OR|OR|OR|OR|OR|**OR**|OR|OR|OR|
|mask|0|0|0|0|0|**1**|0|0|0|
|result|1|0|1|1|0|**1**|0|0|1|

* If the original was a 1 we just leave it on.
* If the original was a  0 we "power it on" to 1.

        int setBit(int num, int i){
          return num | (1 << i); 
        }
        
#### Clear bit

We just want to know "power off" the bit in index i

*X = 101101001*

*i = 3* 

X |1|0|1|1|0|1|0|0|1|
|-|-|-|-|-|-|-|-|-|-|
|mask|0|0|0|0|0|1|0|0|0|
|**~mask**|1|1|1|1|1|**0**|1|1|1|
operation |&|&|&|&|&|**&**|&|&|&|
|result|1|0|1|1|0|**0**|0|0|1|

* If the original was a 1 we turn it off to 0.
* If the original was a  0 we leave it off as it is.

        int clearBit(int num, int i){
          return num & ~(1 << i); 
        }

#### Update bit

We just want to know indicate if we want a 1 or a 0 and the position where we want it.

*X = 101101001*

*i = 3* 

*bitIs1 = true*

|X             |1|0|1|1|0|0|0|0|1|
|-             |-|-|-|-|-|-|-|-|-|
|**~mask**     |1|1|1|1|1|**0**|1|1|1|
|operation     |&|&|&|&|&|**&**|&|&|&|
|clearedPos    |1|0|1|1|0|**0**|0|0|1|
|operation     |OR|OR|OR|OR|OR|**OR**|OR|OR|OR|
|valueMoved    |0|0|0|0|0|**1**|0|0|0|
|**result**        |1|0|1|1|0|**1**|0|0|1|


* If bitIs1 is true we set the bit in position i to 1.
* If bitIs1 is true we set the bit in position i to 0.

        int updateBit(int num, int i, boolean bitIs1){
          int value = bitIs1 ? 1 : 0;
          int maskNegative = ~(1 << i);
          int clearedPos = (num & mask)
          int valueMoved = value << i;
          return clearedPos | valueMoved;
        }

## 6 Math and Logic Puzzles

### Primality

To check if a number *n* is prime or not we dont need to go from 2 up to *n*. 
It will only be necessary to go up to *sqrt(n)*.

This happens because factors repeat each other after the square root of the number

|FACTORS OF 16|
|---------------|
|Forwards|**1**|**2**|**4**|8|16|
|Backwards|**16**|**8**|**4**|2|1|

We can see that the numbers that are not marked are just permutations of previous combinations.

### Sieve of Eratosthenes

Finds a list of the prime numbers up to a number given.

* First makes the list up to n
* Then crossed out the values that can be divided by 2
* Then picks the next non crossed number(everytime will be a prime number)
* Then crosses out the numbers that can be divided by the number that has just been picked


## 7 Sorting & Searching

### Sorting

> Let's go through all the sorting algorithms and review their runtimes and space complexities.


|Algorithm     |Average(t)|Worst(t)|Space|Hint|
|-             |-|-|-|-|
|Bubble        |O(N^2)|O(N^2)|O(1)|Compare element and move if necessary
|Selection     |O(N^2)|O(N^2)|O(1)|Each time search for smallest
|Merge         |O(Nlog(N))|O(Nlog(N))|O(N)|Split and orderly merge arrays
|Quick         |O(Nlog(N))|O(N^2)|O(Nlog(N))|Choose pivot and order elements to its right and left
|Radix         |O(KN)|O(KN)|-|Make array representing the possible numbers to be ordered (**K represents that**) and fill that array with the number of occurrences of each element

### Searching

We could argue that there are other algorithms apart from **binary search**, making use of graphs. But that has been covered in the graphs and trees section of this summary.
Trying to keep it simple and easily generalizable we can recall binary search.

#### Iterative BS

```
int binarySearch(int [] a, int x){
  int low = 0;
  int high = a.length-1;
  int mid;
  while(low <= high){//too small
    mid(low+high) / 2;
    if(a[mid] < x){
      low = mid + 1;
    }else if(a[mid] > x){//too large
      high = mid - 1;
    }else{//FOUND!!
      return mid;
    }
  }
}
```

#### Recursive BS
```
int binarySearchRecursive(int[] a, int x, int low, int high) {
  int mid = (low + high) / 2;
  if(a[mid] < x){//too small
    return binarySearchRecursive(a, x, mid+1, high);
  }else if(a[mid] > x){//too large
    return binarySearchRecursive(a, x, low, mid-1);
  }else{//FOUND!!
    return mid;
  }
}
```

# Sources

My idea behind building this guide was to go over <a href="https://everipedia.org/wiki/lang_en/cracking-the-coding-interview">Cracking the Coding Interview</a> by <a href="https://everipedia.org/wiki/lang_en/gayle-laakmann-mcdowell">Gayle Laakmann McDowell
</a> and try to make it as compact as possible. I also added information that I thought would be useful writing down after figuring out some programming problems solutions.
For instance, I stumbled upon the Master Method to find runtimes of recursive algorithms while doing an <a href="https://www.coursera.org/learn/algorithms-divide-conquer/home/info">online Stanford course</a> by Professor <a href="https://www.coursera.org/instructor/~768">Tim Roughgarden</a>.

# Acknowledgements

 Special thanks to
* My helpulf mentor <a href="https://www.linkedin.com/in/pedromartinezlop3z/">Pedro Martínez López</a>
* My friend <a href="https://www.linkedin.com/in/isabella-ram%C3%ADrez-14a8a0123/">Isabella Ramirez borrero</a>
* The entire <a href="https://spaniardstosv.org/team.html">Spaniards to Silicon Valley team</a>

# Other resources

Feel free to go over all my snippets <a href="https://snippets.cacher.io/user/criskgl">here</a>

---

I sincerely hope this guide becomes useful for anyone trying to find guidance on how to prepare for an interview at any Tech Company.
