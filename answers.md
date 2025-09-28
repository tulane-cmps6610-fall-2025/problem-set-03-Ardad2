# CMPS 6610 Problem Set 03
## Answers

**Name:** Arjun Dadhwal


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

---

- **1d.**

Model (as in lecture): each equality and each OR-combine is \(O(1)\). The two recursive calls in `reduce` run in **parallel**.

#### Work $(W(n)$)

- **Map step (build booleans):** one equality per element  
  \( W_{\text{map}}(n) = O(n). \)

- **Reduce step (balanced tree of ORs):**
  - **Level \(i\):** subproblem size \(n/2^i\); nodes \(2^i\); **cost per node** \(O(1)\).  
    **Cost at level \(i\):** \( \Theta(2^i) \).
  - **Number of levels:** solve \( n/2^L \le 1 \Rightarrow L=\lceil \log_2 n \rceil \).
  - **Total reduce work (sum of per-level costs):**  
    \( W_{\text{red}}(n) \le \sum_{i=0}^{L}\Theta(2^i)
      = \Theta(2^{L+1}-1)
      = \Theta(2^L)
      = \Theta(n). \)

- **Total work:**  
  \( W(n) = W_{\text{map}}(n) + W_{\text{red}}(n) = O(n)+O(n) = \boxed{\Theta(n)}. \)


#### Span \(S(n)\)

- **Map step (parallel map in lecture model):** fork–join over \(n\) equalities  
  \( S_{\text{map}}(n) = O(\log n). \)  
  *(If the map were sequential, it would be \(O(n)\).)*

- **Reduce step:** two halves run in parallel, then an \(O(1)\) combine  
  - **Per-level span cost:** \( \Theta(1) \)  
  - **Number of levels:** \( L=\lceil \log_2 n \rceil \)  
  - **Total reduce span:** \( S_{\text{red}}(n) = \sum_{i=0}^{L}\Theta(1) = \Theta(L)=\Theta(\log n). \)

- **Total span:**  
  \( S(n) = \max\{S_{\text{map}}(n),\,S_{\text{red}}(n)\} = \boxed{\Theta(\log n)}. \)

**Conclusion:** \( \boxed{W(n)=\Theta(n)} \) and \( \boxed{S(n)=\Theta(\log n)} \) for `rsearch` with the lecture’s balanced `reduce` (and parallel map).

---





- **1e.**

Work W(n)

Total work W(n) = Wmap(N) + Wreduce(n).

Mapping:

Wmap(N) = O(N) 

Reducing:

It has one unbalanced split then two balanced reduces of sizes ~n/3 and ~2n/3. Let Wbalance(m) be the work balanced reudce, we know it would we Wbal(m) = O(m) as was derived for the previous reduce function that was given.

Wreduce(n) = Wbalance(n/3) + Wbalance(2n/3) + O(1) = O(n/3) + O(2n/3) + O(1) = O(N)

Therefore, the total work W(n) = Wmap(n) = O(n) + O(n) = O(N)


Span S(N)

The Map Step: N equalities over which we will do fork-join.
Smap(n) = O(logN)
Reduce Step: Both the subreduces will run in parallel with combine being on the critical path.

Let Sbalance(m) = O(logm) be the span for balanced reduce as was derived for the earlier function.

Sreduce(n) = max(Sbalance(n/3), Sbalance(2n/3)} + O(1) = O(log(2n/3)) ~= O(logn)

Therefore, the total span S(n) = max(Smap(N), Sreduce(n)) = O(logN)

Result:

Replacing the reduce function by ureduce for the given function does not change the asymptotic bounds for the work and span for research.

- **2a.**

- **2b.**

- **2c.**

- **3b.**




- **3d.**





- **3f.**



