# Priority Evidence

## Purpose

This document serves as a formal record establishing the priority of the ideas presented in the **SAMMS** (Stable and Available MOE Model Services) project, published on GitHub on **March 26, 2025**, predating the submission of the paper "Expert-as-a-Service: Towards Efficient, Scalable, and Robust Large-scale MoE Serving" (arXiv:2509.17863, submitted September 22, 2025).

---

## Timeline

| Event | Date | Evidence |
|-------|------|----------|
| SAMMS first commit (initial idea published) | 2025-03-26 21:56:43 +0800 | Git SHA: `d1c57098e5278e04eb34353d3f65d8ef83ea3f59` |
| SAMMS citation added | 2025-03-26 22:06:39 +0800 | Git SHA: `f849b9c11bef914533abf9d812cbe7cf0417be9f` |
| EaaS paper submitted to arXiv | 2025-09-22 | arXiv:2509.17863v1 |

**Time difference: SAMMS precedes EaaS by approximately 6 months.**

---

## Core Idea Comparison

### 1. Expert Disaggregation as Independent Services

**SAMMS (March 2025):**
> "We can consider all Experts as many independent models and deploy these models separately. They can accept corresponding token requests and return the results to the requester after computation. In the Experts service, there is no communication between the Experts."

**EaaS (September 2025):**
> "The core principle of EaaS is to re-architect the system around the concept of disaggregating experts into independent services. We decouple the MoE layers from the rest of the model, treating the pool of experts as dynamically accessible and independent services."

---

### 2. Single-GPU Granularity and Fault Isolation

**SAMMS (March 2025):**
> "For DP instances, including P-type and D-type DP instances, once decoupled from the Experts, they will entirely become single-GPU data parallel and can support services independently. Their quantity can be flexibly adjusted based on traffic, with the adjustment granularity being a single GPU."
>
> "If any GPU encounters an issue, we only need to restart (or repair) the faulty GPU, which will not affect other GPUs."

**EaaS (September 2025):**
> "Fine-grained Elasticity: This service-oriented approach eliminates the rigid structure of large serving units. EaaS allows expert capacity to be scaled almost linearly and independently from the attention computation components... potentially one GPU at a time."
>
> "Improved Robustness: By replacing collective communication with peer-to-peer (P2P) interactions between attention clients and expert servers, EaaS removes the dependency on fragile, static communication groups."

---

### 3. P2P Asynchronous Communication Replacing All2All

**SAMMS (March 2025):**
> "In this scheme, All2All synchronous communication will transform into multiple P2P asynchronous communications."

**EaaS (September 2025):**
> "We design and implement a high-performance, asymmetric, asynchronous CPU-free P2P communication library based on IBGDA, tailored for dynamic client-server interactions in GPU clusters, overcoming limitations of existing libraries for this use case."

---

### 4. Load Balancing Without Expert Replication

**SAMMS (March 2025):**
> "After the separation of Experts, the load balancing issue can be completely resolved. We do not need to add redundant experts to alleviate the load on hotspot experts... As long as the probability of global DP instances running at each layer is the same at any given moment, complete load balancing can be achieved."

**EaaS (September 2025):**
> "Dynamic Load Balancing: The independence of each expert server enables real-time load balancing. If specific experts are frequently requested by more tokens, they can be duplicated to balance the computation."

---

### 5. Flexible P/D Separation at GPU Granularity

**SAMMS (March 2025):**
> "PD separation is no longer comprised of 4 Nodes (32xGPUs) for P instances and 18 Nodes (144xGPUs) for D instances, but rather consists of single-card instances. This greatly enhances flexibility, scalability, and reliability."

**EaaS (September 2025):**
> "Serving a model like DeepSeek-V3 can begin with a practical base unit (e.g., 16x 80GB GPUs) and scale incrementally, potentially one GPU at a time, to precisely match demand."

---

### 6. Connection Architecture Challenge (P2P vs. Intermediate Routing)

**SAMMS (March 2025):**
> "How to achieve efficient P2P connections between DP instances and Experts instances, and how to avoid having to re-establish connections for all interconnected Experts instances or DP instances after a disconnection... If connections are established using direct RDMA, efficient communication can reduce latency. However, if a DP instance or an Experts instance fails, it will affect the connected instances."

**EaaS (September 2025):**
> Addresses the same challenge by implementing IBGDA (InfiniBand GPUDirect Async) with CPU-free P2P communication, enabling group-free network operation and fault isolation.

---

## Git Verification Commands

Anyone can independently verify the timeline:

```bash
# Clone the repository
git clone https://github.com/yangpc615/SAMMS.git
cd SAMMS

# View complete commit history with timestamps
git log --pretty=format:"%H | %ai | %an | %s" --all

# Verify initial commit date
git log --reverse --format="%H %ai %s" | head -1

# Show initial commit content (full README with ideas)
git show d1c57098e5278e04eb34353d3f65d8ef83ea3f59
```

**Output:**
```
d1c57098e5278e04eb34353d3f65d8ef83ea3f59 | 2025-03-26 21:56:43 +0800 | apcyang | start
f849b9c11bef914533abf9d812cbe7cf0417be9f | 2025-03-26 22:06:39 +0800 | apcyang | add Citation
```

---

## Additional Archival Evidence

- **Wayback Machine Archive**: https://web.archive.org/web/2025*/https://github.com/yangpc615/SAMMS
- **GitHub API (creation date)**: `GET https://api.github.com/repos/yangpc615/SAMMS` -> `created_at` field
- **Image file metadata**: The architecture diagrams in `images/` directory contain creation timestamps (2025-03-26) embedded in filenames (e.g., `image-20250326201909048.png`)

---

## Conclusion

The SAMMS project publicly proposed the core architectural idea of disaggregating MoE experts into independent, single-GPU services with P2P asynchronous communication **6 months prior** to the EaaS paper. The overlap covers the fundamental system design philosophy, not merely superficial similarities.

---

## Contact

- Author: PC.Yang
- Repository: https://github.com/yangpc615/SAMMS
- Evidence archived: 2025-05-14
