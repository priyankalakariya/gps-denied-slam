# GPS-Denied Drone SLAM for Underground Mine Mapping

Autonomous LiDAR-inertial SLAM system for 3D mapping and mineral detection in GPS-denied underground mine tunnels. Validated in ROS 2 Humble + Gazebo Classic using DARPA SubT Jenolan cave meshes.

---

## Overview

Underground mines offer no GPS signal, sparse visual features, and irregular geometry — conditions that break most autonomous navigation systems. This project builds a full drone pipeline that:

- Maps unknown multi-branch tunnel networks from scratch using LIO-SAM
- Maintains localization continuity via RTAB-Map fallback with loop closure
- Drives autonomous coverage using frontier-based exploration (Nav2 + OctoMap)
- Classifies 5 mineral types in real time using a georeferenced NIR spectral classifier

---

## Results

| Metric | Result |
|---|---|
| Waypoint Coverage | 100% (114/114 frontier goals) |
| RTAB-Map Nodes | 10,578 (post loop closure) |
| Point Cloud Export | Full PLY, 0.01m voxel filter |
| Tunnel Bounding Box | 50 × 30 × 5 m |
| Mineral Classification Confidence | 81% – 93% (5 classes) |

## System Architecture

Velodyne VLP-16 + IMU
        │
        ▼
   LIO-SAM (primary SLAM)
        │
        ├──► RTAB-Map (fallback, loop closure)
        │
        ▼
   OctoMap (3D occupancy)
        │
        ├──► Nav2 Frontier Explorer
        │
        └──► NIR Spectral Classifier
                    │
                    ▼
          Georeferenced Mineral Map

## Simulation Environment

Built in Gazebo Classic 11 using DARPA SubT Challenge Jenolan cave meshes (sections 04–08).

**Tunnel Features:**
- 4 straight corridors (8–18 m)
- 2 T-junction intersections
- 1 dead-end passage
- 1 reduced-ceiling zone (1.4 m) for nav stress testing

**Mineral Zones:**

| Mineral | X (m) | Y (m) | Z (m) |
|---|---|---|---|
| Quartz | 4.55 | -1.81 | 1.0 |
| Hematite | -4.05 | 1.43 | 1.0 |
| Malachite | -10.23 | -0.12 | 1.0 |
| Chalcopyrite | -15.81 | -1.85 | 1.0 |
| Limestone | -15.80 | 2.24 | 1.0 |

---

## Tech Stack

| Component | Tool |
|---|---|
| Framework | ROS 2 Humble |
| Simulation | Gazebo Classic 11 |
| Primary SLAM | LIO-SAM |
| Fallback SLAM | RTAB-Map |
| 3D Mapping | OctoMap |
| Navigation | Nav2 (NavFn + DWB) |
| Point Cloud | PCL |
| Classification | scikit-learn (k-NN, SVM) |
| Evaluation | evo trajectory toolbox |
| Language | Python 3.10 |

---

## Project Structure

drone_slam_ws/
├── src/
│   ├── drone_slam_bringup/        # Launch files, top-level configs
│   ├── gazebo_cave_world/         # Cave mesh assets, world SDF, mineral zones
│   ├── sjtu_drone/                # Drone URDF, Gazebo plugins, velocity controller
│   └── LIO-SAM/                   # LIO-SAM fork with tunnel-tuned params
├── bags/                          # ROS 2 bag recordings for offline eval
└── results/                       # ATE/RPE plots, PLY exports, confusion matrices

## Known Limitations

- Stereo camera plugin unsupported under VMware (OpenGL GLSL 4.6 constraint)
- TF sync warnings accumulate on long runs due to Gazebo sim time vs ROS wall clock
- Real-time factor ~0.88 in dense cave sections
- NIR classifier not validated against physical hardware

---

## References

- T. Shan et al., "LIO-SAM," IEEE/RSJ IROS 2020
- M. Labbé and F. Michaud, "RTAB-Map," J. Field Robot. 2019
- A. Hornung et al., "OctoMap," Autonomous Robots 2013
- R. N. Clark et al., USGS Spectral Library Version 7, 2017
- DARPA SubT Challenge — Jenolan cave meshes (CC BY-SA 4.0)
```
