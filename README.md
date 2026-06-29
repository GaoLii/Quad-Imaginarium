# Quad-Imaginarium

**Quad-Imaginarium** is a large-scale, open-source quadruped robot motion dataset constructed entirely through generative video priors. It contains **7,488 language-annotated quadruped motions** totaling **18.5 hours** at 24 fps, spanning acrobatic and performative behaviors on a Unitree Go2 quadruped robot.

> **Paper:** *Unleashing Infinite Motion: Scaling Expressive Quadrupedal Motion via Generative Video Priors*
>
> Youzhi Liu, Li Gao\*, Yifei Qian\*, Liu Liu, Yang Cai, Ziqiao Li
>
> (\* Corresponding authors)

## Overview

Unlike prior quadruped motion datasets that derive content from real animal motion capture, teleoperation, or artist authoring, **every clip in Quad-Imaginarium is synthesized end-to-end** -- human effort enters only at the annotation stage.

Our pipeline, **Uni-Mo**, removes the animal from the loop by reframing data scarcity as a generation problem:

1. An LLM proposes motion prompts
2. A fine-tuned video diffusion model synthesizes the corresponding robot behaviors
3. Generated videos are lifted into 3D reference trajectories
4. Tracking policies are trained via reinforcement learning and deployed on a real Unitree Go2

We introduce an **Identity Consistency Loss** that enforces appearance coherence across frames, making naively-drifting video generations reliably extractable into 3D motion.

## Dataset Statistics

| Property | Value |
|---|---|
| Total motions | 7,488 |
| Total duration | 18.5 hours |
| Frame rate | 24 fps |
| Duration range | 5 -- 15 s per clip |
| Mean duration | 8.9 s |
| State representation | 19-D per frame (3-D root position + 4-D quaternion + 12 joint angles) |
| Robot platform | Unitree Go2 (12 DOF) |
| Language annotations | Dual: fine-grained description + command instruction |

## Data Format

Each motion clip is stored as a `.pkl` file in the Go2 configuration space with the following fields:

```python
{
    "motion_name": {
        "root_trans_offset": np.array,  # (T, 3) root translation
        "root_rot": np.array,           # (T, 4) root rotation quaternion (xyzw)
        "dof": np.array,                # (T, 12) joint angles
        "pose_aa": np.array,            # (T, 13, 3) axis-angle per body
        "fps": int                      # frame rate (24)
    }
}
```

## Deployment Validation

We validated 392 randomly sampled motions on a real Unitree Go2:

- **96.7%** deployment success rate on real hardware
- **97.6%** success rate across the full dataset in simulation

## Coming Soon

The full dataset (7,488 motion clips with language annotations) will be released in July 2026.

## Citation

If you find this dataset useful, please cite:

```bibtex
@article{liu2026unleashing,
  title={Unleashing Infinite Motion: Scaling Expressive Quadrupedal Motion via Generative Video Priors},
  author={Liu, Youzhi and Gao, Li and Qian, Yifei and Liu, Liu and Cai, Yang and Li, Ziqiao},
  journal={arXiv preprint arXiv:2606.28237},
  year={2026},
  url={https://arxiv.org/abs/2606.28237}
}
```

