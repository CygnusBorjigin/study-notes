# Binary Heap

A binary heap a data structure that is structured as a binary tree. In that each node in a binary heap can have at most two child nodes. There are two main types of binary heaps. In a *min-heap* each node is lesser than or equal to both of its child nodes. Therefore, the minimum value in the tree is always stored at the root of the heap. In a *max-heap* each node is greater then or equal to both of its child nodes. Therefore, the maximum value is always stored at the root of the tree. 

Two of the most common action taken on a binary heap are insertion and deletion. 

## Insertion

When preforming insertion, we need to first preserve the shape of the heap and second preserve the invariant of the heap. 

In terms of preserve the shape of the heap, we always want the heap to be balanced. I.e. each node should have as much child node as it is allowed. Since it is rare for a perfectly balanced binary heap to occur, we often allow the left sub-tree of a node to have one more node than the right sub-tree of the node. 

To preserve the invariant in the insertion, after a node is blindly inserted to the bottom of the heap, we check it with its parent node. If their relationship does not comply with the invariant, we swap those to nodes and repeat the checking process. The stop the process as soon as the invariant is satisfied. Therefore, in the worst case, the number of swap is equal to the height of the tree. 

## Deletion

Just as what we did for insertion, we also need to preserve both the shape and the invariant of the heap during the deletion. 

In term of preserving the shape of the heap, since we only have access to the root node of the tree, that is the only node we can delete. However, after the deletion of the root node, the first level of nodes will be left without a parent and therefore breaking the shape of the tree. To solve that, we bring a node from the bottom level to the root. The node we choose to be the new root, should be chosen in a way that is keep the tree balanced. 

Therefore, if the binary heap is balanced before the root is deleted, we choose the left most node from the right sub-tree to the root. And if the binary heap is not balanced before the root is deleted, we choose the right most node from the left sub-tree to the root. 

Now the shape is fixed, we need to repair the heap to comply with the invariant. To do this, we check if the root node is in compliance with the invariant. If it is not, we choose to swap it with the appreciate the child. We repeat this process until the heap satisfies the invariant.

## Example code for a Min Heap

First, we initialize the heap

```python
class MinHeap:
    def __init__(self, capacity):
        self.storage = [0] * capacity
        self.capacity = capacity
        self.size = 0 # the number of element currently in the heap
```

### Helper Function

Then, we move to the first helper function we need, which is functions that calculate the index of the parent and child node. Those function can be simply translated from the formulas given. 

```python
def getParentIndex(self, index):
    return (index - 1) // 2
    
def getLeftChildIndex(self, index):
    return 2 * index + 1
    
def getRightChildIndex(self, index):
    return 2 * index + 2
```

Now, we have to define is a node has a parent, left child, and right child.

```python
# Simply checking if the index of the supposed parent exists in the list
def hasParent(self, index):
    return self.getParentIndex(index) >= 0 
    
# Simply checking if the index of the supposed child is in the list
def hasLeftChild(self, index):
    return self.getLeftChildIndex(index) < self.size
    
def hasRightChild(self, index):
    return self.getRightChildIndex(index) < self.size
```

Then, define the helper function to fetch the data from the parent, left child and the right child. 

```python
def parent(self, index):
    return self.storage[self.getParentIndex(index)]
    
def leftChild(self, index):
    return self.storage[self.getLeftChildIndex(index)]
    
def rightChild(self, index):
    return self.storage[self.getRightChildIndex(index)]    
```

Finally, we need helper function to determine if the heap is full and to help swap the nodes in the heap.

```python
def ifFull(self):
    return self.size == self.capacity

def swap(self, index1, index2):
    temp = self.storage[index1]
    self.storage[index1] = self.storage[index2]
    self.storage[index2] = temp
```

## Main Methods

First, we need an iterative insert method. We need to check if the heap is full, then do the insert and heapfication steps.

```python
def insert(self, data):
    if (self.isFull()):
        raise("Heap is Full")
        
    # The insert step: Simply place the data to the last position
    self.storage[self.size] = data
    self.size += 1
    
    # Heapfication
    self.heapifyUp()
```

The `heapifyUp()` function represent the process where we check for compliance of the rule and swap elements. Essentially, we are walking up the tree and heapifying it.

```python
    def heapifyUp(self):
        index = self.size - 1
        
        # keep walking up the tree
        # we want to walk up as long as there is a parent element and 
        # that parent element as a value that is greater than what
        # we currently have
        while(self.hasParent(index) and self.parent(index) > self.storage[index]):
            # swap the element, so the element on the lower level
            # has a smaller value
            self.swap(self.getParentIndex(index), index)
            
            # Advance up the tree
            index = self.getParentIndex(index)
```

Notice that we can also implement the insert function in a recursive way

```python
    # recursive implementation
    def insert(self, data):
        if (self.isFull()):
            raise("Heap is full")
        
        self.storage[self.size] = data
        self.size += 1
    
        # Heapfication
        self.heapifyUp(self.size - 1)
        
    def heapifyUp(self, index):
        if self.hasParent(index) and self.parent > storage[index]:
            self.swap(self, getParentIndex(index), index)
            self.heapifyUp(self.getParentIndex(index))
```

Then, we have the delete function. Since a binary heap is a tree structure, we only have access to the root. Therefore, when we delete a node, we can only delete the root node. 

```python
def removeMin(self):
    if(self.size == 0):
        raise("Empty Heap")
        
    # remove the root and place the last element in the heap as the new root
    data = self.storage[0]
    self.storage[0] = self.storage[self.size - 1]
    self.size -= 1
    
    # Heapification
    self.heapifyDown()
```

When we heapify the tree after a deletion, we need to start at the new root node and work our way down the tree.

```python
def heapifyDown(self):
    index = 0
    # we want to keep iterating as long as the current node
    # has a left child.
    while(self.hasLeftChild(index)):
        # get the child with the smallest value
        smallerChildIndex = self.getLeftChildIndex(index)
        if (self.hasRightChild(index) 
            and self.rightChild(index) < self.leftChild(index)):
            smallerChildIndex = self.getRightChildIndex(index)
        
        # check if the heap is in compliance with the rule
        if (self.storage[index] < self.storage[smallerChildIndex]):
            break
        else:
            self.swap(index.smallerChildIndex)

        index = smallerChildIndex
```

We can also implement this function recursively

```python
def removeMin(self):
    if(self.size == 0):
        raise("Empty Heap")
        
    # remove the root and place the last element in the heap as the new root
    data = self.storage[0]
    self.storage[0] = self.storage[self.size - 1]
    self.size -= 1
    
    # Heapification
    self.heapifyDown(0)
    
 def heapifyDown(self, index):
    smallest = index
    if(self.hasLeftChild(index) and self.storage[smallest] > self.leftChild(index)):
        smallest = self.getLLeftChildIndex(index)
    if (self.hasRightChild(index)
        and self.storage[smallest] > self.rightChild(index))
        smallest = self.getRightChildIndex(index)
    
    if(smallest != index):
        # either the left or the right child is the smaller than
        # the current node value
        self.swap(index, smallest)
        self.heapifyDown(smallest)
```