

---

<h1 align="center">TAD-IDS</h1>

<p align="center">
<strong>Temporally Aware Deep Intrusion Detection System</strong><br>
<em>Structured feature modeling for robustness under temporal shift</em>
</p>

---
<p align="center">
  <img src="https://img.shields.io/badge/Status-Research--Prototype-blue?style=flat-square" alt="Status">
  <img src="https://img.shields.io/badge/Framework-PyTorch-ee4c2c?style=flat-square&logo=pytorch&logoColor=white" alt="Framework">
  <img src="https://img.shields.io/badge/Architecture-Multi--Head--Attention-6E3FF3?style=flat-square" alt="Architecture">
  <img src="https://img.shields.io/badge/Python-3.8+-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=flat-square" alt="License">
</p>

# TAD-IDS
### Addressing Temporal Fragility in Network Intrusion Detection via Structured Feature Modeling

Traditional Network Intrusion Detection Systems (NIDS) frequently encounter **temporal fragility**: a performance degradation phenomenon occurring as the temporal gap between training data and live traffic increases. **TAD-IDS** provides a structured feature-modeling framework designed to identify and prioritize time-invariant network patterns to mitigate this decay.

---

## Technical Overview

Standard NIDS architectures typically process network features as a monolithic vector, which fails to account for semantic relationships between packet headers, payloads, and timing metadata. Consequently, models often overfit to transient noise specific to the training window.

**TAD-IDS** (Temporally Aware Deep IDS) addresses this via:
1.  **Semantic Decomposition**: Partitioning raw features into distinct functional groups (Packet, Timing, Flag, Flow, Connection).
2.  **Residual Encoding**: Processing groups through dedicated bottlenecks to extract robust embeddings.
3.  **Attention-Based Synthesis**: Utilizing a Multi-Head Attention mechanism to dynamically weight feature groups based on cross-temporal stability.



## Architecture

The system implements a modular **Encoder-Aggregator** design:

* **Group Encoders**: Dedicated neural blocks utilizing residual connections to project telemetry into a unified 32-dimensional embedding space.
* **Temporal Aggregator**: A Multi-Head Attention (MHA) layer that computes inter-dependencies between feature domains, effectively suppressing brittle feature sets that exhibit high variance across temporal shifts.
* **Calibration Module**: A post-processing framework that optimizes decision boundaries based on Reality Gap metrics rather than static validation performance.

## Performance Analysis

TAD-IDS significantly reduces the **Reality Gap**—the delta between benchmark accuracy and future-test deployment performance—compared to traditional deep learning baselines.

| Model Architecture | Benchmark F1 | Deployment F1 | Reality Gap |
| :--- | :---: | :---: | :---: |
| Standard MLP | 0.98 | 0.81 | -17% |
| Grouped (No Attention) | 0.97 | 0.84 | -13% |
| **TAD-IDS** | **0.97** | **0.89** | **-8%** |

## Implementation

### Prerequisites
* Python 3.8+
* PyTorch 2.0+
* NumPy (Memory-mapping support for large-scale datasets)

### Usage
```python
import torch
from models.tad_ids import TAD_IDS

# Initialize architecture with defined semantic group dimensions
model = TAD_IDS(group_dims=[12, 10, 8, 15, 20, 13], embed_dim=32)

# Forward pass: Structured tensors representing localized network domains
logits = model(packet_data, timing_data, flag_data, flow_data, conn_data, res_data)

```
---

## License

This project is released under the **[MIT License](LICENSE)**. 

*Engineered for high-performance, temporally-robust network security research.*
