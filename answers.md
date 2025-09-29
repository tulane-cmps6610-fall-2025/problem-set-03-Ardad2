# CMPS 6610 Problem Set 03
## Answers

**Name:** Arjun Dadhwal


Place all written answers from `problemset-03.md` here for easier grading.


---

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

---

- **2a.**

dedup input = 
     let 
          member(lst, item) = 
               case lst
               | Nil
               | Cons(hd, tl)

          reverse lst = 
               let
                    revLoop (src, acc) = 
                         case src
                         | Nil 
                         | Cons(hd, tl)
               in
                    revLoop(lst, Nil)
               end


          process (seenSoFar, keptRev, remaining) = 
               case remaining
               | Nil
               | Cons(elem, rest)
                    if member(seenSoFar, elem)
                    then process(seenSoFar, keptRev, rest)
                    else process(Cons(elem, seenSoFar), Cons(elem, keptRev), rest)
               in
                    process(Nil, Nil, input)
               end


The total work = Wmember + Wreverse + Wprocess

Member is called once per element for the "seenSoFar" sub-list therefore for the i-th call, |SeenSOFar| <= i, leading to a cost of O(i)

Therefore, Wmember(n) = Summation from i = 0 to n-1 of O(i) = O(n^2).


Let Wproc(n) be the work done for the process on a list of length n. 

For each step i, the cost would be O(1) for match/branch/cons and O(i) for calling member (as it will iterate through elements in indices i and before to check for membership).

Wproc(n) = summation from i = 0 to n-1 of (O(1) + O(i)) = summation from i = 0 to n-1 of O(1) + summation of i = 0 to n-1 of O(i) = O(n) + summation of i = 0 to n-1 of O(i) = O(n) + O(n^2) (Already calculated for Wmember(n)).

The final reverse, Wreverse(n) would be O(n) going through the entire list.

Therefore, Wtotal = Wmember + Wreverse + Wprocess = O(N^2) + O(N) + O(N) = O(N^2) (Since O(N^2) asymptotically dominates the other two).

Span(N)

Let Smember(m) be the sapn of member. Since member is a linear scan relying on the previous results, Smember(m) = Smember(m-1) + O(1) = O(m).

Let Sproc(n) be the span of the process and n is the length of the input.

For every ith iteration, we know that member will called once per element for the "SeenSoFar" sublist, therefore every ith call the membership check will cost Smember(i) = O(i) span and the other constant overhead will take about O(1). Process is sequential with each call recursing on the rest, this will all add up along the critical path.

Sproc(n) = Summation from i = to n-1(O(1) + Smem(i)) = summation from i = 0 to n-1 of O(1) + Summation from i = 0 to n-1 of O(i) = O(n) + O(n^2) = O(n^2).

Reverse is also sequential, so its span will be the same as the work. Srev(n) = O(n).

The entire program is sequential and parallelizing is not possible.
Therefore, Stotal(n) = Sproc(n) + Srev(n) = O(n^2) + O(n) = O(n^2).

---

- **2b.**

flatten listOfLists = 
     case listOfLists
     | Nil
     | Cons(xs, Nil)
     | _ 
          let
               (leftLists, rightLists) = splitMid listOfLists
               (leftFlat, rightFlat) = (flatten leftLists || flatten rightLists)
          in
               append(leftFlat, rightFlat)
          end

dedupAdjacent sortedList = 
     case sortedList
     | Nil
     | Cons(x, Nil)
     | Cons(curr, Cons(next, rest)) 
          if curr = next
          then dedupAdjacent(Cons(next, rest))
          else Cons(curr, dedupAdjacent(Cons(next, rest)))

multiDedup listOfLists = 
     let
          allItems = flatten listOfLists
          sortedItems = sort allItems
     in
          dedupAdjacent sortedItems
     end


Work

Flatten

This function splits the given outer list into halves, with two halves being recursively done parallely, then being joined together with append.

The total for flattening is:

Wflat(N) = Wflat(Nl) + Wflat(Nr) + Wappend + O(1)

Wappend that will combine the two lists cost O(length of left list) since it will require building a a new list copying the left and then adding right to it.

This is a balanced recursion. 

At each level: Each internal pays the cost for combinig O(size of the left result). This is then summed and the total left sizes will always be a constant fraction of N, therefore each level O(N) work.

For a balanced recursion, the work = (Total number of levels) * (Maximum work per level)

Since we are havling the number of sublists each time, we will have a balanced tree with a height of logn.

Therefore Work = LogN * N = O(NlogN).

Sorting

For this we can use a standard parallel sorting algorithm such as mereg sort, therefore W(N) = 2W(N/2) + O(N), where there are two halves and linear worked required for merging them.

Therefore Wsort(N) = O(NLogN)).

dedupAdjacent SortedList

For each pass there will be one comparison. Therefore, the recurrencne can be written as W(N) = W(N-1) + O(1) => Wdedup(N) = O(N).

The total work for multiDedup

All these phases run sequentially, therefore the work will add up.

W(total) = Wflatten + Wsort + Wdedup = O(NlogN) + O(NLogN) + O(N) = O(NlogN) (Since NLogN dominates asymptotically).


Span

Flatten

We know that the two halves are being run in parallel, therefore the span will be the maximum span along with the cost of merging.

Therefore, S(N) = max(S(Nl), S(Nr)) + Sappend = max(S(Nl), S(Nr)) + O(Size of left half)

Going from the leaves to the root, the append will copy the left sizes N/2, N/4, N/8.. with the sum being less than N. Therefore, 

Sflatten = O(N)

sort allItems

We can use a parallelized sorting algorithm, preferably merge sort with merge being possible with O(logN) span with forking and being across O(logN) levels.

Therefore, Ssort(N) = O(logN * logN) = O((logN)^2)).

dedupAdjacent SortedList

There is nothing we can parallelize since it is sequential, therefore, it will be the same as the work. Sdedup(N) = O(N).

Therefore, the total span will add up across the sequential phases.

Stotal(N, m) = Sflatten + Ssort + Sdedup = O(N) + O((logN)^2) + O(N) = O(N).


Compared to the previous algorithm, the work is better at O(NlogN) compared to the quadratic one for O(N^2) for the previous algorithm. The span is also substantially better at O(N) as compared to quadratic one O(N^2) for the previous algorithm.-

The parallelism for 2a is (W/S) = O(n^2)/O(n^2) = O(1), whereas for the current algorithm it is (W/S) = O(NlogN)/O(N) = O(logN).

The difference mainly comes from the previous algorithm using a sequential membership check compared to the current one's divide conquer flattening as well as parallel sorting phases.

---

- **2c.**

List Deduplication

- Iterate is useful since it can allow the carrying of an accumulator for the seen state from left to right and decided whether to keep or dro pat each stop. However, since it is inherently sequential just like the algorithm, it does not offer much performance benefits.
- Map, Tabulate, and Filter are not useful since the decsions are based on the elements before (prefix) and not just the local elements themselves. Reduce would require an associative combine to be present. Other operations like Update, Inject, Subseq etc. don't address the logic of the problem at hand.

 Deduplication in a network

 - Flatten + append to combine all the lists work quite well for the logic of the problem, allowing for parallel subcalls with total work O(N) and  span O(N).
 - Map or Tabulate combined with Filter are useful as they allow us to drop adjacent duplicate elemnts after sorting has been done with work O(N) and span O(logN).

---

- **3b.**


---

- **3d.**


---


- **3f.**

---

