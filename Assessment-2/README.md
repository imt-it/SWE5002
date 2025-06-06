# IMT-Airport-Manager

README.md

## Information

- Project Name: **IMT-Airport-Manager**
- Institute: **University of Bolton - Greater Manchester**
- Module: **SWE5002 - Data Structures and Algorithms**
- Assessment: **2**
- Instructor: **Stylianos Angelopoulos**
- Student: **Ioannis Marios Tsiakoulias**
- Notes: **Code repository for assessment 2**

---

## Description

This program models the Singapore Metro system using a graph data structure. It simulates the MRT and LRT network, allowing users to find the **shortest path** (by number of stops or by distance) and the **fastest route** (by travel time) between any two stations. The system loads real-world data from CSV files (one for the stations and another for their connections on a per-line basis) and supports various connection types, including direct (same line), internal branch (same line branching), internal connection (same station acting as hub for multiple lines), and external connection (distant hub).

The application uses **Dijkstra’s Algorithm** for pathfinding and the **Haversine formula** to calculate distances between stations based on their coordinates. It also supports randomized travel times and fixed transfer delays to simulate realistic conditions.

## Core Features

The program performs the following:
- **Graph Representation**: Each station is a node, connections are edges, either unweighted or weighted based on distance or time.
- **CSV Data Integration**: Loads station and connection data from structured CSV files. Those files have been manually edited for optimal results and compatibility.
- **Pathfinding Algorithms**:
  - **Shortest Path**: Based on the fewest number of stops (unweighted).
  - **Fastest Route**: Based on travel time (weighted by distance and transfer delays).
- **CLI Options**:
  - Choose between different pathfinding modes.
  - Random station selection for testing.
  - List all stations by line.
- **Debug Mode**: Optional debug output for development and testing.
- **Realistic Time Simulation**:
  - Optional realistic time prediction, based on real-world average speeds for MRT and LRT trains.
- **Extensible Design**:
  - Modular classes for CSV handling, station management, and graph operations.
  - Easily adaptable for other metro systems or datasets.
  **Minimalistic Design**
  - Simple and clean interface with light color accents and line separation.

---

## Requirements
- Python 3.x
- Built-in modules: `random`, `math`, `csv`
- External modules: `networkx`
- The CSV files from this repository
