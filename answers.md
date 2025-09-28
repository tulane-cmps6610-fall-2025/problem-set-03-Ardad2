# CMPS 6610 Problem Set 03
## Answers

**Name:**_________________________


Place all written answers from `problemset-03.md` here for easier grading.




- **1b.**

For the algorithm, each application of the update function and advance made to the next element costs constant time O(1). The given iterate function goes through the entire lost and stops only when the list is empty.

**Work W(n)**
For non empty array a, a constant-time update O(1) is done and then recursion is done on a list of length n - 1. This would give the following work: 

W(n) = W(n-1) + O(1)

Note: Base case W(0) = O(1) when A is empty.

If we unroll this, we get:

W(n) = W(n-1) + c
     = W(n-2) + 2c
     ...
    = W(0) + nc
    = O(1) + O(n) = O(n)

**Span S(n)**

Each call made by isearch to iterate, would compute f(x, a[0]) and would then make a recursive call to a[1:]. Therefore, since each recursive call is dependent on the previous recursive calls, this would require a sequential chain.

Base case S(0) = O(1) for an empty list.

S(n) = S(n-1) + O(1)
     = S(n-1) + c
     = S(n-2) + 2c
       ....
     = S(0) + nc
     = O(1) + O(n) = O(n)

The work and the span are both O(n).

- **1d.**





- **1e.**





- **3b.**




- **3d.**





- **3f.**



