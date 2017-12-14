# CS4335 Review

## 1 Greedy
#### 1.1 Interval Scheduling
```
sort jobs by finish times
```
O(n log n)

#### 1.2 Interval Partitioning
```
sort jobs by starting times
```
O(n log n)

#### 1.3 Minimum Spanning Tree

#### 1.3.1 Kruskal's algorithm
```javascript
sort edges by weight
A = {}
while(A != MST) {
    edge = edge in queue with minimum weight
    if (edge.check_no_cycle())
        A.append(edge)
}
return A
```
O(|E| log |E|)

#### 1.3.2 Prim's Algorithm
Concepts: definition, safe cut
```javascript
Q= {(A, 0, Nil), (B, ∞, Nil), (C, ∞, Nil), (D, ∞, Nil), (E, ∞, Nil), (F, ∞, Nil), (G, ∞, Nil)}
// (this node, distance, adjacent node)

while(!Q.is_empty()) {
    A.append(adjacent with minimum w)
    update Q
}
```
O(|E| log |V|)<br>
O(|E| + |V|log|V|) using Fibonacci heap
(update takes O(1))

#### 1.4 Dijkstra Algorithm
Well-defined: no negative-weight cycles
Assumption: all edges have positive weight

- **π[v] (predecessor)**: src -> path -> π[v] -> v is the shortest path to v
- **d[v]**: weight(src -> path -> π[v] -> v)
- **Relaxation**: 
```python
def RELAX(u, v, w):
    if d[v] > d[u] + w(u, v):
        d[v] = d[u] + w(u, v)
        π[v] = u
```
Algorithm: 
Maintain S the set of vertices whose shortest-paths from s have been found, update S and do relaxation
```python
# initialize
for each v:
    d[v] = INF
    π[v] = NIL
d[src] = 0
# algorithm
while dest in Q:
    u = extract_min(Q)
    S.push(u)
    for v in Q:
        RELAX(u, v, w(u, v))
```
Naive implementation: O(V^2^) <br>
Using priority_queue: O(ElogV)

## 2 Divide and conquer

#### 2.1 Merge Sort
T(n) = 2 * T(n/2) + n = O(n log n)

#### 2.2 Counting Inversions
```
divide into 2
count each part
count inversion across two parts in O(n):
    sort each part
    declare two pointers
    left: i=n...1
    right: if an inversion is encountered, res+=i
    both pointers slides from left to right
```
T(n) = 2 * T(n/2) + O(n) = O(n log n)

#### 2.3 Number Multiplication
In terms of implementation...
[Master Theorem](https://en.wikipedia.org/wiki/Master_theorem_(analysis_of_algorithms))

## 3 Dynamic Programming

#### 3.1 Weighted Interval Scheduling
Label jobs by finish time:  f1 f2 ... fn <br>
Define p(j) = largest index i < j such that job i is compatible with j.
```python
def OPT(j):
    if j==0: 
        return 0
    return max{v(j)+OPT(p(j)), OPT(p(j-1))}
```
- Computing p~j~ in O(n)
    - two pointers traverse from right to left
Overall Time complexity is O(n log n) including sorting

#### 3.2 Manhattan Tourist Problem
Find the longest path
```
s(i, j) = max{s(i-1, j)+w1, s(i, j-1)+w2}
```

#### 3.3 Longest Common Subsequence (LCS)
X[1...i...n] Y[1...j...m]
```python
def c(i, j):
    if i == 0 or j == 0:
        return 0
    if X[i] == Y[j]:
        return c(i-1,j-1)+1
    return max{c(i-1,j), c(i,j-1)}

def LCS(X, Y):
    return c(n, m)
```
O(nm)

#### 3.4 Shortest Common Supersequence
X[1...i...n] Y[1...j...m]
```python
def c(i, j):
    if i == 0:
        return j
    if j == 0:
        return i
    if X[i] == Y[j]:
        return c(i-1,j-1)+1
    return min{c(i-1,j)+1, c(i,j-1)+1}
```

#### 3.5 Bellman-Ford Algorithm
#### 3.6 0-1 Knapsack Problem

## 4 Other Problems

#### 4.1 String Matching - KMP Algorithm
#### 4.2 Maximum Flow
#### 4.3 NP-Complete Problems
