# Humanoid Fighting in Simulation

An open source project for training humanoid robots in competitive, contact-rich simulation.

The project uses **Unitree G1**, **Isaac Lab**, **PyTorch**, and **RSL-RL** to build a modular system for locomotion, recovery, vision, task planning, and multi-agent reinforcement learning.

## Goal

Two humanoid robots compete in a simple arena. The initial task is sumo-style: make the opponent fall or leave the arena while staying balanced.

```text
Vision / State
     |
     v
High Level Policy
     |
     v
Skill Selection
     |
     v
Locomotion / Recovery / Contact
     |
     v
Unitree G1
```

## Roadmap

* [ ] Isaac Lab + G1 setup
* [ ] Locomotion
* [ ] Push and fall recovery
* [ ] Motion imitation
* [ ] Two-robot environment
* [ ] Opponent tracking
* [ ] Contact skills
* [ ] Hierarchical task planning
* [ ] Vision-based control
* [ ] Pretraining and fine-tuning
* [ ] Self-play
* [ ] Evaluation and ablations

## Planned Skills

```text
STAND
APPROACH
RETREAT
CIRCLE
PUSH
EVADE
BRACE
RECOVER
```

## Evaluation

* Win rate
* Fall rate
* Recovery rate
* Ring-out rate
* Energy usage
* Vision accuracy
* Elo rating

## Tech Stack

* Isaac Sim / Isaac Lab
* Unitree G1
* PyTorch
* RSL-RL
* PPO
* CUDA

## Status

Early development. Contributions, ideas, and pull requests are welcome.
