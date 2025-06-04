# IMT-Airport-Manager

README.md

## Information

- Project Name: **IMT-Airport-Manager**
- Institute: **University of Bolton - Greater Manchester**
- Module: **SWE5002 - Data Structures and Algorithms**
- Assessment: **1**
- Instructor: **Stylianos Angelopoulos**
- Student: **Ioannis Marios Tsiakoulias**
- Notes: **Code repository for assessment 1**

---

## Description

This program simulates the operation of a small airport with low traffic and one airway, maintaining one queue for planes requesting **landing** and one for planes requesting **takeoff**.
Based on airport policy, planes landing always have priority over planes taking off, to minimize fuel usage and reduce the overal risk.
A landing airplane may declare an emergency, request which is classified as an **emergency landing**. In that special case, the plane is been given top priority and will have the airway available immediately.
Messages between the plane pilots and the control tower are logged, ensuring proper functionality and correct implementation if the objectives.


## Core Features

The program performs the following:
- **Randomized Plane Data**: Generates planes (in variable sized batches) with random characteristics. They can request landing (normal or emergency) or takeoff (normal only). 
- **Queue Structure**: Keeps track of the planes in two internal queues and one concatenated (global) queue.
- **FIFO Management**: Ensures FIFO (First-In-First-Out) order among plane requesting actions.
- **Emergency Priority**: Prioritizes emergency landings over all other planes. If multiple emergency planes occur, FIFO is again ensured.
- **Visual Feedback**: Provides a visual loading bar to simulate runway activity.
- **Wait Time Tracking**: Calculates how long each plane waits before being cleared.
- **Report Sumary**: Displays the average wait times for each plane condition.
- **Debug Mode**: Offers a debug mode for detailed internal state tracking.

---

## Requirements

- Python 3.x
- Built-in modules only: `random`, `time`
- No external dependencies required
