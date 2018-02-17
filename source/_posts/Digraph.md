---
title: Digraph
tags:
  - Graph
abbrlink: 32086
date: 2018-02-17 16:48:44
---
## Introduction
&emsp;A **graph** *G* consists of a set *V*, whose members are called the **vertices** of *G*, together with a set *E* of pairs of distinct vertices from *V*. These paris are called the **edges** of *G*. If *e = (v, w)* is an edge with vertices *v* and *w*, then *v* and *w* are said to **lie on** *e*, and *e* is said to be **incident** with *v* and *w*.
<!-- more -->

### Two Kinds of Graph
If the pairs are **unordered**, then *G* is called an **undirected graph**;
If the pairs are **ordered**, then *G* is called a **directed graph**, often shortened to **digraph**.

#### Undirected Graph
There are several concepts concerning **undirected graph**:
  + Two vertices in an *undirected graph* are called **adjacent** if there is an edge from one to the other.
  + A **path** is a sequence of distinct vertices, each adjacent to the next.
  + A **circle** is a path containing at least three vertices such that the last vertex on the path is adjacent to the first one.
  + A graph is called **connected** if there is a path from any vertex to any other vertex.
  + If a graph is disconnected, we shall refer to a **maximal subset of connected vertices** as a **component**.

And now we can give the definition of a **tree**:  **A *free tree* is defined as a connected undirected graph with no circle.**

#### Direted Graph
We require all edges in a path or a cycle to have the same direction, so that following a path or a circle means always moving in the direction indicated by the arrows.
A directed graph is called **strongly connected** if there is a directed path from any vertex to any other vertex.
If we suppress the direction of the edges and the resulting undirected graph is connected, we call the directed graph **weakly connected**.

### Traversal
#### Depth-First Algorithm(DFS)
**depth-first** traversal is naturally formulated as a recursive algorithm. Its action, when it reaches a vertex *v*, is:
```C++
visit(v);
for (each vertex w adjacent to v)
  traverse(w);
```

#### Breadth-First Algorithm(BFS)
We use a **Queue** to implement **breadth-first** traversal. Its action, when it reaches a vertex *v*, is:
```C++
q.append(v);
while (!q.empty()) {
  q.retrieve(w);
  visit(w);
  for (all x adjacent to w) {
    q.append(x);
  }
  q.serve();
}
```

### Implementation
There are several ways to represent a graph: **Set**, **Adjacency Table**, **Adjacency List**. Here I'd like to use an **adjacency list** with mixed list.
```C++
typedef int Vertex;
template <int max_size>
class Digraph {
public:
  void insert(const Vertex &start, const Vertex &end);
  void depth_first(void (*visit)(Vertex &x)) const;
  void breadth_first(void (*visit)(Vertex &x)) const;
private:
  int count; // number of vertices, at most max_size
  LinkedList<Vertex> neighbors[max_size];
  // auxilary funcitons
  void traverse(Vertex &v, bool visited[], void (*visit)(Vertex &)) const;
};
```
**neighbors** are an array of *LinkedList*. Each index of **neighbors** represents a vertex, and each *LinkedList* store the vertices which are adjacent to the present vertex.

#### insert(**const** Vertex &start, **const** Vertex &end)
*precondition*: None.
*postcondition*: If *start* and *end* are both valid, *end* will be added to the list of *start*.
```C++
template <int max_size>
void Digraph<max_size>::insert(const Vertex &start, const Vertex &end) {
  // 确保输入的两个点是合法的
  if (start < 0 || start >= max_size || end < 0 || start >= max_size) {
    return;
  }
  for (int i = 0; i < neighbors[start].size(); i++) {
    Vertex tmp;
    // 在start相邻的节点list中找到end，无需再操作
    neighbors[start].retrieve(i, tmp);
    if (end == tmp) {
      return;
    }
  }
  neighbors[start].insert(neighbors[start].size(), end);
  cout << "insert succeed!" << endl;
}
```

#### depth_first(void (*visit)(Vertex &x))
*precondition*: None.
*postcondition*: The function \*visit has been performed at each vertex of the *Digraph* in depth-first order.
```C++
template <int max_size>
void Digraph<max_size>::depth_first(void (*visit)(Vertex &x)) const {
  bool visited[max_size];
  Vertex v;
  for (int i = 0; i < max_size; i++) {
    visited[i] = false;
  }
  for (int i = 0; i < max_size; i++) {
    if (!visited[i]) {
      traverse(i, visited, visit);
    }
  }
}
```

#### traverse(Vertex &v, bool visited[], void (*visit)(Vertex &))
*precondition*: *v* is a vertex of the *Digraph*
*postcondition*: The depth-first traversal, using function \*visit has been completed for *v* and for all vertices that can be reached from *v*.
```C++
template <int max_size>
void Digraph<max_size>::traverse(Vertex &v, bool visited[], void (*visit)(Vertex &x)) const {
  // Pre: v是当前节点，visited是用于记录遍历的数组
  Vertex w;
  visited[v] = true; // 将v标为已遍历
  (*visit)(v);
  // 对v的相邻节点list进行遍历
  for(int i = 0; i < neighbors[v].size(); i++) {
    neighbors[v].retrieve(i, w);
    if (!visited[w]) {
      traverse(w, visited, visit);
    }
  }
}
```

#### breadth_first(void (*visit)(Vertex &x))
*precondition*: None.
*postcondition*: The function \*visit has been performed at each vertex of the *Digraph* in breadth_first order.
```C++
template <int max_size>
void Digraph<max_size>::breadth_first(void (*visit)(Vertex &x)) const {
  LinkedQueue<Vertex> q;
  bool visited[max_size];
  Vertex v, w, x;
  for (v = 0; v < max_size; v++) {
    visited[v] = false;
  }
  for (v = 0; v < max_size; v++) {
    if (!visited[v]) {
      q.append(v);
      while (!q.empty()) {
        q.retrieve(w);
        if (!visited[w]) {
          visited[w] = true;
          (*visit)(w);
          for (int i = 0; i < neighbors[w].size(); i++) {
            neighbors[w].retrieve(i, x);
            if (!visited[x]) {
              q.append(x);
            }
          }
        }
        q.serve();
      }
    }
  }
}
```

In this post I only give a brief introduction about **graph**. However, **graph** can be used to deal with many problems such as the **shortest path problem**. Thank you so much!
