# Hi, I'm Lynn 👋

*Robotics · Reinforcement Learning · LLM Inference · Sim-to-Real*

**PhD Student — Open to Research / Robotics / ML Internships.**

[![Email](https://img.shields.io/badge/Email-lynnhe02@gmail.com-EA4335?logo=gmail&logoColor=white)](mailto:lynnhe02@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Lin%20He-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/lin-he-566260335/)
[![vLLM](https://img.shields.io/badge/vLLM-Contributor-76B900?logo=github&logoColor=white)](https://github.com/vllm-project/vllm/pulls?q=is%3Apr+author%3ALynn-hh)
[![Isaac Lab](https://img.shields.io/badge/Isaac%20Lab-Contributor-76B900?logo=nvidia&logoColor=white)](https://github.com/isaac-sim/IsaacLab/pulls?q=is%3Apr+author%3ALynn-hh)
[![Newton](https://img.shields.io/badge/Newton%20Physics-2%20PRs%20in%20Review-555555?logo=github&logoColor=white)](https://github.com/newton-physics/newton/pulls?q=is%3Apr+author%3ALynn-hh)
[![ROS 2](https://img.shields.io/badge/ROS%202-22314E?logo=ros&logoColor=white)](https://docs.ros.org/)
[![Location](https://img.shields.io/badge/Tennessee-US-555555?logo=googlemaps&logoColor=white)](#)

---

## About Me

PhD student working at the intersection of Robot Learning and Large-scale Systems. I train RL policies in simulation
and push them toward real hardware, and I build the AI agents and infrastructure that make both fast and correct.

I came to AI from an unusual direction: architecture, which taught me to reason about complex systems, geometry and
the tradeoffs between elegant ideas and real-world constraints. I love learning new things, digging into hard
problems, and figuring out how to solve them.

- 🤖 **Robotics Reinforcement Learning** — I trained contact-rich manipulation policies in NVIDIA Isaaclab and
  transferred them from simulation to real robots. I'm a contributor to Isaaclab, with PRs to the Newton physics
  engine.
- 🦾 **ROS 2** — I bridge learned policies to real robot stacks, and I built armguard-mcp, a safety-first MCP
  server that lets LLM agents operate ROS 2 robot arms.
- 🧠 **AI Agents** — I build VLM-driven agents that perceive, reason over policy, and act in a closed
  sense→think→act→report loop (see SafetyCommander below).
- ⚙️ **LLM Infrastructure** — I contribute to vLLM, the core LLM inference engine.

---

## Featured Projects

| Project | Description | Stack |
|---|---|---|
| **[SafetyCommander](https://github.com/Lynn-hh/safety-commander-agent)** — Autonomous Factory Safety Officer | A VLM agent that **owns a safety officer's shift**: it watches the production floor on camera, reasons about risk by **reading the site's written safety policy** (and citing the exact clause it relied on), fires risk-graded actions (log → notify → corrective ticket → escalate → Slack), routes each alert to the right worker, and rolls each shift up into **KPI reports + a forward-looking inspection/training plan**. The **VLM makes every risk decision — no hardcoded rules** (edit one line of policy and the verdict flips). Built at the **Zapdos Labs × Antler** hackathon (*AI Agents for the American Industrial Revolution*); in active development since. | Qwen3-VL on **vLLM**, YOLO perception, TF-IDF RAG (OSHA/SOP), Flask |
| **Arm Reinforcement Learning (Isaac Lab)** | Reinforcement-learning training for a **Franka Emika Panda** manipulator in **NVIDIA Isaac Lab** — GPU-parallel environments for arm control (reaching / manipulation) with PPO-style policy training in simulation. | Isaac Lab, Isaac Sim, PyTorch, RL |
| **[Archiagents](https://archiagents.com/)** | End-to-end AI agent for architectural design (collaborative project). Ingests project briefs + CAD/DWG/IFC/Revit files, runs requirement dialogue, generates design schemes and photorealistic renders, and outputs IFC4 BIM models with an embedded Autodesk APS viewer. **My role:** brought the architecture-domain expertise (B.Arch background) — shaping the design-requirement logic, the agent's reasoning over building programs, and the IFC4 / BIM modeling that turns AI output into valid design deliverables. | Vercel AI SDK, shadcn/ui, Autodesk APS, IFC4 |
| **[Revit-Civil-AI-Estimator](https://github.com/Lynn-hh/Revit-Civil-AI-Estimator)** | Revit 2025 add-in that uses OpenAI to automate quantity takeoff and cost estimation for civil-engineering workflows. | C#, OpenAI API, Revit |

---

## Open Source — LLM Infrastructure Contributions

### vLLM (vllm-project/vllm, ~92k★) — Core LLM Inference Engine

- **PR [#46542](https://github.com/vllm-project/vllm/pull/46542) — `[Perf][LoRA]` (Merged):** Replaced a per-token
  `list.index()` lookup in `convert_mapping` — an O(num_tokens × num_loras) hot path the code had flagged with a
  TODO — by building a reverse `{lora_id: index}` dict once for O(1) lookups, cutting mapping construction to
  O(num_tokens). **2.5×–6.5× faster** in microbenchmarks (e.g. 64 LoRAs / 1024 tokens: 275µs → 42µs), with
  identical output verified against randomized + existing LoRA tests.
- **PR [#46543](https://github.com/vllm-project/vllm/pull/46543) — `[Perf][Multimodal]` (Merged):** Removed a
  wasteful O(num_frames) timestamp-list allocation in GLM-4V / GLM video frame sampling, computing each timestamp
  inline as `frame_index * duration_per_frame`. Byte-for-byte identical behavior with lower memory on long videos.

---

## Open Source — Robotics / Simulation Contributions

### Isaac Lab (isaac-sim/IsaacLab, ~8k★) — NVIDIA's GPU Robot-Learning Framework

- **PR [#7967](https://github.com/isaac-sim/IsaacLab/pull/7967) — Force/Torque Frame Fixes (Merged):**
  - **FORGE:** `change_FT_frame` applied the inverse rotation and the wrong lever-arm sign when re-expressing a
    wrench in another frame. I fixed it and added a point-force reference test.
  - **PhysX Joint-Wrench Sensor:** I suspected the sensor transformed its readings twice, then confirmed it in
    simulation. With rotated and offset joint frames, the raw PhysX wrench matched the analytic value in every
    case, while the sensor output did not. Based on this finding, a maintainer implemented the PhysX fix in this PR.
- **Issue [#7969](https://github.com/isaac-sim/IsaacLab/issues/7969) — Newton Wrist F/T Sensing on Fixed Joints
  (Implemented Upstream in [#7978](https://github.com/isaac-sim/IsaacLab/pull/7978)):** Proposed reporting joint
  reaction wrenches for welded tool flanges and wrist sensors on the Newton backend, for parity with PhysX. The
  proposal covered the failure mode, the physics check and the design.
- **PR [#7989](https://github.com/isaac-sim/IsaacLab/pull/7989) — Body-Offset Jacobian for DiffIK / OSC (Open):**
  Fixes the Jacobian shift to the end-effector offset frame used by the Franka IK/OSC tasks.
  - It rotates the lever arm into the root frame and no longer rotates the angular rows.
  - Against finite differences, the maximum error drops from 0.6 to 2e-7.
- **PRs [#7987](https://github.com/isaac-sim/IsaacLab/pull/7987), [#7988](https://github.com/isaac-sim/IsaacLab/pull/7988) — Observation and Actuator Fixes (Open):**
  - #7987: The multi-body projected-gravity observation crashed with the default all-body selection.
  - #7988: The ANYmal LSTM actuator ignored the DC-motor torque-speed limit.
- **PR [#6235](https://github.com/isaac-sim/IsaacLab/pull/6235) — Documentation Fixes (Merged).**

### Newton (newton-physics/newton, ~5.7k★) — GPU Physics Engine (NVIDIA · Google DeepMind · Disney Research)

- **PR [#4306](https://github.com/newton-physics/newton/pull/4306) — Static vs. Dynamic Coulomb Friction (Open):**
  - Adds a separate static (break-away) friction coefficient to Newton's shape materials.
  - Keeps USD `staticFriction`, which the importer used to discard.
  - Passes the coefficient to the Kamino solver.
  - The change is backward-compatible, and it is the first step toward stick-slip contact
    ([#3560](https://github.com/newton-physics/newton/issues/3560)).
- **PR [#4307](https://github.com/newton-physics/newton/pull/4307) — USD Joint-State Units (Open):** Imported angular joint
  velocities were 57.3× too large. The importer now converts deg/s to rad/s for revolute, D6 and merged joints.

### [armguard-mcp](https://github.com/Lynn-hh/armguard-mcp) — Safety-First MCP Server for ROS 2 Manipulators

- Lets LLM agents inspect, plan and execute on ROS 2 arms (MoveIt 2, ros2_control, franka_ros2).
- The server enforces the safety envelope itself: joint, workspace and force/torque limits, keep-out zones,
  allowlists and rate limits.
- Motion requires human approval through MCP elicitation, and the server provides a software e-stop and an audit log.
- CI runs unit tests and live-MoveIt integration tests on ROS 2 Jazzy.

---

## Tech Stack

**Robotics & Simulation**

![Isaac Sim](https://img.shields.io/badge/Isaac%20Sim-76B900?logo=nvidia&logoColor=white)
![Isaac Lab](https://img.shields.io/badge/Isaac%20Lab-76B900?logo=nvidia&logoColor=white)
![Newton](https://img.shields.io/badge/Newton-76B900?logo=nvidia&logoColor=white)
![ROS 2](https://img.shields.io/badge/ROS%202-22314E?logo=ros&logoColor=white)
![MuJoCo](https://img.shields.io/badge/MuJoCo-000000?logoColor=white)
![Gymnasium](https://img.shields.io/badge/Gymnasium-0081A5?logoColor=white)

**AI / ML**

![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?logo=pytorch&logoColor=white)
![RL (PPO/SAC)](https://img.shields.io/badge/RL%20(PPO%20%2F%20SAC)-5C2D91)
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


