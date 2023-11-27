# Chapter 3 - B-Trees: The Ideas

## The intuition behind B-Trees and BST

Binary trees are popular data structures for sorted data. Keeping the tree in a good shape after inserting or removing keys is what "balancing" means.

Each node in a B-tree contains multiple keys and multiple links to its children.

```
      [1,   4,   9]
      /     |    \
     v      v     v
[1, 2, 3] [4, 6] [9, 11, 12]
```

B-tree is balanced by the size of the nodes:

- If a node is too large to fit in a single page, it is split into two nodes. It will increase the size of the parent node and the height of the tree.
- If a node is too small, it is merged with a sibling node.

## B-tree and nested arrays

Let's get some intuition about B-trees by comparing them to nested arrays.

Let’s start with a sorted array. Queries can be done by bisection. But, updating the array is O(n) which we need to tackle. Updating a big array is bad so we split it into smaller arrays. Let’s say we split the array into sqrt(n) parts, and each part contains sqrt(n) keys on average.

```
[1, [2, 3], 4, [5, 6, 7]]
```

To query a key, we must first determine which part contains the key, and then do a bisection on that sqrt(n) part. This is O(log(n)). After that, bisection on the part again is O(log(n)). It's no worse than before, and updating is O(sqrt(n)).

## Immutable data structures

Immutable data structures are data structures that cannot be modified after they are created. Aka (append only) (copy on write) and (persistent data structures).

For example, when inserting a key into a leaf node, do not modify the node in place, instead, create a new node with all the keys from the to-be-updated node and the new key. Now the parent node must also be updated to point to the new node.

There are several advantages to immutable data structures:

- Avoid data corruption
- Easy concurrency
