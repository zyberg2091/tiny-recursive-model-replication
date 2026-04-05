# Tiny Recursive Model (TRM) – Paper Implementation and Experimentations

## Overview

Implementation of Tiny Recursive Model (TRM) with halting (q_halt) and deep supervision.
Experiments focus on understanding training dynamics of halting, masking, and recursive updates.


## Observations

### Halting Behavior

* For small c (0.01), the model halts after a single step (n_taken = 1)
* Despite multiple supervision steps, effective reasoning depth collapses to 1
* This suggests that halting dominates training dynamics

### Effect of Halting Coefficient (c)

* c controls the relative strength of halting loss
* Small c leads to early halting and reduced recursive utilization
* This indicates a trade-off between prediction accuracy and reasoning depth

### Per-Sample vs Batch Optimization

* Halting operates per sample, leading to variable recursion depth within a batch
* However, optimization is performed at batch level
* Without masking, halted samples continue to influence gradients
* Masking aligns gradient updates with active computation paths

### Masking Behavior

* Training without masking is less stable
* Masking improves stability, although initial updates are slightly noisy
* This suggests masking is important for correct optimization dynamics

### Loss Behavior

* Mean loss across steps reduces the contribution of later recursive updates
* This weakens deeper optimization and discourages multi-step reasoning

### Recursive Structure

* Inner recursion updates latent state (z) multiple times before updating output (y)
* z refines internal representations, while y produces final predictions
* Outer recursion performs repeated refinement with detached states

### Latent State Dynamics

* Latent layers are not directly supervised
* They contribute to stabilizing internal representations before prediction

### Task Complexity

* The model reaches near 100% accuracy quickly
* This suggests the task is too simple to fully utilize recursive reasoning

### Key Takeaways

* Halting significantly affects training dynamics, not just inference
* Proper balancing of halting and prediction is critical
* Masking is required for correct per-sample optimization
* Recursive depth remains underutilized on simple tasks


### Inspired by [Less is More: Recursive Reasoning with Tiny Networks](https://arxiv.org/pdf/2510.04871)
