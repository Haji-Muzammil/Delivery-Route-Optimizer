# PROJECT PROPOSAL
**Semester Lab Project · Weightage 1 mark**

## Group Information
| Role | Name | Roll Number | Section |
| :--- | :--- | :--- | :--- |
| **Leader** | Haji Muzammil | 24k-2057 | BCY-5A |
| **Member 2** | Zakir Hussain | 24k-2045 | BCY-5A |

| Field | Details |
| :--- | :--- |
| **Project Title** | Delivery Route Optimizer on a Real City Map |
| **Project Idea** | Idea No. 14 from the Project Ideas list ☑ |
| **Category** | Part B — Applied / Data ☑ |
| **GitHub Repository URL** | https://github.com/Haji-Muzammil/Delivery-Route-Optimizer |
| **Date of Submission** | October 21, 2026 |

---

## 1. Project Title and Summary
**Delivery Route Optimizer on a Real City Map**  
This project is an intelligent route optimization application that computes the most efficient path for a delivery driver navigating through 10 to 20 delivery addresses in a real city. Instead of relying on abstract toy graphs, the software fetches real road network data for cities like Lahore, Karachi, or Islamabad using OpenStreetMap data via OSMnx. It will be utilized by logistics operators or delivery riders seeking to minimize total travel distance and fuel consumption. To accomplish this, the system applies the A* search algorithm to compute exact shortest driving paths between individual waypoints, and subsequently deploys local search techniques—including Simulated Annealing and Genetic Algorithms—to solve the overarching Traveling Salesperson Problem (TSP). Ultimately, an interactive Folium map visualizes the computed optimal driving route for the driver.

## 2. Problem Statement and Motivation
The project addresses the routing challenge inherent in 'last-mile' delivery logistics, formulated as a Traveling Salesperson Problem (TSP) on a real-world road network graph. It is exceedingly difficult to solve by brute force because the search space of visiting sequences grows factorially; for just 15 delivery stops, there are over 87 billion (15!) possible permutations. Furthermore, evaluating the true driving distance between any two stops requires pathfinding across thousands of real geographical intersections (nodes) and road segments (edges). Delivery riders, courier services, and postal workers face this daily, currently coping through static route familiarity or manually plotting stops on consumer GPS apps that do not automatically optimize a multi-stop sequence. Solving this is highly worthwhile because optimized routing significantly reduces fuel consumption, carbon emissions, and operational costs, proving how AI search delivers practical economic value.

## 3. Problem Formulation

### 3.1 PEAS Description
| Component | Description for your project |
| :--- | :--- |
| **Performance measure** | Total route distance in kilometers and algorithm execution time in seconds. |
| **Environment** | Real-world road network graph extracted via OpenStreetMap (OSMnx). |
| **Actuators** | Generated optimal route sequence and interactive Folium HTML map visualization. |
| **Sensors** | Array of geographical coordinates (latitude/longitude) representing delivery stops. |

### 3.2 Environment and Agent Type
| Property | Classification |
| :--- | :--- |
| **Observability** | Fully observable ☑ |
| **Determinism** | Deterministic ☑ |
| **Change over time** | Static ☑ |
| **State and actions** | Discrete ☑ |
| **Agents** | Single-agent ☑ |
| **Agent type** | Goal-based ☑ |

### 3.3 Problem Representation
| Element | Your representation |
| :--- | :--- |
| **State representation** | An ordered list/array of delivery stop node IDs representing the current route permutation. |
| **Actions / moves** | Swapping two delivery stops in the sequence, or mutating a sub-segment of the route. |
| **Initial state** | A randomly generated valid permutation sequence of all delivery stops. |
| **Goal or terminal test** | Reaching the maximum iteration/generation limit or temperature cooling to zero. |
| **Cost, utility, fitness** | Total cumulative driving distance of the route computed via pre-calculated A* paths (lower is better). |

## 4. Proposed AI Approach
| # | Technique | Role in the project | Own code or library |
| :--- | :--- | :--- | :--- |
| 1 | A* Search | Calculates exact shortest driving distance between pairs of stops on the road graph. | Own code |
| 2 | Simulated Annealing | Explores the TSP route permutation space, accepting worse moves initially to escape local minima. | Own code |
| 3 | Genetic Algorithm | Evolves a population of route permutations using crossover and mutation operators. | Own code |
| B | Baseline: Hill Climbing | Greedy local search that only accepts route swaps that strictly decrease total distance. | Own code |

These techniques suit the problem because it involves two distinct search layers. The first layer is point-to-point pathfinding on a road graph; A* is optimal here using Haversine straight-line distance as an admissible heuristic. The second layer is optimizing the visiting order of 15–20 stops (TSP). Since exact TSP solvers scale poorly, local search algorithms (Simulated Annealing, Genetic Algorithm, and Hill Climbing) provide excellent near-optimal solutions. To ensure high performance, A* is used to pre-compute an N×N distance matrix once, allowing local search agents to evaluate route fitness in O(1) time.

