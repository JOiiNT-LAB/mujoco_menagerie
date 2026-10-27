# Unitree H2 Description (MJCF)

> [!IMPORTANT]
> Requires MuJoCo 2.3.4 or later.

## Overview

MJCF description of the [Unitree](https://www.unitree.com/) H2 humanoid: 31 actuated joints (6 per leg,
3 waist, 2 head, 7 per arm). Joint order (qpos and ctrl):

```
legs (L, R): hip_pitch, hip_roll, hip_yaw, knee, ankle_roll, ankle_pitch
waist:       waist_yaw, waist_roll, waist_pitch
head:        head_pitch, head_yaw
arms (L, R): shoulder_pitch, shoulder_roll, shoulder_yaw, elbow, wrist_roll, wrist_pitch, wrist_yaw
```

At `elbow = 0` the forearm points forward (G1: straight arm).

## Sources

| Source | Used for |
|---|---|
| [unitree_mujoco `h2_mujoco.xml`](https://github.com/unitreerobotics/unitree_mujoco/blob/main/unitree_robots/h2/h2_mujoco.xml) | Kinematics, inertias, joint ranges, torque limits, armature, friction loss, meshes |
| [unitree_ros `H2.urdf`](https://github.com/unitreerobotics/unitree_ros/blob/master/robots/h2_description/H2.urdf) | Pelvis inertial (missing in `h2_mujoco.xml`) |
| [unitree_rl_mjlab `h2_constants.py`](https://github.com/unitreerobotics/unitree_rl_mjlab/blob/main/src/assets/robots/unitree_h2/h2_constants.py) | PD gains and home pose (`h2_mjx.xml`) |

## Files

- `h2.xml`, `scene.xml`: full model with mesh collisions. Joint position actuators, kp 1000 (not tuned).
- `h2_mjx.xml`, `scene_mjx.xml`: MJX version with capsule colliders, explicit contact pairs, `unitree_rl_mjlab`
  PD gains and `home` / `knees_bent` keyframes.

Changes from `h2_mujoco.xml`: torque motors replaced by position actuators in qpos order (the source lists the
head last), pelvis inertial added, IMU sites renamed `imu_in_pelvis` / `imu_in_torso`, foot sites added.

## Open points

- IMU sites are at the link origins as in `h2_mujoco.xml`; `H2.urdf` places them at
  pelvis `(-0.055, 0, -0.0589)` and torso `(-0.06382, -0.06538, 0.29588)`.
- Head gains have no official value (`unitree_rl_mjlab` welds the head); wrist gains are used.
- Torque limits differ in `unitree_rl_mjlab`: arms 54 Nm (here 130 / 60 Nm), wrist pitch/yaw 25 Nm (here 10 Nm).

## License

This model is released under a [BSD-3-Clause License](LICENSE).
