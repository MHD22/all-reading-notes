##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: Linked Lists

#### ______________________________________________________


## What is a Linked List

* A Linked List is a sequence of Nodes that are connected/linked to each other. 
* The most defining feature of a Linked List is that each Node references the next Node in the link.
* There are two types of Linked List - Singly and Doubly.

## Terminology:

1. Linked List - A data structure that contains nodes that links/points to the next node in the list.
2. Singly - Singly refers to the number of references the node has. A Singly linked list means that there is only one reference, and the reference points to the Next node in a linked list.
3. Doubly - Doubly refers to there being two (double) references within the node. A Doubly linked list means that there is a reference to both the Next and Previous node.
4. Node - Nodes are the individual items/links that live in a linked list. Each node contains the data for each link.
5. Next - Each node contains a property called Next. This property contains the reference to the next node.
6. Head - The Head is a reference of type Node to the first node in a linked list.
7. Current - The Current is a reference of type Node to the node that is currently being looked at. When traversing, you create a new Current variable at the Head to guarantee you are starting from the beginning of the linked list.

#### ______________________________________________________

## what makes arrays and linked lists different?:

### Memory management

* The biggest differentiator between arrays and linked lists is the way that they use memory in our machines. 
* When an array is created, it needs a certain amount of memory(all in one place ).
* On the other hand, when a linked list is born, it doesn’t need bytes of memory all in one place. One byte could live somewhere, while the next byte could be stored in another place in memory altogether!

* arrays are **static data structures.**
* linked lists are **dynamic data structures.**

#### ______________________________________________________

## Parts of a linked list:

* A linked list is made up of a series of nodes, which are the elements of the list.
* The starting point of the list is a reference to the first node, which is referred to as the `head`.
* Each node has just two parts:
    1. data.
    2. a reference to the next node.

> *A node only knows about what data it contains, and who its neighbor is.*


*A good rule of thumb for remember the characteristics of linked lists is this:*

> *a linked list is usually efficient when it comes to adding and removing most elements, but can be very slow to search and find a single element.*







#### ______________________________________________________

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.