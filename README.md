# Hi, I'm Lynn 👋

*Robotics Reinforcement Learning · Sim-to-Real · LLM Infrastructure*

**PhD student — open to research / robotics / ML internships. (Not seeking full-time.)**

[![Email](https://img.shields.io/badge/Email-lynnhe02@gmail.com-EA4335?logo=gmail&logoColor=white)](mailto:lynnhe02@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Lin%20He-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/lin-he-566260335/)
[![vLLM](https://img.shields.io/badge/vLLM-2%20PRs%20Merged-D32F2F?logo=github&logoColor=white)](https://github.com/vllm-project/vllm/pulls?q=is%3Apr+author%3ALynn-hh)
[![Isaac Lab](https://img.shields.io/badge/Isaac%20Lab-RL-76B900?logo=nvidia&logoColor=white)](https://isaac-sim.github.io/IsaacLab/)
[![ROS 2](https://img.shields.io/badge/ROS%202-22314E?logo=ros&logoColor=white)](https://docs.ros.org/)
[![Location](https://img.shields.io/badge/Texas-US-555555?logo=googlemaps&logoColor=white)](#)

---

## About Me

PhD student working at the intersection of **robot learning and large-scale systems** — I train RL policies in
simulation and push them toward real hardware, and I care about the infrastructure that makes both fast and correct.

I came to AI from an unusual direction: a **B.Arch / architecture background** that taught me to reason about
structure, geometry, and spatial constraints — which now feeds directly into how I think about robot learning,
3D simulation, and AI for design.

- 🤖 **Robotics RL in simulation** — building and training policies in **NVIDIA Isaac Sim / Isaac Lab**
  (locomotion / manipulation / sim-to-real) with GPU-parallel environments and PPO/SAC-style training.
- 🦾 **ROS 2** — bridging learned policies to real robot stacks (nodes, controllers, perception → action pipelines).
- ⚙️ **LLM infrastructure** — contributor to **[vLLM](https://github.com/vllm-project/vllm)**, the core LLM
  inference engine: **2 performance PRs merged**, 2 more in review (details below).
- 🔬 Research interests: sim-to-real transfer, reinforcement learning, robot manipulation/locomotion,
  GPU-accelerated simulation, and efficient model serving.

📫 **lynnhe02@gmail.com** · 📍 Texas

---

## Open Source — LLM Infrastructure Contributions

### vllm-project/vllm (~84k★) — the core LLM inference engine

- **PR [#46542](https://github.com/vllm-project/vllm/pull/46542) — `[Perf][LoRA]` (merged):** Replaced a per-token
  `list.index()` lookup in `convert_mapping` — an O(num_tokens × num_loras) hot path the code had flagged with a
  TODO — by building a reverse `{lora_id: index}` dict once for O(1) lookups, cutting mapping construction to
  O(num_tokens). **2.5×–6.5× faster** in microbenchmarks (e.g. 64 LoRAs / 1024 tokens: 275µs → 42µs), with
  identical output verified against randomized + existing LoRA tests.
- **PR [#46543](https://github.com/vllm-project/vllm/pull/46543) — `[Perf][Multimodal]` (merged):** Removed a
  wasteful O(num_frames) timestamp-list allocation in GLM-4V / GLM video frame sampling, computing each timestamp
  inline as `frame_index * duration_per_frame`. Byte-for-byte identical behavior with lower memory on long videos.
- **PRs [#46539](https://github.com/vllm-project/vllm/pull/46539) · [#46540](https://github.com/vllm-project/vllm/pull/46540) — `[Perf][V1]` (in review):**
  Sampler optimizations — applying min-p in log-space and reusing log-probs to avoid redundant softmax passes.

---

## Tech Stack

**Robotics & Simulation**

![Isaac Sim](https://img.shields.io/badge/Isaac%20Sim-76B900?logo=nvidia&logoColor=white)
![Isaac Lab](https://img.shields.io/badge/Isaac%20Lab-76B900?logo=nvidia&logoColor=white)
![ROS 2](https://img.shields.io/badge/ROS%202-22314E?logo=ros&logoColor=white)
![MuJoCo](https://img.shields.io/badge/MuJoCo-000000?logoColor=white)
![Gymnasium](https://img.shields.io/badge/Gymnasium-0081A5?logoColor=white)

**AI / ML**

![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?logo=pytorch&logoColor=white)
![RL (PPO/SAC)](https://img.shields.io/badge/RL-PPO%20%2F%20SAC-5C2D91)
![vLLM](https://img.shields.io/badge/vLLM-D32F2F)
![NumPy](https://img.shields.io/badge/NumPy-013243?logo=numpy&logoColor=white)
![Weights & Biases](https://img.shields.io/badge/W%26B-FFBE00?logo=weightsandbiases&logoColor=black)

**Languages & Systems**

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?logo=cplusplus&logoColor=white)
![C#](https://img.shields.io/badge/C%23-239120?logo=csharp&logoColor=white)
![CUDA](https://img.shields.io/badge/CUDA-76B900?logo=nvidia&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?logo=git&logoColor=white)

---

## Featured Projects

| Project | Description | Stack |
|---|---|---|
| **Franka Arm RL (Isaac Lab)** | Reinforcement-learning training for a **Franka Emika Panda** manipulator in **NVIDIA Isaac Lab** — GPU-parallel environments for arm control (reaching / manipulation), with PPO-style policy training in simulation. | Isaac Lab, Isaac Sim, PyTorch, RL |
| **[Archiagents](https://archiagents.com/)** | End-to-end AI agent for architectural design (collaborative project). Ingests project briefs + CAD/DWG/IFC/Revit files, runs requirement dialogue, generates design schemes and photorealistic renders, and outputs IFC4 BIM models with an embedded Autodesk APS viewer. **My role:** brought the architecture-domain expertise (B.Arch background) — shaping the design-requirement logic, the agent's reasoning over building programs, and the IFC4 / BIM modeling that turns AI output into valid design deliverables. | Vercel AI SDK, shadcn/ui, Autodesk APS, IFC4 |
| **[Revit-Civil-AI-Estimator](https://github.com/Lynn-hh/Revit-Civil-AI-Estimator)** | Revit 2025 add-in that uses OpenAI to automate quantity takeoff and cost estimation for civil-engineering workflows. | C#, OpenAI API, Revit |

---

## GitHub Stats

![Stats](https://github-readme-stats.vercel.app/api?username=Lynn-hh&show_icons=true&theme=default&hide_border=true)
![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=Lynn-hh&layout=compact&hide_border=true)
