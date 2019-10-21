# I. Java Refresher: Java SE

## Prerequisite

[Java Programming Basics | Udacity](https://www.udacity.com/course/java-programming-basics--ud282)

## Overview of Basic APIs

Math: [https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html)

Date: [https://docs.oracle.com/javase/8/docs/api/java/util/Date.html](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html)

String: [https://docs.oracle.com/javase/8/docs/api/java/lang/String.html](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)

Character: [https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html)

## String Builder VS String Buffer

StringBuilder: [https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html)

StringBuffer: [https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html)

[StringBuilder class in Java with examples](https://www.geeksforgeeks.org/stringbuilder-class-in-java-with-examples/)

[String VS StringBuilder VS StringBuffer in Java](https://www.journaldev.com/538/string-vs-stringbuffer-vs-stringbuilder)

1. String is immutable in java - whenever we do String manipulation like concatenation, substring etc, it generates a new String and discards the older String for garbage collection
2. These are heavy operations and generate garbage in heap. So Java has provided StringBuffer and StringBuilder class that are used for String manipulation
3. StringBuilder and StringBuffer are mutable objects, providing api such as append(), insert(), delete() and substring() methods to manipulate string. With mutable objects.
4. An important difference between StringBuffer and StringBuilder is that each method of StringBuffer is synchronized (it can't be accessed by multiple threads at once, but can only be accessed by one thread at a time) which makes StringBuffer thread safe. On the other hand, StringBuilder is not thread safe. Multiple threads can access its methods at the same time. This makes StringBuilder faster than StringBuffer.

## Big O Notation

It's used to talk about how long an algorithm takes to run.

[Time Complexities and Examples](https://www.notion.so/c806b9a801ad48fc9230357a7e3163b9)

## Collections: List

One problem with an array is, once the size is defined, you will have to stick with the size. Therefore, you need your array to have a flexible size like List. List is an interface, the two common List implementations include:

- ArrayList
- LinkedList

**Difference between ArrayList and LinkedList**

- **ArrayList is based on a data structure called dynamic array**, which overcomes the problem of having a fixed size that needs to be known at the allocation time. The dynamic array can have variable size, it allows the addition and deletion of elements even after the array is declared.
- Dynamic array uses fixed-size arrays. It allocates a larger than the initial number of elements space for a fixed-size array; if new elements need to be added, they can go into the extra available spaces that were allocated. If more elements need to be added and no additional spaces are available, a new larger array will be allocated, all elements will be copied over to the new array. Overall, this is an expensive process in terms of performance.
- LinkedList use doubly-linked list (DLL) to store elements. DLL is a list where each node has two pointers, one points to the previous node, and the other points to the next one.
- The first element in the list is called the head and its previous pointer points to Null, while the last element is called the tail and its next pointer points to Null.

ArrayList: [https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)

LinkedList: [https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html)

Comparison between ArrayList and LinkedList:

**Element Retrieval**

1. ArrayList:

- Performance: Better
- Reason: it can retrieve an element by its index
- Big O: O(1)

2. LinkedList:

- Performance: Worse
- Reason: it needs to start searching for an element from the head and keep checking the next element until it finds it
- Big O: O(n)

**Element Insertion / Deletion**

1. ArrayList:

- Performance: Worst
- Reason: This can require allocating a new array in the memory and copying the elements to it
- Big O: O(n)

2. LinkedList:

- Performance: Better
- Reason: for element addition, it simply needs to allocate a new space in the memory for the new element and set its "next" and "previous" pointers to the elements preceding it and following it.
  It will also set the "next" pointer of the preceding element to point to the new element, and set the "previous" pointer of the following element to point to the new element. All these operations are not expensive and they require a constant time.
- Big O: O(1)
