# Flocking Algorithm Implementation

A complete Unity implementation of Craig Reynolds' Boids Flocking Algorithm created for **Pirate Raiders**. This repository contains all C# scripts, prefabs, materials, and animation assets required to integrate dynamic flocking behaviour into any Unity project.

---

## 🌟 Overview

The Flocking System simulates realistic collective motion for autonomous agents (boids/birds/ships). Each unit independently computes its velocity based on local neighborhood interactions and environmental obstacles using weighted vector calculations.

### Core Flocking Behaviors:
1. **Cohesion**: Steers units toward the average position of nearby flockmates.
2. **Alignment**: Steers units to match the average heading/velocity of nearby flockmates.
3. **Avoidance (Separation)**: Steers units away from nearby flockmates to prevent overcrowding and collisions.
4. **Obstacle Avoidance**: Uses multi-directional 3D raycasting to detect surrounding colliders and steer clear.
5. **Waypoint Path Following**: Guides the flock along sequence points defined by a `Waypoint` system.

---

## 📐 Mathematical Formulation

At each frame step, the overall movement vector $\vec{V}_{move}$ for a flock unit is computed as a weighted sum of independent behavior vectors:

$$\vec{V}_{move} = w_{cohesion} \cdot \vec{V}_{cohesion} + w_{alignment} \cdot \vec{V}_{alignment} + w_{avoidance} \cdot \vec{V}_{avoidance} + w_{waypoint} \cdot \vec{V}_{waypoint} + w_{obstacle} \cdot \vec{V}_{obstacle}$$

- **Smooth Damp Filtering**:
  $$\vec{V}_{smooth} = \text{SmoothDamp}(\vec{V}_{forward}, \vec{V}_{move}, \text{ref } \vec{V}_{velocity}, t_{smooth})$$
- **Velocity Normalization & Speed Clamp**:
  $$\vec{V}_{final} = \text{Clamp}(\vec{V}_{smooth}, S_{min}, S_{max})$$

---

## 📁 Repository Structure

```
Flocking-Algorithm/
├── Scripts/
│   ├── flock.cs           # General Flock Manager & Spawner script
│   ├── FlockUnit.cs       # Boid Unit script (computes Cohesion, Alignment, Avoidance, Raycasting)
│   ├── Bird_Flock.cs      # Interactive Bird Flock Manager & Flight Controller
│   ├── Flock_Movement.cs  # Bird Boid unit movement & steering logic
│   └── Waypoint.cs        # Waypoint node array provider for path navigation
├── Prefabs/
│   └── Bird Variant.prefab # 3D Flock Unit prefab with Mesh, Colliders & Behaviors
├── Materials/
│   ├── Boid.mat           # Unity Standard Material for Boid bodies
│   ├── Insect Wing Color.mat # Material for Boid wing geometry
│   ├── Wing.controller    # Animator Controller managing wing flap states
│   └── Wing 1 Animation.anim # Keyframe Animation asset for wing movement
└── README.md              # Documentation
```

---

## 🛠️ Component Descriptions

### 1. `flock.cs` (Flock Manager)
- **Role**: Spawns `flockSize` number of `FlockUnit` prefabs randomly inside `spawnBounds`.
- **Configurable Parameters**:
  - `cohesionDistance`, `alignmentDistance`, `avoidanceDistance`, `obsticaleDistance`
  - `cohesionWeight`, `alignmentWeight`, `avoidanceWeight`, `obsticaleWeight`
  - `minSpeed`, `maxSpeed`

### 2. `FlockUnit.cs` (Autonomous Unit Logic)
- **Role**: Handles individual boid steering mechanics.
- **Key Methods**:
  - `FindNeighbors()`: Filters surrounding boids within distance thresholds.
  - `CalculateCohesionVector()` / `CalculateAlignmentVector()` / `CalculateAvoidanceVector()`: Implements Reynolds' rules constrained by Field-of-View (`FOVAngle`).
  - `CalculateObsticaleVector()` & `FindBestDirectionToAvoidObsticale()`: Casts rays along `directionsToCheckWhenAvoidingObsticales` to locate open flight corridors.
  - `followWaypoint()`: Calculates directional vector toward active waypoint target.

### 3. `Bird_Flock.cs` & `Flock_Movement.cs`
- **Role**: Specialized flight controller setup providing user pitch, yaw, and roll controls for player-guided or interactive bird flocks.

### 4. `Waypoint.cs`
- **Role**: Automatically collects child transform nodes into a static `points` array used by all flock units for path looping.

---

## 🚀 How to Use in Unity

1. **Clone or Copy** the `Scripts/`, `Prefabs/`, and `Materials/` folders into your Unity project's `Assets` directory.
2. **Setup Waypoints**:
   - Create an empty GameObject named `Waypoints` and attach `Waypoint.cs`.
   - Add child GameObjects as position nodes along the desired path.
3. **Spawn the Flock**:
   - Create an empty GameObject and attach `flock.cs` or `Bird_Flock.cs`.
   - Assign `Bird Variant.prefab` (or any prefab with `FlockUnit.cs`/`Flock_Movement.cs`) to `flockUnitPrefab`.
   - Adjust flock weights, distances, and speed ranges in the Unity Inspector.
4. **Hit Play**: Watch the flock navigate dynamically with obstacle avoidance and cohesion!

---

## 📄 License
This project is licensed under the MIT License - see the repository source for details.
