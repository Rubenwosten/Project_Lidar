# Project_Lidar
Misdetection Risk-Aware Adaptive LiDAR Sensing

This repository contains the codes used for our IEEE VTC 2025 paper
"Misdetection Risk-Aware Adaptive LiDAR Sensing for Automotive Driving."

The project optimizes LiDAR scanning profiles for autonomous vehicles using a spatial risk map and power profile optimization. 
The LiDAR adapts its transmitted power across angular sectors based on real-time risk to reduce the chance of misdetection in high-risk regions.

--------------------------------------------------------------------------------
Project Overview

Conventional automotive LiDARs scan their field of view uniformly. 
Our approach introduces risk-aware adaptation:
- Builds a spatial risk map using lane semantics, object tracking, and information-availabilty data.
- Uses optimization algorithms to distribute LiDAR power across sectors.
- Integrates with the nuScenes dataset for realistic autonomous driving scenarios.

Simulation results show a ~40% reduction in effective misdetection risk compared to uniform scanning.

--------------------------------------------------------------------------------
Installation

1. Clone this repository:
   git clone https://github.com/<your_username>/RiskAwareLiDAR.git
   cd RiskAwareLiDAR

2. Install Nuscenes:
   https://www.nuscenes.org/nuscenes#download
   
3. Add the extra initialization code found in installation.txt to your local nuScenes database setup. 
   This will speed up the map generations.

--------------------------------------------------------------------------------
Running the Simulation

1. Configure dataset and simulation parameters in test.py:
   dataroot = r"C:/path/to/nuscenes"
   map_name = 'boston-seaport'
   LIDAR_RANGE = 100
   RESOLUTION = 0.5
   amount_cones = 8
   max_power = 64

2. Run the full simulation:
   python test.py

3. Results (plots, risk maps, power profiles, etc.) are saved under:
   /Runs/<city_name>/scene_<id>_res=<resolution>/

--------------------------------------------------------------------------------
Output and Visualization

- Risk maps: show static, dynamic, and time-lapse-based risks per cell.
- Power profiles: display optimized vs. uniform power allocation per sector.
- Point clouds: visualize detected vs. missed LiDAR points.
- GIFs: automatically generated for time-series visualizations.

Example output directories:
Runs/
 └── singapore/
      └── scene 6 res=0.5/
           ├── Constant Power/
           ├── Variable Power/
           └── Power Profiles/

---------------------------------------------------------------------------------
Repository Structure

File | Description
-----|-------------
test.py | Main entry point — runs the full LiDAR simulation. Initializes the map, risk model, and power optimization, and manages output folders for “Constant Power” and “Variable Power” runs.
Visualise.py | Handles all visualization tasks — including risk maps, point clouds, occupancy plots, power profiles, and GIF creation of simulation results.
Utility.py | Provides helper utilities to reinitialize or reload grid data, manage output folders, and visualize static risk and ETA maps.
Power.py | Contains the LiDAR power optimization module. Divides the 360° FoV into cones, computes risk per cone, and optimizes power allocation via constrained minimization.
Subsample.py | Implements the LiDAR subsampling algorithm. Simulates realistic point clouds by filtering detections according to power and distance-dependent probability models.
Object_filter.py | Identifies which detected LiDAR points correspond to known objects in the scene, filtering based on bounding boxes and geometric proximity.
Severity.py | Defines collision severity factors based on object type and relative orientation. Used to compute risk weights for pedestrians, vehicles, and static obstacles.
Untitled-1.py | Experimental notebook-style script containing physics-based LiDAR equations and test plots for signal-to-noise ratio and detection probability models.
Cell.py | Defines the basic spatial cell structure used in the grid. Each cell holds risk, occupancy, and LiDAR scan data, and determines its layer type (e.g., lane, walkway).
Grid.py | Implements the spatial grid system that manages all cells in the LiDAR’s field of view. Handles ETA weighting, average risk computation, and spatial range analysis.
Map.py | Loads the nuScenes map and scene data, constructs the simulation environment, manages patch boundaries, and assigns layer information to grid cells.
Object.py | Implements object dynamics and trajectory prediction using nuScenes models (MTP, ResNet backbone). Propagates predicted object risks to the spatial grid.
Dectetion.py | (Detection module) Converts LiDAR data into grid occurrences and updates risk maps based on detected reflections and accumulated scan uncertainty.
Error.py | Calculates performance and error metrics between simulations, including occupancy uncertainty and severity differences across distance ranges.

--------------------------------------------------------------------------------
Citation

If you use this code, please cite our IEEE VTC 2025 paper:
C. Hogendoorn, R. Wosten, M. Zimmerman, and N. J. Myers,
“Misdetection Risk-Aware Adaptive LiDAR Sensing for Automotive Driving,”
IEEE Vehicular Technology Conference (VTC-Spring), 2025.
****