### 4.1 Planned Class Design
- **MapEnvironment:** Loads and holds the OSMnx graph. Key methods: `load_graph()`, `get_random_nodes()`, `save_cache()`.
- **PathFinder:** Implements point-to-point pathfinding. Key methods: `astar_search()`, `haversine_heuristic()`, `build_distance_matrix()`.
- **OptimizerAgent (Abstract Base Class):** Defines the TSP interface. Attributes: `distance_matrix`, `best_route`, `best_cost`. Key methods: `optimize()`.
- **HillClimbingAgent (inherits OptimizerAgent):** Implements greedy hill climbing route search by neighbor swapping.
- **AnnealingAgent (inherits OptimizerAgent):** Implements simulated annealing with exponential cooling schedule.
- **GeneticAgent (inherits OptimizerAgent):** Implements population-based GA with order crossover (OX) and swap mutation.

## 5. Scope
### In scope
| # | Feature | Why it is essential |
| :--- | :--- | :--- |
| 1 | Real Road Network Integration via OSMnx | Transforms abstract graph search into a practical real-world applied scenario. |
| 2 | Custom A* Search Implementation | Computes realistic driving distances instead of straight-line Euclidean estimates. |
| 3 | Hill Climbing, SA, and GA Solvers | Fulfills the core AI requirement of comparing multiple search techniques against a baseline. |
| 4 | Interactive Folium Map Visualization | Renders the optimized path clearly on a real geographic map layout. |
| 5 | Offline Data Caching Logic | Ensures the application runs smoothly without internet dependency during evaluation. |

### Out of scope
Real-time dynamic traffic updates, turn-by-turn navigation voice guidance, multi-vehicle fleet routing (VRP), and dynamic mid-route stop insertions are deliberately excluded to keep the project focused on core search algorithms.

## 6. Data or Environment Source
| Field | Details |
| :--- | :--- |
| **Source name and URL** | OpenStreetMap road graph data fetched via Python OSMnx library (https://openstreetmap.org) |
| **Type** | Public dataset ☑ |
| **Access requirements** | No key needed ☑ |
| **Approximate size** | 10–50 MB per downloaded city bounding box network (`.graphml` format) |
| **Licence / terms of use** | Open Data Commons Open Database License (ODbL) |
| **Offline fallback** | Downloaded city road graphs are saved locally as `.graphml` files in `data/raw/`. The application loads these cached files when network access is unavailable. |

## 7. Evaluation Plan
| Field | Details |
| :--- | :--- |
| **What will be compared** | Hill Climbing (Baseline) vs. Simulated Annealing vs. Genetic Algorithm |
| **Metric(s)** | Total route distance (km), execution time (seconds), and convergence rate |
| **Baseline** | Hill Climbing (greedy local search) |
| **Test instances / test set** | 3 benchmark city maps (Karachi, Lahore, Islamabad) evaluated across 10, 15, and 20 delivery stops |
| **Number of runs and seed** | 10 runs per test instance using fixed random seed = 42 |
| **Planned charts** | 1. Route Distance Comparison Bar Chart<br>2. Iteration vs. Fitness Convergence Line Plot<br>3. Interactive Folium Route Map Overlay |

## 8. Division of Work
| Member | Modules / components owned | Approximate share |
| :--- | :--- | :--- |
| **Haji Muzammil** | Map environment integration (OSMnx), A* pathfinding algorithm, distance matrix pre-computation, offline caching system (`src/environment.py`, `src/search.py`). | 50% |
| **Zakir Hussain** | TSP Optimizer Agents (Hill Climbing, Simulated Annealing, GA), experiment execution runner, Folium map visualization, result charts (`src/agents.py`, `src/experiments.py`, `src/visualise.py`). | 50% |

## 9. Milestones and Timeline
| # | Milestone | Target date | Owner |
| :--- | :--- | :--- | :--- |
| 1 | OSMnx map graph loading with offline caching and basic Folium map rendering working. | Week 11 | Haji Muzammil |
| 2 | Custom A* search algorithm computing node-to-node shortest paths and filling distance matrix. | Week 12 | Haji Muzammil |
| 3 | Hill Climbing baseline and Simulated Annealing agents optimizing 15-stop routes. | Week 13 | Zakir Hussain |
| 4 | Genetic Algorithm agent running; benchmark experiment runner generating comparison charts. | Week 14 | Zakir Hussain |
| 5 | Experiments complete, final integration, documentation and repository cleanup. | Week 15 | Both |

## 10. Expected Outcome and Risks
A working submission will prompt the user to select a city and number of delivery stops. The system will compute the optimal visiting order, display terminal logs of distance reduction per algorithm, and open an interactive Folium map tracing the complete path. We expect Simulated Annealing and GA to achieve 15–25% shorter route distances than Hill Climbing.

**Risk 1:** Fetching large map graphs from OpenStreetMap live may fail or time out during evaluation.  
*Mitigation:* Pre-download and commit cached `.graphml` files in `data/raw/` so the program runs entirely offline.

**Risk 2:** Re-running A* for every fitness check in GA/SA would cause severe computational slowdown.  
*Mitigation:* Pre-compute an N×N distance matrix using A* once before optimization, allowing O(1) fitness lookups.
