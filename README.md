# Hi, I'm Lynn 👋

*Robotics · Reinforcement Learning · LLM Inference · Sim-to-Real*

**PhD student — open to internship opportunities in robot learning, contact-rich manipulation and sim-to-real.**

[![Email](https://img.shields.io/badge/Email-lynnhe02@gmail.com-EA4335?logo=gmail&logoColor=white)](mailto:lynnhe02@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Lin%20He-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/lin-he-566260335/)
[![vLLM](https://img.shields.io/badge/vLLM-Contributor-76B900?logo=github&logoColor=white)](https://github.com/vllm-project/vllm/pulls?q=is%3Apr+author%3ALynn-hh)
[![Isaac Lab](https://img.shields.io/badge/Isaac%20Lab-Contributor-76B900?logo=nvidia&logoColor=white)](https://github.com/isaac-sim/IsaacLab/pulls?q=is%3Apr+author%3ALynn-hh)
[![Newton](https://img.shields.io/badge/Newton%20Physics-Contributor-76B900?logo=github&logoColor=white)](https://github.com/newton-physics/newton/pulls?q=is%3Apr+author%3ALynn-hh)
[![ROS 2](https://img.shields.io/badge/ROS%202-22314E?logo=ros&logoColor=white)](https://docs.ros.org/)
[![Location](https://img.shields.io/badge/Tennessee-US-555555?logo=googlemaps&logoColor=white)](#)

---

## About Me

PhD student working at the intersection of Robot Learning and Large-scale Systems. I train RL policies in simulation
and push them toward real hardware, and I build the AI agents and infrastructure that make both fast and correct.

I came to AI from an unusual direction: architecture, which taught me to reason about complex systems, geometry and
the tradeoffs between elegant ideas and real-world constraints. I love learning new things, digging into hard
problems, and figuring out how to solve them.

- 🤖 **Robotics Reinforcement Learning** — I trained contact-rich manipulation policies in NVIDIA Isaac Lab and
  transferred them from simulation to real robots. I'm a contributor to Isaac Lab, the Newton physics engine
  and MuJoCo Warp.
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

### vLLM (vllm-project/vllm, ~96k★) — Core LLM Inference Engine

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

### Isaac Lab (isaac-sim/IsaacLab, ~8.5k★) — NVIDIA's GPU Robot-Learning Framework

- **PR [#8715](https://github.com/isaac-sim/IsaacLab/pull/8715) — Task-Space Action Clipping (Merged):**
  `ActionTermCfg.clip` keys were matched against joint names and then used to index the task-space clip buffer of
  the DiffIK action term (an `IndexError`, or joint ranges silently applied to pose components), and the OSC term
  never applied them. Both terms now expose `action_names` (`pos_x`, `rot_z`, `stiffness_x`, ...) and resolve
  `clip` against them; a key that matches nothing or a quaternion component raises.
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
- **PR [#7989](https://github.com/isaac-sim/IsaacLab/pull/7989) — Body-Offset Jacobian for DiffIK / OSC (Merged):**
  Fixes the Jacobian shift to the end-effector offset frame used by the Franka IK/OSC tasks.
  - It rotates the lever arm into the root frame and no longer rotates the angular rows.
  - Against finite differences, the maximum error drops from 0.6 to 2e-7.
- **PRs [#7987](https://github.com/isaac-sim/IsaacLab/pull/7987), [#7988](https://github.com/isaac-sim/IsaacLab/pull/7988) — Observation and Actuator Fixes (Merged):**
  - #7987: The multi-body projected-gravity observation crashed with the default all-body selection.
  - #7988: The ANYmal LSTM actuator ignored the DC-motor torque-speed limit.
- **PR [#7997](https://github.com/isaac-sim/IsaacLab/pull/7997) — OVPhysX Joint-Wrench Frame (Merged):** Applied the
  same double-transform fix to the kit-less OVPhysX backend and reused the shared frame-convention test.
- **PR [#6235](https://github.com/isaac-sim/IsaacLab/pull/6235) — Documentation Fixes (Merged).**

### Newton (newton-physics/newton, ~5.9k★) — GPU Physics Engine (NVIDIA · Google DeepMind · Disney Research)

- **PR [#4787](https://github.com/newton-physics/newton/pull/4787) — Per-World Broadphase Bounds in SolverMuJoCo (Merged):**
  With `separate_worlds=True`, worlds whose shapes differ in size, or are resized at runtime, ran MuJoCo Warp's
  broadphase with world 0's bounds: `geom_size` was written per world but `geom_aabb` and `geom_rbound` were not.
  The update kernel now derives both per world for the primitive geoms the way `mj_setConst` does, and
  `geom_aabb` is batched per world.
- **PR [#4306](https://github.com/newton-physics/newton/pull/4306) — Static vs. Dynamic Coulomb Friction (Merged):**
  - Adds a separate static (break-away) friction coefficient to Newton's shape materials.
  - Keeps USD `staticFriction`, which the importer used to discard.
  - Passes the coefficient to the Kamino solver.
  - The change is backward-compatible, and it is the first step toward stick-slip contact
    ([#3560](https://github.com/newton-physics/newton/issues/3560)).
- **PR [#4897](https://github.com/newton-physics/newton/pull/4897) — MJCF/USD `filterparent` and Weld-Group Collision Filters (Open):**
  The importers honor `<flag filterparent="disable"/>` and the USD `mjc:flag:filterparent` scene flag.
  `ModelBuilder` filters parent-child collisions by weld group, as MuJoCo does, and `SolverMuJoCo` exports
  Newton's filter pairs instead of relying on MuJoCo's own parent rule, so a joint that opts out of the filter
  collides in both pipelines.
- **PR [#4307](https://github.com/newton-physics/newton/pull/4307) — USD Joint-State Units (Merged):** Imported angular joint
  velocities were 57.3× too large. The importer now converts deg/s to rad/s for revolute, D6 and merged joints.

### MuJoCo Warp (google-deepmind/mujoco_warp, ~1.6k★) — MuJoCo on NVIDIA Warp

- **PR [#1800](https://github.com/google-deepmind/mujoco_warp/pull/1800) — Batched `set_const` Outputs (Open):**
  Fields that `set_const` derives per world from batched sources (`geom_aabb`, `geom_rbound`,
  `body_subtreemass`) kept a single row, so every world overwrote the same values. `put_model` now batches them
  with their sources, and `set_const` rejects an output with fewer rows than the worlds it computes.

- **PR [#1926](https://github.com/google-deepmind/mujoco_warp/pull/1926) — Contact Parameter Override Flag (Open):**
  Implemented MuJoCo's `override` enable flag, which MJWarp previously refused to load: `o_margin`, `o_solref`,
  `o_solimp` and `o_friction` become per-world (batchable) options applied in the shared contact-parameter helpers of
  every narrowphase, with the NxN/SAP broadphase expansion mirroring MuJoCo's `makeAAMM`; parity-tested against MuJoCo
  for both broadphases and two heterogeneous worlds.

### Warp (NVIDIA/warp, ~7.3k★) — NVIDIA's Python Framework for GPU Simulation and Differentiable Programming

- **PR [#2281](https://github.com/NVIDIA/warp/pull/2281) — Gradients of `wp.frac()` and Vector `wp.mod()` (Open):**
  Found with a finite-difference sweep over ~90 differentiable built-ins: `wp.frac()` and the vector overloads of
  `wp.mod()` were registered as non-differentiable, so `tape.backward()` silently returned zero gradients through them.
  Implemented the adjoints (vector `mod` reuses the scalar rule per component) and extended the existing tests,
  which had skipped these outputs ([issue #2280](https://github.com/NVIDIA/warp/issues/2280)).

### [armguard-mcp](https://github.com/Lynn-hh/armguard-mcp) — Safety-First MCP Server for ROS 2 Manipulators

[![PyPI](https://img.shields.io/pypi/v/armguard-mcp.svg)](https://pypi.org/project/armguard-mcp/)
[![M8ven Verified](https://m8ven.ai/badge/mcp/lynn-hh-armguard-mcp-4jlmpb?v=5f7b4dde398f8793a5072c54b401f450&variant=verified)](https://m8ven.ai/mcp/lynn-hh-armguard-mcp-4jlmpb)

- Lets LLM agents inspect, plan and execute on ROS 2 arms (MoveIt 2, ros2_control, franka_ros2).
- The server enforces the safety envelope itself: joint, workspace and force/torque limits, keep-out zones,
  allowlists and rate limits.
- Motion requires human approval through MCP elicitation, and the server provides a software e-stop and an audit log.
- A reproducible evaluation blocks 23/23 unsafe requests (forged approvals, keep-out targets, over-force contact,
  motion during an e-stop) and executes 1000/1000 random safe moves; CI runs it on every push, along with
  live-MoveIt integration tests on ROS 2 Jazzy.

### [awesome-isaac-lab](https://github.com/Lynn-hh/awesome-isaac-lab) — Curated Resources for NVIDIA Isaac Lab

[![Stars](https://img.shields.io/github/stars/Lynn-hh/awesome-isaac-lab?style=social)](https://github.com/Lynn-hh/awesome-isaac-lab)

- A curated list of about 260 projects, tools, papers and learning resources built on Isaac Lab, covering locomotion,
  humanoids, manipulation, dexterous hands, navigation and sim-to-real deployment.
- Every entry is checked against GitHub and arXiv, and a weekly CI job checks the links.

### [isaaclab-robot-doctor](https://github.com/Lynn-hh/isaaclab-robot-doctor) — Pre-flight Checks and Config Generation for Isaac Lab Robots

[![PyPI](https://img.shields.io/pypi/v/isaaclab-robot-doctor.svg)](https://pypi.org/project/isaaclab-robot-doctor/)

- `check robot.urdf` finds what breaks an Isaac Lab import before Isaac Sim is opened: invalid masses and inertias,
  millimetre units, missing joint limits, mimic joints, kinematic loops, a base that should be fixed, and the
  breadth-first joint order the physics engine exposes. URDF and USD input, no GPU.
- `gen-cfg` writes an `ArticulationCfg` with actuator groups from the joint-name families and starting PD gains
  taken from the medians of the 106 actuator configs shipped with Isaac Lab.

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


