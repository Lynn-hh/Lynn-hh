# Hi, I'm Lynn 👋

*Robotic Reinforcement Learning · AI Agents · LLM Infrastructure · Sim-to-Real*

**PhD Student — Open to Research / Robotics / ML Internships.**

[![Email](https://img.shields.io/badge/Email-lynnhe02@gmail.com-EA4335?logo=gmail&logoColor=white)](mailto:lynnhe02@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Lin%20He-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/lin-he-566260335/)
[![vLLM](https://img.shields.io/badge/vLLM-2%20PRs%20Merged-D32F2F?logo=github&logoColor=white)](https://github.com/vllm-project/vllm/pulls?q=is%3Apr+author%3ALynn-hh)
[![Isaac Lab](https://img.shields.io/badge/Isaac%20Lab-Contributor-76B900?logo=nvidia&logoColor=white)](https://github.com/isaac-sim/IsaacLab/pulls?q=is%3Apr+author%3ALynn-hh)
[![ROS 2](https://img.shields.io/badge/ROS%202-22314E?logo=ros&logoColor=white)](https://docs.ros.org/)
[![Location](https://img.shields.io/badge/Texas-US-555555?logo=googlemaps&logoColor=white)](#)

---

## About Me

PhD student working at the intersection of **Robot Learning and Large-scale Systems** — I train RL policies in
simulation and push them toward real hardware, and I build the AI agents and infrastructure that make both fast
and correct.

I came to AI from an unusual direction: a **B.Arch / architecture background** that taught me to reason about
complex systems, geometry, spatial relationships, and the tradeoffs between elegant ideas and real-world
constraints. That convinced me the most important problems in the physical world will be solved not by better
static tools, but by intelligent systems that can learn, adapt, and act — which drew me to robot learning,
simulation, and AI infrastructure. I'm most energized by unfamiliar, technically demanding problems and by turning
ideas across disciplines into scalable systems that work in practice.

- 🤖 **Robotics RL in Simulation** — training policies in **NVIDIA Isaac Sim / Isaac Lab**
  (locomotion / manipulation / sim-to-real) with GPU-parallel environments and PPO/SAC-style training.
- 🧠 **AI Agents** — VLM-driven agents that perceive, reason over policy/knowledge, and act in a closed
  sense→think→act→report loop (see **SafetyCommander** below).
- ⚙️ **LLM Infrastructure** — contributor to **[vLLM](https://github.com/vllm-project/vllm)**, the core LLM
  inference engine: **2 performance PRs merged**.
- 🦾 **ROS 2** — bridging learned policies to real robot stacks (nodes, controllers, perception → action pipelines).
- 🔬 **Research interests**: Reinforcement Learning, AI Agents, Robot Manipulation/Locomotion,
  GPU-accelerated simulation and sim-to-real transfer.

📫 **lynnhe02@gmail.com** · 📍 Texas

---

## Featured Projects

| Project | Description | Stack |
|---|---|---|
| **[SafetyCommander](https://github.com/Lynn-hh/safety-commander-agent)** — Autonomous Factory Safety Officer | A VLM agent that **owns a safety officer's shift**: it watches the production floor on camera, reasons about risk by **reading the site's written safety policy** (and citing the exact clause it relied on), fires risk-graded actions (log → notify → corrective ticket → escalate → Slack), routes each alert to the right worker, and rolls each shift up into **KPI reports + a forward-looking inspection/training plan**. The **VLM makes every risk decision — no hardcoded rules** (edit one line of policy and the verdict flips). Built at the **Zapdos Labs × Antler** hackathon (*AI Agents for the American Industrial Revolution*); in active development since. | Qwen3-VL on **vLLM**, YOLO perception, TF-IDF RAG (OSHA/SOP), Flask |
| **Franka Arm RL (Isaac Lab)** | Reinforcement-learning training for a **Franka Emika Panda** manipulator in **NVIDIA Isaac Lab** — GPU-parallel environments for arm control (reaching / manipulation) with PPO-style policy training in simulation. | Isaac Lab, Isaac Sim, PyTorch, RL |
| **[Archiagents](https://archiagents.com/)** | End-to-end AI agent for architectural design (collaborative project). Ingests project briefs + CAD/DWG/IFC/Revit files, runs requirement dialogue, generates design schemes and photorealistic renders, and outputs IFC4 BIM models with an embedded Autodesk APS viewer. **My role:** brought the architecture-domain expertise (B.Arch background) — shaping the design-requirement logic, the agent's reasoning over building programs, and the IFC4 / BIM modeling that turns AI output into valid design deliverables. | Vercel AI SDK, shadcn/ui, Autodesk APS, IFC4 |
| **[Revit-Civil-AI-Estimator](https://github.com/Lynn-hh/Revit-Civil-AI-Estimator)** | Revit 2025 add-in that uses OpenAI to automate quantity takeoff and cost estimation for civil-engineering workflows. | C#, OpenAI API, Revit |

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

---

## Open Source — Robotics / Simulation Contributions

### isaac-sim/IsaacLab — NVIDIA's GPU robot-learning framework

- **PR [#6235](https://github.com/isaac-sim/IsaacLab/pull/6235) — Documentation fix (merged):** Fixed doc typos and
  a broken image path across asset-import, IMU, task-workflow, and OSC-controller docs. Merged into IsaacLab's
  `develop` branch; added my name to `CONTRIBUTORS.md`.
- **PR [#6237](https://github.com/isaac-sim/IsaacLab/pull/6237) — Bug fix (open, under review):** Four state-machine
  / tutorial scripts called `AppLauncher(headless=args_cli.headless)` *after* registering the full launcher CLI arg
  set — silently dropping every other flag (`--viz`, `--livestream`, `--enable_cameras`, …). Forwarded the full
  parsed args so the flags take effect. **Closes #5572.**
- **PR [#6306](https://github.com/isaac-sim/IsaacLab/pull/6306) — Bug fix (open):** Corrected an invalid task ID in
  the Newton-physics sim-to-sim docs (the documented training command failed as written), plus related typos.

---

## Tech Stack

**Robotics & Simulation**

![Isaac Sim](https://img.shields.io/badge/Isaac%20Sim-76B900?logo=nvidia&logoColor=white)
[![Isaac Lab](https://img.shields.io/badge/Isaac%20Lab-PR%20Merged-76B900?logo=nvidia&logoColor=white)](https://github.com/isaac-sim/IsaacLab/pull/6235)
![ROS 2](https://img.shields.io/badge/ROS%202-22314E?logo=ros&logoColor=white)
![MuJoCo](https://img.shields.io/badge/MuJoCo-000000?logoColor=white)
![Gymnasium](https://img.shields.io/badge/Gymnasium-0081A5?logoColor=white)

**AI / ML**

![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?logo=pytorch&logoColor=white)
![RL (PPO/SAC)](https://img.shields.io/badge/RL-PPO%20%2F%20SAC-5C2D91)
![vLLM](https://img.shields.io/badge/vLLM-D32F2F)
![YOLO](https://img.shields.io/badge/YOLO-00FFFF?logoColor=black)
![RAG](https://img.shields.io/badge/RAG-4B8BBE)
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


