# CMPS 6610 Problem Set 03
## Answers

**Name:** Arjun Dadhwal


Place all written answers from `problemset-03.md` here for easier grading.


---

- **1b.**

For the algorithm, each application of the update function and advance made to the next element costs constant time O(1). The given iterate function goes through the entire list and stops only when the list is empty.

**Work W(n)**
For non empty array a, a constant-time update O(1) is done and then recursion is done on a list of length n - 1. This would give the following work: 

$$
W(n) = W(n-1) + O(1)
$$

### Deriving an asymptotic upper bound for $W(n)=W(n-1)+c$

- **Unroll $k$ steps:**

$$
W(n) = W(n-1) + c
$$

$$
W(n) = W(n-2) + 2c
$$

$$
...
$$

$$
W(n) = T(n-k) + kc.
$$

- **Base case:**
When the array is of length 0, i.e. empty.
$W(0) = O(1)$


- **Stop the iteration when $n-k=0$**

$$
W(n) = W(0) + cn = O(1) + cn.
$$


Therefore, the upper bound for the work will be:

$$
\boxed{W(n)\in O(n)}
$$



**Span S(n)**

Each call made by "isearch" to iterate, would compute f(x, a[0]) and would then make a recursive call to a[1:]. Therefore, since each recursive call is dependent on the previous recursive calls, this would require a sequential chain.


### Deriving an asymptotic upper bound for $S(n)=S(n-1)+c$

- **Unroll $k$ steps:**

$$
S(n) = S(n-1) + c
$$

$$
S(n) = S(n-2) + 2c
$$

$$
...
$$

$$
S(n) = S(n-k) + kc.
$$

- **Base case:**
When the array is of length 0, i.e. empty.
$S(0) = O(1)$


- **The iteration is completete when $n-k=0$**

$$
S(n) = S(0) + cn = O(1) + cn.
$$


Therefore, the upper bound for the span will be:

$$
\boxed{S(n)\in O(n)}
$$


---

- **1d.**

Eeach equality and each OR-combine is \(O(1)\). These two recursive calls in `reduce` run in **parallel**.

#### Work (W(n))

- **Map step (build booleans):** one equality per element

$$
W_{\mathcal map}(n)=O(n)
$$



- **Reduce step (balanced tree of ORs):**
  - **Level \(i\):** subproblem size $n/2^i$; nodes $2^i$; **cost per node** $O(1)$.  
    **Cost at level \(i\):** $O(2^i)$.
  - **Number of levels:** solve $n/2^(\lceil \log_2 n \rceil) \le 1$
  - **Total reduce work (sum of per-level costs):**  $W_{\text{red}}(n) \le \sum_{i=0}^{\lceil \log_2 n \rceil}\Theta(2^i)
= \Theta(2^{\lceil \log_2 n \rceil+1}-1)
= \Theta(2^\lceil \log_2 n \rceil)
= \Theta(n).$








- **Total work:**  
  $W_{\mathcal map}(n) + W_{\mathcal reduction}(n)=O(n) + O(n) = O(n)$


#### Span (S(n))

- **Map step (using parallel map):** fork–join over \(n\) equalities  

$$
S_{\mathcal map}(n)=O(\log_2 n)
$$

- **Reduce step:** two halves run in parallel, then an \(O(1)\) combine  
  - **Per-level span cost:** $O(1)$ 
  - **Number of levels:** $\lceil \log_2 n \rceil$  
  - **Total reduce span:** $S_{\text{red}}(n) = \sum_{i=0}^{\lceil \log_2 n \rceil}\Theta(1) = \Theta(L)=\Theta(\log n).$

- **Total span:**  
$S(n)=\max{S_{\mathcal Map}(n),\,S_{\mathcal Red}(n)\} = O(\log_2 n)$

**Conclusion:** The work of $O(n)$ and span of $O(\log_2 n)$ for research is consistent with that of the balanced reduce and parallel map that was shown in the lecture.

---
- **1e.**

#### Work (W(n))

$W_{\mathcal total}(n) = W_{\mathcal map}(n) + W_{\mathcal reduce}$


