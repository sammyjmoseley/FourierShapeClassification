--- 
title: Math 4420 Final Problem Set
author: Gabriel Ngam
date: 5/13/16
header-includes: 
    - \usepackage[margin=1in]{geometry}
    - \usepackage{amssymb}
    - \usepackage{tikz}
--- 

# Problem 1

Let $G$ be a bipartite graph on vertex set $X \sqcup Y$. A *matching* in $G$ is a set of edges $M \in E(G)$ having no vertices in common. 

## Part a

Let $\mathcal{I} \in 2^X$ be defined by $I \in \mathcal{I}$ if and only if there is a matching $M$ in $G$ such that, among the vertices in $X$, $M$ meets just those in $I$. Show that the family $\mathcal{I}$ is the family of independent sets of a matroid on $X$. 

## Solution

Recall the cryptomorphism for Independent Sets on a Matroid: 

\begin{enumerate}
\item $I \subset J \in \mathcal{I} \implies I \in \mathcal{I}$
\item $I, J \in \mathcal{I}, |I| > |J| \implies \exists a \in I \textbackslash J$ such that $J \cup \lbrace a \rbrace \in \mathcal{I}$
\end{enumerate}

To solve this problem, I will "generate" a network flow to generate the matchings for $X \cup Y$. I will prove the two parts above, which implies that the bipartite matching is a matroid. 

**Proof(1)**: Construct a network flow, with a constricted set consisting of the set of vertices in $J$. Apply ford-flukerson to this constricted set to obtain a matching $M$. Now, in $M$, remove all elemnts not containing vertices in $I$. This will produce a matching $M' \subset E(G)$ for $I$. 

**Proof(2)**: Let $M_I, M_J$ represent a set of matchings obtained for $I$ and $J$ through applying Ford-Fulkerson. To make my arguments easier to understand, let $I_X$ and $J_X$ represent the $X$ components in $M_I$ and $M_J$, and let $I_Y$ and $J_Y$ represent the $Y$ components found in the matchings of $M_I$ and $M_J$. I will split this up into three separate cases: $I_X \cap J_X = \emptyset, I_X \subset J_X$, and $I_X \cap J_X \neq \emptyset$. The last one will represent the "catch-all" case. 

*Case 1*: Choose any $X \in J - I$. Then, $I \cup \lbrace x \rbrace$ will be an independent set by the proof for part 1, since $I \cup \lbrace x \rbrace \subseteq J$. 

*Case 2*: Designate some $x$ such that $(x, y) \in M_J, y \in J_Y - I_Y$. We know that $(x, y)$ is an edge and will not collide with $M_I$, so we will be good. 

*Case 3*: Let $B_X = J_X - I_X$, $B_Y = J_Y - I_Y, A_Y = I_Y - J_Y, A_X = I_X - J_X$. For each $x \in J_X \cap I_X$, associate $x$ with the $y$'s in $M_J$ that it matched with. Next, we know that $B_Y > A_Y$, which implies that there is some $x \in B_X$ such that the $y$ that matches with $x$ in $M_J$ that does not collide with any other $y$. Designate that $x$ as our exchange argument, which will result in a safe matching. 

To clarify the solution matching for $I \cup \lbrace x \rbrace$, we will let $A_X$ match with their corresponding $A_Y$, all values in $I_X \cap I_Y$ match with their corresponding $y \in M_J$, and then the 'last' x will match with the $y$ in $M_J$ that is not in $M_I$, which we know exists due to the size differences. 

## Part b

Show that the matroid on $\lbrace 1,2,3,4,5 \rbrace$ defined as in part a by the bipartite graph is the same as the graphic matroid on $K_4 \textbackslash \lbrace e \rbrace$ for any $e$. 



## Solution

To solve this, I will show the set of everything in $I$, and then label the edges of $K_4 \textbackslash \lbrace e \rbrace$ such that the spanning forests will be identical. 

The family of independent sets for this matroid is equal to 1, 2, 3, 4, 5, 12, 13, 14, 15, 23, 24, 25, 34, 35, 45, 124, 125, 134, 135, 145, 234, 235, 245. There are no independent sets of size 4 or larger because $|Y| = 3$, which means there wouldn't be any subset of edges that doesn't have a collision for an element in $y$. $123,345$ are the only size 3 subsets of $X$ that do not have a matching, because none of them will match with the "bottom" element in $y$. 

