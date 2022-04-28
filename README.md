Ford-Fulkerson Algorithm
==========================

개념 
---
폴드 풀커스 알고리즘은 유량 네트워크에서 소스와 싱크 사이에 흐를 수 있는 최대 유량(Maximum Flow)을 구하는 알고리즘이다. 유량 네트워크의 모든 간선의 유량을 0으로 두고 시작해서, 소스에서 싱크로 유량을 더 보낼 수 있는 경로(증가 경로)를 찾아 유량을 추가로 보내기를 반복한다. 이렇게 증가 경로를 찾아서 유량을 추가로 흘려보내다가 증가 경로가 존재하지 않으면 네트워크에 최대 유량이 흐르고 있다고 판단하고 알고리즘을 종료한다. 

용어 정리
-------
* Source: 시작점
* Sink: 도착점
* Capacity: 용량 (간선에서 소화 가능한 최대 양 or 값)
* Flow: 유량 간선에서 용량을 점유하고 있는, 사용하고있는 양 or 값
* c(a, b): 정점 a 에서 b로, 소화 가능한(남은) 용량 값
* f(a, b): 정점 a 에서 b로, 사용하고 있는(쓴) 유량 값 

과정
---
1. 각 간선의 용량을 입력받는다.
2. DFS(포드-풀커슨)를 이용하여 r(u,v) > 0인 증가 경로를 찾는다. (DFS 기 때문에 최단경로 아님 )
3. 찾은 증가 경로 상에서 r(u,v)이 가장 낮은 엣지를 찾는다.
4. 찾은 엣지의 r(u,v)만큼만 S에서 T까지 유량을 흘려보낸다(경로의 모든 엣지에 유량 추가).
5. 더 이상 증가 경로가 발견이 되지 않을 때까지 반복한다.

시간복잡도
-------
시간복잡도는 O((V+E)F)이다.

코드
-------------
```python
from collections import defaultdict

class Graph:
 
    def __init__(self, graph):
        self.graph = graph 
        self. ROW = len(graph)
      
    '''Returns true if there is a path from source 's' to sink 't' in
    residual graph. Also fills parent[] to store the path '''
 
    def BFS(self, s, t, parent):
 
        visited = [False]*(self.ROW)
 
        queue = []
 
        queue.append(s)
        visited[s] = True
 
        while queue:
 
            u = queue.pop(0)
 
            for ind, val in enumerate(self.graph[u]):
                if visited[ind] == False and val > 0:
                    queue.append(ind)
                    visited[ind] = True
                    parent[ind] = u
                    if ind == t:
                        return True
 
        return False
     
    def FordFulkerson(self, source, sink):
 
        parent = [-1]*(self.ROW)
 
        max_flow = 0 
 
        while self.BFS(source, sink, parent) :
 
            path_flow = float("Inf")
            s = sink
            while(s !=  source):
                path_flow = min (path_flow, self.graph[parent[s]][s])
                s = parent[s]
 
            max_flow +=  path_flow
 
            v = sink
            while(v !=  source):
                u = parent[v]
                self.graph[u][v] -= path_flow
                self.graph[v][u] += path_flow
                v = parent[v]
 
        return max_flow
 
graph = [[0, 16, 13, 0, 0, 0],
        [0, 0, 10, 12, 0, 0],
        [0, 4, 0, 0, 14, 0],
        [0, 0, 9, 0, 0, 20],
        [0, 0, 0, 7, 0, 4],
        [0, 0, 0, 0, 0, 0]]
 
g = Graph(graph)
 
source = 0; sink = 5
  
print ("The maximum possible flow is %d " % g.FordFulkerson(source, sink))
```
결과
---
```
The maximum possible flow is 23
```


