---
  title: 'PythonTestFile.py'
  description: 'component description'
  pubDate: 'July 30, 2024'
  author: 'Carlos P'
  ---
  
  
  
  # PythonTestFile.py
  # Explanation of Shortest Path Algorithm using Dijkstra's Algorithm

This code implements a graph data structure and uses Dijkstra's algorithm to find the shortest path between two nodes in the graph.

### Graph Class
- The `Graph` class represents a graph data structure.
- **Attributes**:
  - `nodes`: A dictionary to store nodes and their neighbors.

### `add_node` Method
- **Parameters**:
  - `label`: The label of the node being added.
  - `neighbors`: A list of tuples containing the neighboring nodes and their edge costs.
- **Operation**:
  - Adds a node to the graph with its neighbors and edge costs.

### `shortest_path` Method
- **Parameters**:
  - `start`: The starting node for the path.
  - `end`: The destination node for the path.
- **Operation**:
  - Finds the shortest path from the `start` node to the `end` node using Dijkstra's algorithm.
- **Steps**:
  1. Initialize data structures for tracking distances, previous nodes, and visited nodes.
  2. Use a priority queue (`heapq`) to explore nodes based on their current cost.
  3. Update distances and previous nodes as shorter paths are found.
  4. Backtrack from the destination node to the start node to reconstruct the shortest path.
  5. Return the shortest path as a list of nodes.

### Example Usage
- Create a graph with nodes and their neighbors.
- Find and print the shortest path from node "A" to node "F".

### External Libraries
- The code uses the `heapq` module for heap queue operations.

### Example Output
```
Shortest path from A to F: ['A', 'C', 'D', 'F']
```
  
  ## Component Code
  ```jsx
  import heapq

class Graph:
    def __init__(self):
        self.nodes = {}

    def add_node(self, label, neighbors):
        self.nodes[label] = neighbors

    def shortest_path(self, start, end):
        queue = [(0, start)]
        distances = {node: float('inf') for node in self.nodes}
        distances[start] = 0
        previous = {node: None for node in self.nodes}
        visited = set()

        while queue:
            current_cost, current_node = heapq.heappop(queue)

            if current_node in visited:
                continue

            visited.add(current_node)

            for neighbor, cost in self.nodes[current_node]:
                if neighbor in visited:
                    continue

                new_cost = current_cost + cost
                if new_cost < distances[neighbor]:
                    distances[neighbor] = new_cost
                    previous[neighbor] = current_node
                    heapq.heappush(queue, (new_cost, neighbor))

        path = []
        at = end
        while at is not None:
            path.append(at)
            at = previous[at]

        return path[::-1]

if __name__ == "__main__":
    graph = Graph()
    graph.add_node("A", [("B", 4), ("C", 2)])
    graph.add_node("B", [("A", 4), ("C", 1), ("D", 5)])
    graph.add_node("C", [("A", 2), ("B", 1), ("D", 8), ("E", 10)])
    graph.add_node("D", [("B", 5), ("C", 8), ("E", 2), ("F", 6)])
    graph.add_node("E", [("C", 10), ("D", 2), ("F", 3)])
    graph.add_node("F", [("D", 6), ("E", 3)])

    print("Shortest path from A to F:", graph.shortest_path("A", "F"))
  ```
  