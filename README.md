# GPS-Denied Drone SLAM

Autonomous SLAM system for underground mine mapping without GPS. 

## About

This project builds a drone system that autonomously maps underground mine tunnels using LiDAR and IMU — no GPS available. Covers environment simulation, sensor fusion, SLAM, and autonomous exploration.

## Current Progress

**Done:**
- Custom Gazebo mine tunnel with multi-branch layout (4 corridors, 2 T-junctions, 1 dead-end, 1.4m reduced-ceiling zone)
- 5 mineral classification zones with distinct wall materials (Quartz, Hematite, Malachite, Chalcopyrite, Limestone)
- GPS-denied simulation environment (underground, no satellite signals)

**Next:**
- Drone URDF with Velodyne VLP-16 LiDAR + IMU
- LIO-SAM for LiDAR-inertial odometry
- Nav2 frontier-based exploration
- OctoMap 3D occupancy mapping
- Trajectory evaluation against Gazebo ground truth

## Tech Stack

ROS 2 Humble, Gazebo 11, LIO-SAM, Nav2, OctoMap, Python 3.10

## Running It
```bash
gzserver --verbose worlds/mine_tunnel_world.sdf &
sleep 5
gzclient
```

## Project Structure
