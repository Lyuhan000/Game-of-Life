# CSA Coursework: Game of Life

This repository contains implementation of John Conway’s Game of Life in Go, completed as part of the Computer Systems A summative coursework at the University of Bristol. The project focuses on applying parallel and distributed computing concepts, exploring concurrency, scalability, and network communication.

The implementation consists of two main stages:  
1. **Parallel Implementation** – Multi-threaded Game of Life using goroutines and channels.  
2. **Distributed Implementation** – Multi-node Game of Life using a client–server model over RPC.

---

## Stage 1 – Parallel Implementation

### Features
- Implements Game of Life rules on a 2D grid with toroidal boundaries.
- Utilises goroutines for concurrent processing of grid segments.
- Workload distribution based on dividing the grid into independent row ranges.
- Synchronisation using `sync.WaitGroup` to avoid race conditions.
- SDL visualisation with interactive controls:
  - `s`: Save current state as a PGM image.
  - `p`: Pause or resume the simulation.
  - `q`: Quit and save current state.
- Benchmark tests with grids of 512×512 cells over 1000+ iterations using 1–16 worker goroutines.

### Performance Summary
- Clear performance gains when increasing worker threads up to an optimal point.
- Best performance observed with 16 workers for the benchmark setup.
- Performance plateau occurs beyond optimal worker count due to overhead from context switching and synchronisation.

---

## Stage 2 – Distributed Implementation

### Features
- **Client–Server Architecture**:
  - **GOL Engine (Server)**: Performs all Game of Life computations.
  - **Local Controller (Client)**: Sends parameters, receives results, and controls execution.
- **RPC Communication**:
  - Transmits grid state data and parameters between client and server.
  - Supports multiple servers for workload distribution.
- **Control Commands**:
  - `s`: Save current state.
  - `q`: Disconnect client without stopping the server.
  - `k`: Shut down all servers and save final state.
  - `p`: Pause or resume processing with turn number feedback.
- Capability to run multiple servers on different nodes to process different subgrids.

### Performance Summary
- Significant reduction in computation time when increasing workers from 1 to 8.
- Optimal performance observed with 8 distributed workers.
- Diminishing returns beyond optimal point due to network communication overhead.

---

## Performance Highlights
- **Parallel mode** significantly reduces execution time by utilising concurrency.
- **Distributed mode** enables scaling across multiple networked nodes.
- Both implementations pass all required test cases and meet coursework success criteria.

---

## Installation

### Prerequisites
- Go 1.18+
- SDL2 library installed:
  - Linux: `sudo apt install libsdl2-dev`
  - macOS: `brew install sdl2`
  - Windows: WSL2 + Ubuntu

### Clone the Repository
```bash
git clone https://github.com/<your-username>/game-of-life.git
cd game-of-life