Consider the following edge labeling for a graphic matroid: 

\begin{center}
\begin{tikzpicture}[scale=0.2]
\tikzstyle{every node}+=[inner sep=0pt]
\draw [black] (24.4,-9.3) circle (3);
\draw [black] (4.8,-28.6) circle (3);
\draw [black] (24.4,-28.6) circle (3);
\draw [black] (45.3,-28.6) circle (3);
\draw [black] (6.94,-26.5) -- (22.26,-11.4);
\fill [black] (22.26,-11.4) -- (21.34,-11.61) -- (22.04,-12.32);
\draw (15.62,-19.43) node [below] {$5$};
\draw [black] (26.6,-11.34) -- (43.1,-26.56);
\fill [black] (43.1,-26.56) -- (42.85,-25.65) -- (42.17,-26.39);
\draw (33.83,-19.44) node [below] {$1$};
\draw [black] (42.3,-28.6) -- (27.4,-28.6);
\fill [black] (27.4,-28.6) -- (28.2,-29.1) -- (28.2,-28.1);
\draw (34.85,-28.1) node [above] {$2$};
\draw [black] (7.8,-28.6) -- (21.4,-28.6);
\fill [black] (21.4,-28.6) -- (20.6,-28.1) -- (20.6,-29.1);
\draw (14.6,-29.1) node [below] {$4$};
\draw [black] (24.4,-25.6) -- (24.4,-12.3);
\fill [black] (24.4,-12.3) -- (23.9,-13.1) -- (24.9,-13.1);
\draw (24.9,-18.95) node [right] {$3$};
\end{tikzpicture}
\end{center}

**Claim**: The family of independent sets for this graphic matroid(the forests of the graph) are identical to the bipartite matching described above. 

**Proof**: The graph is simple, so there are no cycles with 1 or two edges. Therefore, all 2-subsets of $E(K_4 - \lbrace e \rbrace)$ are forests. As for the 3-subsets of the graphic matroid above, the only non-forests would be the two triangles 123 and 345, as desired. There are no 4-subsets of the graphic matroid that are forests because you either have a triangle with an edge hanging off or a loop along the "outside" of the graph. The family of the independent sets are thus equivalent, as desired. 

# Problem 2

Let $P$ be a finite poset with 0 and 1, and suppose $P$ has been partitioned into nonempty subsets $P = A \cup B \cup C$, where $A$ is an *order ideal* and $C$ is an *order filter*. Show that 

$$ 1 + \sum_{x \in A} \sum_{y \in C} \mu(x,y) = \sum_{x,y \in B} \mu(x,y)$$

## Solution

I wasn't really successful in making any substantial progress on this problem. However, here are some "properties" that I found out about this problem: $1 \in C, 0 \in A$, for any $a \in A, b \in B \wedge a = 0$ or $a$, and the same except with joins for $a$. 

One attempt that I tried to do with this problem was try to simplify $\sum_{a \in A} \sum_{c \in C} \mu(a,c)$ into $\sum_{a \leq z < c} \mu(a,z)$. If $z \in C$, then it will cancel out with another Mobius inversion in the summation, but the number of negatives would far outweigh the positives and there might be overcounting. Also, I was unsure about how $z \in B$ or $z \in A$ would end up being evaluated. 

Somehow applying Weisner's theorem might have been another option, but I wasn't entirely sure how to approach it. 

    
# Problem 3

Compute the determinant of the $n \times n$ matrix in terms of $\phi$. 

## Solution

Let $f(n)$ denote the matrix of the $n \times n$ matrix defined from the problem. I claim that $f(n) = \pi_{i=1}^n \phi(n)$. 

The main intuition behind this problem involves finding ways to manipulate the matrix $M$ such that all values $a_{ij}, i \neq 1, j \neq i = 0$. On the diagonal, the values $a_{ii}$ will be equal to $\phi(i)$, at which point the determinant will reduce to calculating the product of the values on the diagonal, which will be equal to equal to $f(n)$ as described above. 

Define the map $g: M \rightarrow M'$, where both are $n \times n$ matrices. Let $a_q$ represent the $q$-th row of matrix $M$, and $a_q'$ represent the $q$-th row of matrix $M'$. We know $q = p_1^{a_1} \ldots p_r^{a^r}$. Let $a_q' = a_q - \sum_{i=1}^R a_{q/p_i} + \sum_{i,j=1}^R a_{q/(p_ip_j)} - \ldots$. 

