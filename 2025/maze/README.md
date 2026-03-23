# Maze Generator and Solver

## Overview
This project is a browser-based visualization of maze generation and pathfinding. It uses an HTML5 `<canvas>` element to plot a grid-based maze. The application utilizes a randomized depth-first search algorithm to carve the maze and an A* (A-Star) search algorithm to calculate the optimal path from start to finish.

## Core Mechanisms

### Maze Generation
The maze generation is handled by the `generateMaze()` asynchronous function. It models the maze as an 2D array and uses a stack-based Recursive Backtracker (depth-first search) to walk the grid, carving out paths. The visualization occurs by pausing execution via `await new Promise(...)` every few steps to allow the canvas to render the changing state.

```javascript
while (stack.length > 0 && steps < maxSteps) {
    // ... determine unvisited neighbor cells ...
    if (neighbors.length > 0) {
        const next = neighbors[Math.floor(Math.random() * neighbors.length)];
        // Carve wall between current and next
        const wallX = (current[0] + next[0]) / 2;
        const wallY = (current[1] + next[1]) / 2;
        gameState.maze[wallX][wallY] = 0;
        // ... push next to stack ...
    } else {
        stack.pop(); // Backtrack
    }
}
```

### A* Pathfinding (Solver)
Pathfinding is executed by `solveMaze()`. It implements the standard A* graph-search algorithm. It maintains an `openSet` of nodes to be evaluated, selecting the node with the lowest `fScore` (which is `gScore + heuristic`).

```javascript
const tentativeGScore = gScore.get(current.toString()) + 1;

if (!gScore.has(neighbor.toString()) || tentativeGScore < gScore.get(neighbor.toString())) {
    cameFrom.set(neighbor.toString(), current);
    gScore.set(neighbor.toString(), tentativeGScore);
    fScore.set(
        neighbor.toString(),
        tentativeGScore + heuristic(neighbor, end)
    );
    // Add neighbor to openSet to evaluate its neighbors later
    openSet.push(neighbor);
}
```
The heuristic function used for A* is the Manhattan distance between current coordinates and destination coordinates, as diagonal movement is prohibited.

```javascript
function heuristic(a, b) {
    return Math.abs(a[0] - b[0]) + Math.abs(a[1] - b[1]);
}
```

### Canvas Rendering
The `drawMaze()` function iterates over the 2D `gameState.maze` array. Different integers within the array correspond to distinct semantic meanings (e.g., 1 = wall, 0 = path, 2 = start, 3 = end, 4 = solution path) which map to distinct fill colors on the canvas context.
