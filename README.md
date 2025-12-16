# Vision-Based Environment Mapping and RRT* Path Planning

This repository contains the code for the ME 556 Advanced Robotics project
**“Vision-Based Environment Mapping and RRT\* Path Planning in Simulation.”**
The goal is to convert a 2D image of an environment into a binary occupancy
grid and then use the RRT* algorithm to compute a collision-free path between
a start and a goal location.

## 1. How to Run the Code

This project was developed and tested using Spyder IDE version 6.0.3 (standalone) with Python 3.11.10 on Windows 10.

### Running in Spyder (Recommended)

1. Download or clone the repository.

2. Ensure the following files are located in the **same directory**:
   - `Code.py`
   - `map_example1.png`
   - `map_example2.png`

3. Open **Spyder** and open `Code.py`.

4. Run the script.

5. A successful run will display the following:
   - the grayscale image,
   - the binary occupancy grid, and
   - the RRT* tree and final optimal path.

### Selecting the Map

- By default, the code uses:
  ```python
  image_path = "map_example1.png"
- To run the second map, change this line to:
  ```python
  image_path = "map_example2.png"
  
## 2. Project Overview

The pipeline has two main stages:

1. **Vision-based mapping**
   - Load the input map image in grayscale using OpenCV
   - Apply a global threshold to obtain a binary image:
     - pixels with intensity ≤ threshold → obstacles (black)
     - pixels with intensity > threshold → free space (white)
   - Build a 2D NumPy occupancy grid where `1 = obstacle` and `0 = free`.

2. **RRT\* motion planning**
   - Plan directly in pixel coordinates on the occupancy grid.
   - Nodes store `(x, y)` position, cumulative cost, and parent index.
   - The algorithm performs:
     - sampling in free space with a small goal bias,
     - nearest-neighbor search,
     - steering with a fixed step size,
     - collision checking along each edge,
     - cost-based parent selection,
     - rewiring of nearby nodes for path optimality.
   - A path is accepted when a node enters the goal region; the final path
     is extracted by following parent pointers.

The visualization flips the grid vertically so the plot uses a conventional
Cartesian orientation (x to the right, y upward) while the internal indexing
still uses image coordinates (row 0 at the top).

## 3. File Structure

- Main script contains:
  - image loading and thresholding (`load_occupancy_grid`)
  - RRT* implementation (`rrt_star` and helper functions)
  - visualization (`plot_rrt_star_result`)
  - `main()` function that runs the full pipeline on a chosen map.

- `map_example1.png`  
  Color–shape environment (simple geometric obstacles with different colors).

- `map_example2.png`  
  Nested maze environment (complex geometry with narrow corridors).

## 4. Dependencies

This project uses Python 3.11.10 and the following libraries:

- `opencv-python` (cv2) – image loading, grayscale conversion, thresholding
- `numpy` – numerical operations and occupancy grid representation
- `matplotlib` – plotting the occupancy grid, RRT* tree, and final path
- `dataclasses` – for the `Node` data structure (Python 3.7+)

To install them (for example using `pip`):

```bash
pip install opencv-python numpy matplotlib
# ME-556_Advanced-Robotics_Final-Project
Vision-Based Environment Mapping and RRT* Path Planning in Simulation
