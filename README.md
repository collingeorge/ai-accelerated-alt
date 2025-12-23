# Bidirectional A* with Landmarks and Triangle Inequality (ALT) Algorithm

**Research-Grade Implementation for Shortest Path Problems**

![Status](https://img.shields.io/badge/Status-Educational%20Project-blue)
![Project Type](https://img.shields.io/badge/Project-Pre--Medical%20Research-blue)
![Language](https://img.shields.io/badge/Language-Python-green)
![Version](https://img.shields.io/badge/Version-1.0-green)
![License](https://img.shields.io/badge/License-MIT-blue)

---

## Educational Use Only

This repository contains educational materials developed for computer science learning and medical school application portfolio purposes.

**This is not:**
- Production-ready software for commercial or mission-critical applications
- Peer-reviewed academic research or published algorithm development
- Professional software engineering work
- Validated for large-scale deployment (tested up to ~10K nodes)

**This is:**
- Independent educational project exploring algorithm design and analysis
- Demonstration of computational thinking and statistical validation methodology
- Medical school application portfolio material showing analytical skills
- Learning exercise in AI-assisted algorithm development

---

## Disclaimers

**Software Maturity:**  
This is an educational implementation developed for learning purposes. While statistically validated on test datasets, it has NOT undergone:
- Production-level code review or security audit
- Large-scale testing (>10K nodes)
- Optimization for real-world deployment
- Formal verification or proof of correctness

**Performance Claims:**  
Speedup measurements (up to 8× vs. Dijkstra) are:
- Based on specific test configurations (grid, road network graphs up to ~10K nodes)
- May not generalize to all graph types or scales
- Include preprocessing costs in measurements
- Statistically validated on test datasets but not universally applicable

**Use in Production Systems:**  
This code is for educational purposes. Production use would require:
- Professional code review and testing
- Optimization for specific use cases
- Validation on target graph sizes and types
- Appropriate error handling and edge case coverage

**Liability:**  
This software is provided "as is" without warranty of any kind. Users assume full responsibility for any use of this code.

**Author Status:**  
Pre-medical student with computer science background. Not a professional software engineer or algorithm researcher.

---

## Overview

This project implements a bidirectional A* search algorithm enhanced with landmarks and triangle inequality preprocessing (ALT) for single-pair shortest path (SPSP) problems. The methodology generalizes to repeated queries (multi-pair) with amortized preprocessing costs.

**Educational Purpose:**  
This work demonstrates:
- Understanding of graph algorithms and data structures
- Ability to implement and validate computational methods
- Statistical analysis and experimental methodology
- AI-assisted algorithm development and testing
- Technical communication and documentation

**Algorithm Description:**  
ALT combines three key techniques:
1. **Bidirectional Search:** Simultaneous forward and backward exploration from source and target
2. **Landmark Preprocessing:** Strategic selection of reference nodes for distance estimation
3. **Triangle Inequality Heuristics:** Lower bounds on distances using precomputed landmark distances

**Use Cases (Theoretical):**
- Urban planning and robotics (grid networks)
- Navigation and logistics (road networks)
- Social network analysis (scale-free graphs)

---

## Performance Summary

### Experimental Results

**Tested on five graph types with 30 trials per configuration:**

| Graph Type | Nodes | Speedup vs Dijkstra | Statistical Significance | Use Case |
|------------|-------|---------------------|-------------------------|----------|
| **Grid Networks** | 100-1000 | 8.0× | p < 0.001 | Urban planning, robotics |
| **Road Networks** | ~9K | 7.05× | p < 0.001 | Navigation, logistics |
| **Scale-Free** | 1000 | 4.44× | p < 0.001 | Social networks |
| **Random Graphs** | 100 | 1.08× | p = 0.32 | General graphs |
| **Chain (worst-case)** | 1000 | 6.94× | p < 0.001 | Pathological cases |

**Important Notes:**
- All measurements include preprocessing costs
- Results averaged over 30 trials with statistical testing
- Tested up to ~10,000 nodes; scalability beyond this unknown
- Performance highly dependent on graph structure
- Preprocessing time increases with graph complexity

---

## Algorithm Features

### Core Implementation

**Bidirectional Search:**
- Simultaneous forward search from source and backward search from target
- Termination when search frontiers meet
- Path reconstruction from meeting point

**Landmark Preprocessing:**
- Strategic selection of k landmark nodes
- Precomputation of shortest distances from all nodes to landmarks
- Triangle inequality lower bounds for A* heuristic

**Optimizations:**
- Priority queue management for both search directions
- Efficient path reconstruction with cycle detection
- Memory-optimized data structures

**Complexity Analysis:**
- Preprocessing: O(k² × (m + n log n)) where k = number of landmarks, n = nodes, m = edges
- Query: O((n + m) log n) worst-case, much faster in practice on structured graphs
- Space: O(k × n) for landmark distance storage

---

## Repository Structure
```
bidirectional-alt/
├── README.md                           # This file
├── LICENSE                             # MIT License
├── CITATION.cff                        # Citation metadata
├── src/
│   ├── alt_algorithm.py                # Core ALT implementation
│   ├── graph_generators.py             # Test graph generation
│   ├── benchmark.py                    # Performance testing
│   └── visualization.py                # Result plotting
├── tests/
│   ├── test_correctness.py             # Algorithm correctness tests
│   └── test_performance.py             # Performance benchmarks
├── data/
│   ├── grid_results.csv                # Grid network benchmark data
│   ├── road_results.csv                # Road network benchmark data
│   └── statistical_analysis.csv        # Statistical test results
├── notebooks/
│   └── analysis.ipynb                  # Jupyter notebook with full analysis
└── docs/
    ├── methodology.md                  # Detailed experimental methodology
    └── results.md                      # Complete results and analysis
```

---

## Installation and Usage

### Requirements
```
Python 3.8+
numpy >= 1.21.0
scipy >= 1.7.0
networkx >= 2.6.0
matplotlib >= 3.4.0
pytest >= 6.2.0 (for testing)
```

### Installation
```bash
git clone https://github.com/collingeorge/bidirectional-alt.git
cd bidirectional-alt
pip install -r requirements.txt
```

### Basic Usage
```python
from src.alt_algorithm import BidirectionalALT
from src.graph_generators import generate_grid_graph

# Generate test graph
graph = generate_grid_graph(width=50, height=50)

# Initialize ALT with 5 landmarks
alt = BidirectionalALT(graph, num_landmarks=5)

# Find shortest path
source, target = 0, 2499  # Opposite corners of 50×50 grid
path, distance = alt.shortest_path(source, target)

print(f"Path length: {distance}")
print(f"Path: {path}")
```

### Running Benchmarks
```bash
# Run full benchmark suite (30 trials per configuration)
python src/benchmark.py

# Run specific graph type
python src/benchmark.py --graph-type grid --size 100

# Run with custom parameters
python src/benchmark.py --landmarks 10 --trials 50
```

### Running Tests
```bash
# Run correctness tests
pytest tests/test_correctness.py

# Run performance benchmarks
pytest tests/test_performance.py

# Run all tests
pytest tests/
```

---

## Methodology

### Graph Generation

**Five graph types tested:**

1. **Grid Networks:** n×n grids with 4-connected neighbors
2. **Road Networks:** Real road network data (OpenStreetMap)
3. **Scale-Free:** Barabási-Albert model (preferential attachment)
4. **Random Graphs:** Erdős-Rényi model
5. **Chain Graphs:** Pathological worst-case (linear structure)

### Landmark Selection

**Strategy:** Farthest-first traversal
1. Select random initial landmark
2. Iteratively select node farthest from existing landmarks
3. Repeat until k landmarks chosen

**Rationale:** Maximizes landmark coverage of graph space

### Statistical Validation

**Per configuration:**
- 30 independent trials
- Welch's t-test for speedup significance (unequal variance assumption)
- Significance threshold: α = 0.05 (p < 0.05 considered significant)
- Effect size reported (Cohen's d)

**Controlled Variables:**
- Random seed fixed per trial for reproducibility
- Source/target pairs randomly selected but consistent across algorithms
- Graph structure held constant within trial

---

## Results and Analysis

### Key Findings

**Structured Graphs (Grids, Roads):**
- Consistently large speedups (7-8×) vs. Dijkstra
- High statistical significance (p < 0.001)
- Landmark heuristics highly effective due to spatial structure

**Scale-Free Graphs:**
- Moderate speedups (4.4×)
- Hub nodes serve as effective landmarks
- Bidirectional search benefits from power-law degree distribution

**Random Graphs:**
- Minimal speedup (1.08×)
- Not statistically significant (p = 0.32)
- Lack of structure limits landmark effectiveness

**Pathological Cases (Chains):**
- Surprisingly strong performance (6.9×)
- Bidirectional search divides problem effectively
- Demonstrates robustness beyond typical use cases

### Performance Characteristics

**Preprocessing Overhead:**
- Grid (1000 nodes, 5 landmarks): ~0.2s
- Road network (9000 nodes, 5 landmarks): ~1.5s
- Amortized over multiple queries in practical applications

**Memory Usage:**
- O(k × n) for landmark distances
- Grid (1000 nodes, 5 landmarks): ~40 KB
- Acceptable for graphs up to 10⁴-10⁵ nodes

**Scalability Observations:**
- Tested up to ~10,000 nodes
- Performance beyond this scale unknown
- Preprocessing time increases quadratically with landmarks

---

## Limitations

### Algorithmic Limitations

**Graph Structure Dependency:**
- Performance highly dependent on graph topology
- Minimal benefit on unstructured random graphs
- Preprocessing cost may exceed benefits for single queries

**Scalability:**
- Tested only up to ~10,000 nodes
- Preprocessing complexity O(k² × (m + n log n)) limits very large graphs
- Memory requirements scale with graph size and landmark count

**Landmark Selection:**
- Heuristic selection (not provably optimal)
- Performance sensitive to landmark placement
- No adaptive landmark selection implemented

### Implementation Limitations

**Code Maturity:**
- Educational implementation, not production-hardened
- Limited error handling for edge cases
- Not optimized for maximum performance (clarity prioritized)

**Testing Scope:**
- Tested on synthetic and small real-world graphs
- Large-scale real-world validation not performed
- Edge case coverage incomplete

**Generalization:**
- Results specific to tested graph types and sizes
- Performance on other graph classes unknown
- Hyperparameter tuning (landmark count) not exhaustive

---

## Future Directions

### Algorithmic Improvements

**Adaptive Landmark Selection:**
- Query-dependent landmark placement
- Online landmark optimization
- Machine learning for landmark prediction

**Advanced Heuristics:**
- Combining ALT with other preprocessing (contraction hierarchies)
- Graph partitioning for hierarchical search
- Approximate distance oracles

**Parallelization:**
- Multi-threaded bidirectional search
- Parallel preprocessing
- GPU acceleration for large-scale graphs

### Experimental Extensions

**Larger Scale Testing:**
- Graphs with 10⁵-10⁶ nodes
- Continental-scale road networks
- Large social network datasets

**Additional Graph Types:**
- Temporal graphs (time-dependent weights)
- Dynamic graphs (changing topology)
- Uncertain graphs (probabilistic edges)

**Alternative Applications:**
- Multi-pair shortest paths (batch queries)
- All-pairs shortest paths with approximation
- k-shortest paths variants

**Note:** These represent areas for future development, not current capabilities.

---

## Citation

### Recommended Citation

**Vancouver Style:**
```text
George CB. Bidirectional A* with Landmarks and Triangle Inequality (ALT) 
Algorithm: Research-Grade Implementation. Published 2025. Available from: 
https://github.com/collingeorge/bidirectional-alt 
[Accessed: date]
```

**APA Style:**
```text
George, C. B. (2025). Bidirectional A* with landmarks and triangle inequality 
(ALT) algorithm: Research-grade implementation. 
https://github.com/collingeorge/bidirectional-alt
```

**BibTeX:**
```bibtex
@software{george2025alt,
  author = {George, Collin B.},
  title = {Bidirectional A* with Landmarks and Triangle Inequality (ALT) Algorithm: Research-Grade Implementation},
  year = {2025},
  url = {https://github.com/collingeorge/bidirectional-alt},
  note = {Educational software project}
}
```

---

## Author Information

**Author:** Collin B. George, BS  
**Project Type:** Independent educational software project  
**Educational Context:** Algorithm design, implementation, and statistical validation  
**Status:** Preparing for medical school matriculation 2026  
**Background:** Computer science, algorithms, data structures

**GitHub:** github.com/collingeorge  
**ORCID:** 0009-0007-8162-6839  
**License:** MIT

---

## Acknowledgments

**Development Methodology:**  
This project was developed using AI-assisted coding techniques, demonstrating efficient algorithm implementation and testing through human-AI collaboration.

**Educational Support:**  
The author is grateful to University of Washington faculty for computer science education in algorithms, data structures, and statistical analysis that informed this work.

**Data Sources:**  
Road network data from OpenStreetMap (© OpenStreetMap contributors, ODbL license)

This work represents independent educational exploration and does not constitute professional algorithm research or commercial software development.

---

## License

This project is licensed under the MIT License.

**Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software.**

**The software is provided "as is", without warranty of any kind, express or implied.**

See [LICENSE](LICENSE) file for full license text.

© 2025 Collin B. George

---

## Keywords

Shortest Path, Graph Algorithms, A* Search, Bidirectional Search, Landmarks, Triangle Inequality, Algorithm Design, Performance Analysis, Statistical Validation, Python, Educational Software
