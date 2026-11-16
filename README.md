# Lab 09: Exploring D&C Algorithms

**Instructions:** Work in small groups to complete the following exercises. Try sketching out your work on the whiteboards or scratch paper. Everyone should submit their own answers to the exercises, but you are encouraged to discuss your reasoning with your group members.

Name(s): ___Rivan________________________

---

## Exercise 1: Recurrence Extraction

Consider the following divide-and-conquer algorithm, which takes an array $A$ of $n$ numbers as input and returns a single number. 

```text
Procedure Mystery(A, n)
    if n <= 1:
        return A[1]                              // Base case
        
    k = floor(n / 3)                             // Find the one-third point
    
    left = Mystery(A[1..k], k)                   // Recur on the left third
    right = Mystery(A[k+1..n], n - k)            // Recur on the rest
    
    total = 0 
    for i = 1 to n:
        total = total + (A[i] * left * right)    // Combine results
        
    return total
```

### Part A: The Recurrence Relation
Let $T(n)$ be the running time of `Mystery` on an input of size $n$. Write down a recurrence relation for the worst-case running time $T(n)$. You may assume $n$ is a power of 3 for simplicity. *(Note: You do not need to solve the recurrence relation yet, just formulate it).*
The algorithm makes two recursive calls:

One on an array of size (n/3)
One on an array of size (2n/3)
The for loop performs (n) iterations, so the combine step takes (\Theta(n)) time.

Therefore,

[
T(n)=T\left(\frac{n}{3}\right)+T\left(\frac{2n}{3}\right)+\Theta(n)
]

with the base case
[
T(1)=\Theta(1).
]

More explicitly, using a constant (c) for the loop work:

[
T(n)=T\left(\frac{n}{3}\right)+T\left(\frac{2n}{3}\right)+cn,
\qquad T(1)=c.
]

### Part B: Visualizing Algorithm Behavior
Without solving the recurrence, look closely at the recursive calls and the combine step. If you were to draw the recursion tree for this algorithm, what would it look like? Is the tree balanced or skewed? 

--- The recursion tree is not balanced because each problem is divided into unequal parts:

[
\frac{n}{3} \quad\text{and}\quad \frac{2n}{3}.
]

The tree is skewed toward the larger (2n/3) branch. The left branch reaches the base case more quickly, while the right branch continues for more levels. However, both branches still shrink geometrically, so the tree has logarithmic height.

## Exercise 2: Unrolling the Recursion Tree

Suppose you have developed a new divide-and-conquer algorithm. You analyze your pseudocode and determine that its worst-case running time $T(n)$ satisfies the following recurrence relation:

$$T(n) \leq 3T\left(\frac{n}{2}\right) + cn^2 \quad \text{for } n > 1$$
$$T(1) \leq c$$

where $c$ is a positive constant. 

### Part C: Recursion Tree Analysis

Analyze the running time of the algorithm by unrolling the recursion tree. Specifically, answer the following questions:

1. **Root Work:** Find the amount of work done at the root level (level $0$) of the recursion tree.
- At level (0), there is one problem of size (n). Therefore, the work is

[
cn^2.
]

2. **First Levels:** Find the amount of work done at level $1$ and level $2$ of the recursion tree.
--- At level (1), there are (3) problems, each of size (n/2):

[
3\left(c\left(\frac n2\right)^2\right)
=3\left(\frac{cn^2}{4}\right)
=\frac{3cn^2}{4}.
]

Thus, the work at level (1) is

[
\boxed{\frac{3cn^2}{4}}.
] 
[
9\left(c\left(\frac n4\right)^2\right)
=9\left(\frac{cn^2}{16}\right)
=\frac{9cn^2}{16}.
]

Thus, the work at level (2) is

[
\boxed{\frac{9cn^2}{16}}.
]

Work at Level 2
At level (2), there are (3^2=9) problems, each of size (n/4):
3. **Tree Height:** Find the height of the recursion tree (at what level does the input size reach $1$?).
--- --- The input size at level (i) is

[
\frac{n}{2^i}.
]

We reach an input size of (1) when

[
\frac{n}{2^i}=1.
]

Therefore,

[
2^i=n
]

and

[
\boxed{i=\log_2 n}.
]

The tree height is (\log_2 n), assuming (n) is a power of (2).

**Bonus!** Find a general formula for the total work done at level $i$ of the recursion tree.
--- At level (i):

There are (3^i) subproblems.
Each subproblem has size (n/2^i).
The work per subproblem is (c(n/2^i)^2).
Therefore, the total work at level (i) is

[
3^i \cdot c\left(\frac{n}{2^i}\right)^2.
]

Simplifying:

[
\boxed{cn^2\left(\frac34\right)^i}.
]


## Exercise 3: Reflection

1. During lecture last week, we looked at two different divide-and-conquer algorithms. What were these algorithms and what problems did they solve?


Binary search, which searches for a target value in a sorted array by repeatedly dividing the search range in half.
Merge sort, which sorts an array by dividing it into two halves, recursively sorting each half, and then merging the sorted halves.


2. How do the algorithms you analyzed in this lab compare to the ones we discussed in lecture? Specifically, each algorithm differs from the two we discussed in lecture in one important way. For each of the algorithms above, what is that difference? 
Algorithm 1: Mystery
Mystery is similar to merge sort because it divides the input recursively and performs linear work during the combine step. However, unlike merge sort, it divides the input into unequal parts:

[
\frac n3 \quad\text{and}\quad \frac{2n}{3}
]

instead of two equal halves.

It also makes two recursive calls, unlike binary search, which makes only one recursive call.

Algorithm 2: The Algorithm from Exercise 2
The recurrence

[
T(n)\leq 3T(n/2)+cn^2
]

describes an algorithm that makes three recursive calls, whereas merge sort makes two recursive calls and binary search makes one.

It also performs quadratic work, (cn^2), at each node. This is more combine work than merge sort, which typically performs linear work, (cn), during its merge step.

So the main differences are:

Mystery: unequal recursive subproblem sizes.
Exercise 2 algorithm: three recursive calls and quadratic combine work instead of fewer recursive calls and linear combine work.