# Asteroids - Balls

## Overview
This project features a dual-mode interactive canvas animation that showcases two distinct procedural simulations. Through a central navigation overlay, users can toggle between an autonomous ecosystem of spaceships and asteroids, or a 2D elastic particle collision engine.

## Core Mechanisms

### Mode 1: Swarm Ecosystem
The primary animation simulates a flocking system where automated spaceships interact with a field of drifting asteroids.
- **Flocking & Pathfinding**: Spaceships utilize steering behaviours to actively align with nearby peers and seek out asteroids, while avoiding the canvas boundaries.
- **Energy Simulation**: Each ship possesses an internal energy level that constantly depletes. Coming into proximity with an asteroid allows the ship to siphon energy, which visually affects the length of its trailing path and movement capability.

### Mode 2: Elastic Collisions
The secondary animation transforms the view into a bounded physics simulation.
- **Particle System**: It employs a custom-built 2D physics engine utilizing vector mathematics to track entity positions and velocities.
- **Kinematic Resolution**: The script accurately computes two-dimensional elastic collisions. Upon intersection, normal and relative velocities are calculated to apply correct impulses, ensuring the particles bounce off each other and the screen constraints seamlessly.