**Claim**: The determinant of $M'$ is equal to the determinant of $M$. 

**Proof**: The map $G$ consists of only row manipulations, which implies that the determinant will remain the same. 

**Claim**: Let $a'_{ij}$ represent the value at the $i$-th row and $j$-th column of $M'$. 

$$a'_{ij}  =
\left\{
    \begin{array}{ll}
        1       & \mbox{if } i = 1 \\
        0       & \mbox{if } i \neq 1, i < j \\
        \phi(i) & \mbox{if } i = j
    \end{array}
\right. $$

**Proof**: The first row holds because we didn't apply any row manipulations to the matrix. 

$i > j$: To make the notation not conflict with the prime number subscripts, take $n > k$. 

Let $n = p_1^{a_1}\ldots p_r^{a_r}$, and let $k = p_1^{b_1} \ldots p_r^{b_r}$. When we are taking the inclusion-exclusion sum, a prime number in $k$ that does not "collide" with a prime number in $n$ also wouldn't end up colliding with any of $n$'s factors, so without loss of generality, simplify the equation so that we are doing operations on $l = p_1^{l_1} \ldots p_r^{l_r}$ with extraneous values removed. In this case, $(n,k) = l$

Now, we want to prove that $l - \sum_{i=1}^r (\frac{n}{p_i},l) + \sum_{i,j}^r (\frac{n}{p_ip_j},l) \ldots = 0$. I'm not sure how to prove this.  

$i = j$: Consider $a_{nn}'$, and just the subtractions of the GCD's on that column. Now, $a_q$ from above can be turned into the following: 

$$a_{nn}' = a_{nn} - \sum_{i=1}^r (\frac{n}{p_i},n) + \sum (\frac{n}{p_ip_j},n)  - \ldots = n - \sum_{i=1}^r \frac{n}{p_i} + \sum \frac{n}{p_ip_j} - \ldots = n \pi_{i=1}^r (1 - \frac{1}{p_i}) = \phi(n)$$

## Example

The following is an example of the mapping $g$ applied to a $7 \times 7$ matrix.   

Before the reduction: 

\begin{verbatim}
1 1 1 1 1 1 1
1 2 1 2 1 2 1
1 1 3 1 1 3 1
1 2 1 4 1 2 1
1 1 1 1 5 1 1 
1 2 3 2 1 6 1 
1 1 1 1 1 1 7
\end{verbatim}

\begin{verbatim}
1 1 1 1 1 1 1
0 1 0 1 0 1 0
0 0 2 0 0 2 0
0 0 0 2 0 0 0  
0 0 0 0 4 0 0
0 0 0 0 0 2 0 
0 0 0 0 0 0 6
\end{verbatim}

The matrix is upper triangular, and the values on the diagonal are equal to $\phi(n)$, which implies that the determinant is equal to the product of the $\phi$-functions up to $n$. 

# Problem 4

Define a *minimal cutset* of a connected graph $G$ to be a set of edges $F \subseteq E(G)$ whose removal disconnects $G$, yet no proper subset of $F$ has this property. Use the Dowling-Wilson Theorem to show that for any connected graph $G$, the number of minimal cutsets is at least $|E(G)|$. 

## Some Definitions

**Corollary(Dowling Wilson)**: In a finite geometric lattice of rank $n$, the number of elements of rank $\geq n-k$ is at least the number of elements of rank $k$, i.e. if $W_i = \# \lbrace x \in L | \text{rank}(x) = i \rbrace =$ Whitney number of the second Kind, then $W_0 + \ldots + W_K \leq W_{n-k} + \ldots + W_N$

Remember the partition lattice for a Graph $G$: The bottom element consists of every vertex in the graph being connected, and as edges are removed from the graph, the graph becomes more partitioned, and more graph vertex sets are being partitioned from each other. The top element will thus consist of all vertices being separated from each other. 

## Solution

From the partition lattice above, consider the coatoms of the Lattice. The coatoms consist of rank $n-1$, which implies that two vertices are connected to each other in the lattice. But wait, that means that $W_{n-1} = |E(G)|$! That implies that if we can prove the number of minimum cutsets is greater than or equal $W_1$, then we can apply Dowling-Wilson, and we are done. 

**Claim 1**: A minimal cutset partitions the lattice into no more than 2 separate parts. 

**Proof**: Suppose there exists a minimal cutset such that it partitions the graph into 3 separate subgraphs. we know that we cannot simultaneously partition a graph into three separate subgraphs by removing one edge, which means we can remove an edge from the cutset and still have a partition. Therefore, the cutset is not minimal. 

**Claim 2**: A minimal cutset for one atom cannot be a minimal cutset for another atom.

**Proof**: Suppose we have two separate atoms $a_1$ and $a_2$. Becuase the atoms differ, the partitions are different, which implies our cutset partitions the graph into more than 2 subgraphs. Therefore, it is not a minimal cutset by Claim 1. 

**Claim 3**: For each atom in the partition lattice, there exists at least 1 minimal cutset associated with it. 

**Proof**: Because the atom is in the lattice, we know there exists *some*
set of edges that partitions the graph as such. Call this set $F$. Then, take the power set of $F$, and choose the set of edges in the power set such that it is minimal and it still partitions the graph. This set of edges, while not necessarily unique, is still a minimal cutset because our method found no sets of smaller size that partition the graph into the desired atom. Therefore, we have found at least 1 minimal cutset for each atom.

**Claim 4**: $W_1 \leq$ the number of minimal cutsets in a graph. 

**Proof**: We know that every atom has at least one minimal cutset, and the same minimal cutset cannot apply to two different atoms, so we are done. 



# Problem 5

Show that $\mu_L(0,1) = |\mathcal{H}| - \frac{1}{2}R$

## Solution

We know that the number of rank 1 atoms in the intersection lattice for this hyperplane arrangement is equal to the number of hyperplanes in the lattice, so $|\mathcal{H}| = n = W_1$. 

Also, from Zavlavsky's Theorem, we know that $R = (-1)^{rank(L)} \chi_G(-1)$. 

Now, expand the Mobius Inversion for $\mu(0,1)$: 

$$\mu(0,1) = - \sum_{0 \leq z < y} \mu(0,y) = -1 - \sum_{x \text{ atom}} \mu(0,x) - \sum_{x \text{ coatom}} \mu(0,x) = (-1 + n - \sum{x \text{ coatom}} \mu(0,x))$$

Now, apply Zavlavsky's Theorem to expand the right hand side:

$$|\mathcal{H}| - \frac{1}{2}R = n + \frac{1}{2} \chi_G{-1} = n + \frac{1}{2}  
(- \mu(0,0) + \sum_{x \text{ atom}} \mu(0,x) - \sum_{x \text{ coatom}} \mu(0,x) + \mu(0,1))$$ 

$$ = n + \frac{1}{2} (-1 - n - \sum_{x \text{ coatom}}\mu(0,x) + (-1 + n - \sum_{x \text{ coatom}} \mu(0,x))) = n - 1 - \sum_{x \text{ coatom}} \mu(0,x) = \mu(0,1)$$

The last substitution seems suspect, but it ended up working out, so I'm good. 

# Problem 6

Let $m$ be given. Show that if $n$ is large enough, every $n \times n$ (0,1)-matrix has a principal submatrix of size $m$, in which all the elements below the diagonal are the same, and all the elements above the diagonal are the same. 


## Solution

This disregards the hint, and finds a matrix with the . 

Consider the first $2m$ rows in a single column of an 0-1 matrix. 

**Claim**: The column contains either $m$ ones or $m$ zeroes

**Proof**: You have 2m balls and m boxes. One box will have at least m balls in one of the boxes. 

First, crop out the rows of the matrix that are below index $2m$. Now we have a $2m \times n$ matrix, where $n$ is currently undefined.

There are $(2^m)^2 = 2^{2m}$ unique columns. Label these columns $a_i$, and let $2^{2m} = a$. What we want is to find some $a_i$ such that it appears at least $m$ times. Therefore, through the pigeonhole principle, let there be $a^{m-1} + 1$ such columns. As a result, there would be some $a_i$ such that there are at least $m$ instances of it in the matrix. Remove all other columns not equal to $a_i$. Now, you have some $2m \times m$ matrix where all rows are identical. From claim one, we know at least at least one half of the rows are all ones or all zeroes. Remove all rows that are equal to the minority color, and then remove rows until you have a $m \times m$ matrix. As a result, you either have the 1 or the 0 matrix, which is a stronger condition than what we originally wanted, also with a much larger bound. 

$N = 2^{2m(m-1)} + 1$