- **Mapping** $W_{\mathcal map}(n) =O(N)$
- **Reducing** It has one unbalanced split then two balanced reduces of sizes ~n/3 and ~2n/3. Let Wbalance(m) be the work balanced reudce, we know it would we Wbal(m) = O(m) as was derived for the previous reduce function that was given.

$$
W_{\mathcal reduce}(n) = W_{\mathcal balance}(n/3) + W_{\mathcal balance}(2n/3) + O(1) = O(n/3) + O(2n/3+ O (1) = O(N)
$$


Therefore, the total work is: 
$W_{\mathcal total}(n) = W_{\mathcal map}(n) + W_{\mathcal reduce} = O(N) + O(N) = O(N)$


#### Span (S(n))

- **Mapping** The N equalities over which we will do a fork-join. Therefore  $S_{\text{map}}(n) = O(\log_2 n)$
- **Reducing** We know that the general span is $S_{\mathcal Balance}(m)=O(\log_2 m)$ when the size of input is m as had been derived for the earlier function. Therefore, considering the unbalanced split, the span for the reduction can be derived like this:

$S_{\mathcal reduce}(n)=\max{S_{\mathcal balance}(n/3),\,S_{\mathcal balance}(2n/3)\} +O(1) = O(\log_2 (2n/3)) = O(\log_2 n)$

- **Total span** Therefore, from the two phases, the total span would be $S_{\mathcal total}(n)=\max{S_{\mathcal map}(n),\,S_{\mathcal reduce}(2n/3)\} = O(\log_2 n)$

- **Conclusion** Replacing the reduce function by "unreduce" for the given function does not change the asymptotic bounds for the work and span as compared to research.


__


- **2a.**

```
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
```

#### Work (W(n))

The program has different phases: 

- **Seen so far size bound for every ith index**: $$|seenSoFar| \le i $$
For every ith iteration, there can be at most i items behind, therefore that is the maximum number of items that could have been seen.

- **Member Work:** $W_{\text{member}}(n) \le \sum_{i=0}^{n-1}\Theta(i)
= \Theta(n^2).$

The memeber function looks back at previous items and the maximum number of items could be around the same at its index.

- **Process work:** $W_{\text{proc}}(n) \le \sum_{i=0}^{n-1}(\Theta(1) + \Theta(i))
= \Theta(n) + \Theta(n^2)
= \Theta(n^2).$

For each step i, the cost would be O(1) for match/branch/cons and O(i) for calling the Member since it will iterate through all the elements in indices i and before to check for membership.

- **Reverse work:** $W_{\text{rev}}(n) = \Theta(n)$

Reverse goes through the entire list so would be done in linear time based on the length.

- **Total work:** $W_{\text{rev}}(n) = W_{\text{member}}(n) + W_{\text{proc}}(n) + W_{\text{rev}}(n)  = \Theta(n^2) + \Theta(n^2) + \Theta(n) $

Putting them altogether, the work is asymptotically dominated by $\Theta(n^2)$.



#### Span (S(n))


The entire program is sequential and parallelizing is not possible.
Therefore, Stotal(n) = Sproc(n) + Srev(n) = O(n^2) + O(n) = O(n^2).


- **Member Span:** $S_{\text{member}}(m) = S_{\text{member}}(m-1) +  \Theta(1) =  \Theta(m)$

Let Smember(m) be the sapn of member. Since member is a linear scan relying on the previous results, Smember(m) = Smember(m-1) + O(1) = O(m).


- **Process Span:** $S_{\text{proc}}(n) \le \sum_{i=0}^{n-1}(\Theta(1) + S_{\text{member}}(i)) = \sum_{i=0}^{n-1}(\Theta(1) + \Theta(i) 
= \Theta(n) + \Theta(n^2)
= \Theta(n^2).$

For every ith iteration, we know that member will called once per element for the "SeenSoFar" sublist, therefore every ith call the membership check will cost Smember(i) = O(i) span and the other constant overhead will take about O(1). Process is sequential with each call recursing on the rest, this will all add up along the critical path.


For each step i, the cost would be O(1) for match/branch/cons and O(i) for calling the Member since it will iterate through all the elements in indices i and before to check for membership.

- **Reverse span:** $S_{\text{rev}}(n) = \Theta(n)$

Reverse is also sequential, so its span will be the same as the work.

- **Total span:** $S_{\text{rev}}(n) = S_{\text{member}}(n) + S_{\text{proc}}(n) + S_{\text{rev}}(n)  = \Theta(n^2) + \Theta(n^2) + \Theta(n) $

The entire program is sequential and parallelizing is not possible. Putting them altogether, the span is asymptotically dominated by $\Theta(n^2)$.


---

- **2b.**
```
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
```

#### Work (W(n))

### Work $W(N)$

**Flatten**
This function splits the given outer list into halves, with two halves being recursively done parallely, then being joined together with append.

Per combine, appending two lists of sizes $N_L$ and $N_R$ costs $\Theta(N_L)$ work (copy left spine once).

$$
W_{\text{flat}}(N)
\;\le\;
W_{\text{flat}}(N_L) + W_{\text{flat}}(N_R) + \Theta(N_L),
\qquad
N_L + N_R = N.
$$

Across any recursion **level**, the left sizes over all combines sum to $N$, so each level is $\Theta(N)$.
There are at most $\log K$ levels for $K$ sublists; since $K \le N$, we get

$$
W_{\text{flat}}(N) \;=\; \Theta(N\log N).
$$

**Sort**

For this we can use a standard parallel sorting algorithm such as mereg sort, therefore W(N) = 2W(N/2) + O(N), where there are two halves and linear worked required for merging them.


$$
W_{\text{sort}}(N)
\;=\;
2\,W_{\text{sort}}(N/2) + \Theta(N)
\;=\;
\Theta(N\log N).
$$

**Dedup adjacent (single pass)**

For each pass there will be one comparison. Therefore, the recurrencne can be written as W(N) = W(N-1) + O(1)

$$
W_{\text{dedup}}(N)
\;=\;
W_{\text{dedup}}(N-1) + \Theta(1)
\;=\;
\Theta(N).
$$

**Total work**

All these phases run sequentially, therefore the work will add up.


$$
W(N)
\;=\;
W_{\text{flat}}(N) + W_{\text{sort}}(N) + W_{\text{dedup}}(N)
\;=\;
\Theta(N\log N) + \Theta(N\log N) + \Theta(N)
\;=\;
\boxed{\Theta(N\log N)}.
$$


#### Span (S(n))

### Span $S(N)$

**Flatten**

We know that the two halves are being run in parallel, therefore the span will be the maximum span along with the cost of merging.

$$
S_{\text{flat}}(N)
\;=\;
\max\!\big(S_{\text{flat}}(N_L),\,S_{\text{flat}}(N_R)\big) + \Theta(N_L),
\qquad N_L+N_R=N.
$$

Across levels (from leaves to root) the left sizes sum to
$N/2 + N/4 + N/8 + \cdots = \Theta(N)$, hence

$$
S_{\text{flat}}(N) \;=\; \Theta(N).
$$


**Sort**

We can use a parallelized sorting algorithm, preferably merge sort with merge being possible with O(logN) span with forking and being across O(logN) levels.

$$
S_{\text{sort}}(N)
\;=\;
S_{\text{sort}}(N/2) + \Theta(\log N).
$$

Unrolling for $\lceil \log_2 N \rceil$ levels,

$$
S_{\text{sort}}(N)
\;\le\;
\sum_{i=0}^{\lceil \log_2 n \rceil} \Theta\!\big(\log (N/2^i)\big)
\;=\;
\Theta\!\left(\sum_{i=0}^{\lceil \log_2 n \rceil}(\log N - i)\right)
\;=\;
\Theta\!\big((\log N)^2\big).
$$


**Dedup adjacent**

There is nothing we can parallelize since it is sequential, therefore, it will be the same as the work

$$
S_{\text{dedup}}(N) \;=\; S_{\text{dedup}}(N-1) + \Theta(1) \;=\; \Theta(N).
$$


**Total span**
The total span will add up across the sequential phases

$$
S(N)
\;=\;
S_{\text{flat}}(N) + S_{\text{sort}}(N) + S_{\text{dedup}}(N)
\;=\;
\Theta(N) + \Theta\!\big((\log N)^2\big) + \Theta(N)
\;=\;
\boxed{\Theta(N)}.
$$




**Comparison to previous algorithm**

- **Work:** current $W(N)=\Theta(N\log N)\) vs. previous \(W_{\text{prev}}(N)=\Theta(N^2)$.
- **Span:** current $(S(N)=\Theta(N)\) vs. previous \(S_{\text{prev}}(N)=\Theta(N^2)$.
- **Parallelism:** previous $frac{W}{S}=\Theta\!\left(\frac{N^2}{N^2}\right)=\Theta(1)$ ; current $\frac{W}{S}=\Theta\!\left(\frac{N\log N}{N}\right)=\Theta(\log N)$

**Reason:** the earlier algorithm performed a sequential membership check per element, leading to quadratic work and span.  
The current approach uses divide-and-conquer flattening plus parallel sorting, cutting the work to $N\log N$ and the span to $N$.





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

The algorithms makes use of iterate which processes the list sequentially, since each item relies on the previous and about O(1) constant work is done by parens_update function. Iterate also keeps going until it has visited all the items.

We get the following recurrence

W(n) = W(n - 1) + c

For base case W(0) = O(1)

### Deriving an asymptotic upper bound for $W(n)=W(n-1)+c$

- **Unroll $k$ steps:**

$$
W(n) = W(n-1) + c
$$

$$
W(n) = W(n-2) + 2c
$$

$$
...
$$

$$
W(n) = T(n-k) + kc.
$$

- **Base case:**
When the array is of length 0, i.e. empty.
$W(0) = O(1)$


- **Stop the iteration when $n-k=0$**

$$
W(n) = W(0) + cn = O(1) + cn.
$$


Therefore, the upper bound for the work will be:

$$
\boxed{W(n)\in O(n)}
$$

Therefore, the work would be O(N)


The given algorithm can not be parallized, therefore the critical path would include all the steps:

S(n) = S(n - 1) + c

### Deriving an asymptotic upper bound for $S(n)=S(n-1)+c$

- **Unroll $k$ steps:**

$$
S(n) = S(n-1) + c
$$

$$
S(n) = S(n-2) + 2c
$$

$$
...
$$

$$
WS(n) = T(n-k) + kc.
$$

- **Base case:**
When the array is of length 0, i.e. empty.
$S(0) = O(1)$


- **Stop the iteration when $n-k=0$**

$$
S(n) = S(0) + cn = O(1) + cn.
$$


Therefore, the upper bound for the span will be:

$$
\boxed{S(n)\in O(n)}
$$


Therefore, the span is O(N).




---

- **3d.**

The given implementation can be divided into three different sequential phases: the mapping phase where the input characters are mapped with the parenthesis map to values of (1, -1, 0), the scan (prefix sum via contraction) done over that array, and then the reduce of the prefix array with min, checking that no prefix goes below 0.

### Phase 1: Map

`paren_map` is applied to all the n items. For each element it takes O(1) work. This is parallelizable and can be done separately for each item.

Work:

$$
W_{\text{map}}(n)=\sum_{i=1}^{n}\Theta(1)=\Theta(n).
$$

Span (parallel map model):

$$
S_{\text{map}}(n)=\max_i \Theta(1)=\Theta(1).
$$


### Phase 2: Scan

Adjacent items are combined pairwise leading to half-size smaller problems that can be solved in parallel. Recursion is done on these subproblems and the results are used to fill up the even and odd positions of the full prefix array.

Work recurrence:

$$
W_{\text{scan}}(n)=W_{\text{scan}}(n/2)+c\,n,\qquad W_{\text{scan}}(1)=\Theta(1).
$$

Unrolling:

$$
W_{\text{scan}}(n)=W(n/2)+c\,n
$$

$$
=\,W(n/4)+\tfrac{c n}{2}+c n
$$

$$
=\,W(n/8)+\tfrac{c n}{4}+\tfrac{c n}{2}+c n
$$

$$
=\,W(1)+c\!\left(n+\frac{n}{2}+\frac{n}{4}+\cdots\right)=\Theta(n).
$$

Span recurrence:

$$
S_{\text{scan}}(n)=S_{\text{scan}}(n/2)+\Theta(1),\qquad S_{\text{scan}}(1)=\Theta(1).
$$

Balanced tree with constant cost per level \(\Rightarrow\) height \(\log n\):

$$
S_{\text{scan}}(n)=\Theta(\log n).
$$


### Phase 3: Reduce

min is computed over all the n prefix values. A balanced binary tree is formed during reduction.

Work recurrence:

$$
W_{\text{red}}(n)=2\,W_{\text{red}}(n/2)+\Theta(1),\qquad W_{\text{red}}(1)=\Theta(1).
$$

Unrolling (sum of per-level costs over log n levels):

$$
W_{\text{red}}(n)=\sum_{i=0}^{\lceil\log_2 n\rceil} 2^{i}\cdot\Theta(1)=\Theta(n).
$$

Span recurrence:

$$
S_{\text{red}}(n)=S_{\text{red}}(n/2)+\Theta(1),\qquad S_{\text{red}}(1)=\Theta(1).
$$

Balanced tree, height \(\log n\):

$$
S_{\text{red}}(n)=\Theta(\log n).
$$


### Putting all of these together

Total work:

$$
W(n)=W_{\text{map}}(n)+W_{\text{scan}}(n)+W_{\text{red}}(n)=\boxed{\Theta(n)}.
$$

Map must be finished before scan and scan must be finished before reduce. Therefore, this is a sequential procedure across phases and the spans add.

Total span:

$$
S(n)=S_{\text{map}}(n)+S_{\text{scan}}(n)+S_{\text{red}}(n)=\boxed{\Theta(\log n)}.
$$

---


- **3f.**

Every call to the function splits the given list into two halves, makes parallel recursive calls to both, and then combines the two results in constant O(1) time.

### Work

There are two half-sized subproblems and O(1) combine:

$$
W(n) \;=\; 2\,W(n/2) + \Theta(1), \qquad W(1)=\Theta(1).
$$

Unrolling a few steps:

$$
\begin{aligned}
W(n) &= 2W(n/2) + c \\
     &= 4W(n/4) + 2c \\
     &= 8W(n/8) + 4c + 2c \\
     &\;\;\vdots \\
     &= 2^{i} W(n/2^{i}) + (2^{i-1}+\cdots+2+1)c .
\end{aligned}
$$

At each level i, the add-on cost is $2^{i-1}c$. The number of levels is
\lceil \log_2 n \rceil. Summing the geometric series:

$$
\sum_{i=1}^{\lceil \log_2 n \rceil} 2^{i-1}c \;=\; c\,(2^{\lceil \log_2 n \rceil}-1) \;=\; \Theta(n).
$$

Therefore,

$$
\boxed{W(n)=\Theta(n)}.
$$

### Span

The two recursive calls run in parallel, so we take the **maximum** branch on the critical path, then add the O(1) combine:

$$
S(n) \;=\; S(n/2) + \Theta(1), \qquad S(1)=\Theta(1).
$$

Unrolling:

$$
S(n) \;=\; S(n/2)+c \;=\; S(n/4)+c+c \;=\; \cdots \;=\; S(1)+c\,\log_2 n
\;=\; \Theta(\log n).
$$

Equivalently, the recursion forms a balanced tree of height log n with O(1) cost per level.

$$
\boxed{S(n)=\Theta(\log n)}.
$$

**Conclusion:** With parallel recursive calls and \(O(1)\) combine, the algorithm has linear work and logarithmic span: \(W(n)=\Theta(n)\), \(S(n)=\Theta(\log n)\).

--

