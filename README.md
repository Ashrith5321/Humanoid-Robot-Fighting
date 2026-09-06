# Humanoid Fighting in Simulation

An open source project for training and evaluating humanoid robots in competitive, contact-rich simulation.

The goal is to build a modular framework for humanoid locomotion, recovery, interaction, vision, task planning, and multi-agent reinforcement learning using simulated Unitree G1 robots.

The project is being built around **Isaac Lab**, **PyTorch**, and **RSL-RL**, with an emphasis on reusable skills, hierarchical control, and self-play.

## Overview

The environment contains two humanoid robots competing inside an arena.

The initial task is closer to humanoid sumo than unrestricted fighting. A robot wins by making the opponent fall or leave the arena while maintaining its own balance.

The system is designed to support progressively more capable policies, starting from basic locomotion and recovery and eventually moving toward vision-based multi-agent control.

## Planned Architecture

```text
RGB / Depth
    |
    v
Opponent Perception
    |
    v
High Level Policy
    |
    +--> Approach
    +--> Retreat
    +--> Circle
    +--> Evade
    +--> Push
    +--> Recover
    |
    v
Low Level Humanoid Policy
    |
    v
Joint Targets / Whole Body Control
    |
    v
Unitree G1
```

The project uses a hierarchical design rather than starting with a fully end-to-end RGB-to-joint policy.

Low level controllers handle locomotion, balance, and recovery, while a higher level policy decides which behavior to execute based on the current state of the match.

## Roadmap

### 1. Simulation Setup

* Set up Isaac Lab
* Spawn Unitree G1
* Configure joint control
* Read robot state and contact forces
* Run environments in parallel on GPU
* Add arena and reset logic

### 2. Locomotion

Train reusable movement policies for:

* Standing
* Forward walking
* Backward walking
* Side stepping
* Turning
* Stopping
* Direction changes

### 3. Recovery

Train policies for balance and fall recovery under disturbances such as:

* External pushes
* Stumbling
* Large body tilt
* Kneeling
* Front falls
* Back falls
* Side falls

### 4. Motion Imitation

Support reference motion tracking and imitation for behaviors such as:

* Walking
* Running
* Sidestepping
* Crouching
* Turning
* Pushing
* Recovery motions

These policies can also be used as pretrained initialization for downstream tasks.

### 5. Multi-Robot Environment

Add a second humanoid and support relative observations such as:

* Opponent position
* Opponent orientation
* Opponent velocity
* Arena position
* Relative distance
* Robot stability

Initial interaction tasks include:

* Facing the opponent
* Approaching
* Maintaining distance
* Circling
* Retreating

### 6. Contact Skills

Introduce controlled physical interaction between robots.

Initial skills include:

* Pushing
* Bracing
* Avoiding contact
* Maintaining balance during contact
* Recovering after impact

### 7. Skill Library

The project will expose reusable high level actions such as:

```text
STAND
APPROACH
RETREAT
CIRCLE_LEFT
CIRCLE_RIGHT
PUSH
EVADE
BRACE
RECOVER
```

These skills can be selected by either a hand-written controller or a learned policy.

## Task Planning

The first high level controller can use a simple state machine:

```python
if fallen:
    recover()
elif near_boundary:
    move_to_center()
elif opponent_far:
    approach()
elif opponent_close:
    push()
else:
    circle()
```

This provides a baseline for comparison against learned high level policies.

Planned comparisons include:

* Finite state machine
* Hierarchical RL
* End-to-end RL

## Vision

Early versions of the environment use privileged simulator state for opponent information.

Later versions will support onboard camera observations.

```text
RGB
 |
 v
Vision Encoder
 |
 v
Opponent Representation
 |
 v
High Level Policy
```

Planned vision experiments include:

* Opponent detection
* Relative pose estimation
* Motion estimation
* Vision-based policy control
* State-to-vision policy distillation

## Pretraining and Fine-Tuning

The project is designed so individual components can be trained independently and reused.

Possible pretrained components include:

```text
Locomotion
+
Recovery
+
Motion Imitation
+
Vision
+
Interaction Skills
```

These components can then be fine-tuned for full competitive behavior.

## Self-Play

The later stages of the project will support multi-agent self-play.

Instead of always training against the latest policy, the environment can sample opponents from a pool of previous checkpoints.

```text
Current Policy
      |
      v
Opponent Pool
      |
      +--> Policy 20
      +--> Policy 50
      +--> Policy 100
      +--> Policy 150
```

Potential extensions include:

* Historical opponent sampling
* Elo ratings
* League training
* Population-based training
* Strategy diversity analysis

## Evaluation

Planned metrics include:

* Win rate
* Fall rate
* Ring-out rate
* Recovery success rate
* Episode length
* Energy usage
* Velocity tracking error
* Opponent tracking accuracy
* Vision localization error
* Elo rating

The project will also support ablation experiments such as:

* State vs vision
* Recovery vs no recovery
* Pretraining vs training from scratch
* Flat vs hierarchical control
* Motion imitation vs no imitation
* Self-play vs fixed opponent
* Curriculum vs no curriculum
* Domain randomization vs no randomization

## Tech Stack

* NVIDIA Isaac Sim
* Isaac Lab
* Unitree G1
* PyTorch
* RSL-RL
* PPO
* CUDA
* TensorBoard / Weights & Biases

## Hardware

Development and training are currently targeted at modern NVIDIA GPUs, with primary development on an RTX 5090.

## Project Status

Early development.

Current priorities:

* Isaac Lab environment setup
* G1 locomotion baseline
* Recovery training
* Multi-robot arena
* Contact interaction environment

## Contributing

Contributions are welcome.

Possible areas include:

* Humanoid locomotion
* Recovery policies
* Motion imitation
* Reward design
* Contact modeling
* Vision
* Multi-agent RL
* Self-play
* Evaluation tools
* Environment design
* Documentation

If you are interested in contributing, feel free to open an issue or submit a pull request.

## Goal

The long-term goal is to build a flexible open source benchmark and training environment for competitive humanoid interaction.

The final system should support humanoids that can:

* Perceive an opponent
* Move and reposition dynamically
* Maintain balance under contact
* Recover from disturbances and falls
* Select different skills based on the situation
* Adapt to different opponents
* Improve through self-play

The project is still early, so the architecture and scope will evolve as the system develops.
