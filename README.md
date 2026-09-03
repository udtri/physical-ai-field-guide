# Physical AI Field Guide

[![License: CC0-1.0](https://img.shields.io/badge/License-CC0_1.0-lightgrey.svg)](http://creativecommons.org/publicdomain/zero/1.0/)

_Last reviewed: September 3, 2026_

> [!IMPORTANT]
> An independent, opinionated guide for practitioners shipping Physical AI.

**Physical AI** closes the loop between software and reality. It observes through sensors, reasons over physical state, and acts through people or machines. It runs on factory floors, in warehouses, in hospitals, and inside robots. Latency, safety, reliability, and incomplete data are product requirements—not infrastructure details.

This guide is curated, not exhaustive. It tracks models with public weights, usable APIs, or serious reference implementations. A benchmark result earns attention; an observable, reversible production loop earns trust.

For Azure + NVIDIA work, the headline reference is **[microsoft/physical-ai-toolchain](https://github.com/microsoft/physical-ai-toolchain)**. It now offers a local-first T0 path and a graduated T0–T5 architecture for capture, training, evaluation, deployment, and fleet operations.

## What changed in 2026

- **[TimesFM 3.0](https://research.google/blog/timesfm-3-a-zero-shot-foundation-model-for-multivariate-forecasting/)** adds native multivariate forecasting and covariates. Its current weights are non-commercial, so TimesFM 2.5 remains the permissive deployment choice.
- **[Gemini Robotics 2](https://deepmind.google/blog/gemini-robotics-2-brings-whole-body-intelligence-to-robots/)** separates embodied reasoning, whole-body control, and an on-device VLA. ER 2 has a public API; the action models remain early access.
- **[Isaac GR00T N1.7](https://developer.nvidia.com/blog/develop-humanoid-robot-policies-end-to-end-with-nvidia-isaac-gr00t/)** ships an Apache-2.0, 3B VLA with ONNX and TensorRT export paths.
- **[Cosmos 3](https://github.com/NVIDIA/Cosmos)** unifies physical reasoning, world generation, sound, and action modeling across 4B, 16B, and 64B variants.
- **[SAM 3.1](https://github.com/facebookresearch/sam3/blob/main/RELEASE_SAM3p1.md)** makes language-prompted, multi-object video segmentation substantially more practical.

---

## The stack

| Layer | What it is | Look for |
|---|---|---|
| **Use cases** | Production deployment patterns | Visual inspection, predictive maintenance, AMRs, manipulation, humanoids |
| **Models** | The intelligence layer | Time-series FMs, VLMs, VLAs, world FMs |
| **Platform** | Simulation, edge runtimes, orchestration | Isaac Sim/Lab, ROS 2, Azure IoT Operations, Jetson, ONNX/TensorRT |
| **Data plane** | State, context, and lineage across the loop | OPC UA, MQTT, ROS 2, MCAP, OpenUSD, time-series stores |
| **Operations** | Evaluation, rollout, and feedback | Shadow mode, safety gates, drift, rollback, fleet telemetry |
| **Reference architectures** | End-to-end blueprints you can fork | `microsoft/physical-ai-toolchain`, `aio-edge-intelligence` |

---

## Contents

1. [Use cases](#1-use-cases)
2. [Platform](#2-platform)
3. [Time-series & sensor foundation models](#3-time-series--sensor-foundation-models)
4. [Computer vision & vision-language models](#4-computer-vision--vision-language-models)
5. [Vision-language-action models](#5-vision-language-action-models)
6. [World foundation models](#6-world-foundation-models)
7. [Data & connectivity](#7-data--connectivity)
8. [Datasets](#8-datasets)
9. [Evaluation & operations](#9-evaluation--operations)
10. [Reference architectures](#10-reference-architectures)

---

## 1. Use cases

The hard part of Physical AI is not the model — it is the deployment pattern around it. Each use case below is a pattern that has crossed from research demo to production reference, with a public blueprint you can read, fork, or learn from. Pair every pattern with a platform decision (section 2), an evaluation plan (section 9), and an operational reference architecture (section 10).

- **Visual inspection / quality control** — Edge VLM or anomaly model behind a real-time API, with operator-in-the-loop labelling. Reference: [Anomalib](https://github.com/open-edge-platform/anomalib) for the model layer; [microsoft/physical-ai-toolchain](https://github.com/microsoft/physical-ai-toolchain) for the inspection-on-Azure-IoT-Operations pattern; pair with Florence-2 or Grounding DINO when you need zero-shot defect description.
- **Predictive maintenance & time-series forecasting** — Start with a zero-shot foundation model, but keep deterministic thresholds and failure policies outside the model. Reference: [Chronos-2](https://github.com/amazon-science/chronos-forecasting), [TimesFM](https://github.com/google-research/timesfm), or [Toto 2.0](https://github.com/DataDog/toto); [aio-edge-intelligence](https://github.com/udtri/aio-edge-intelligence) shows the provider pattern on Kubernetes.
- **Autonomous mobile robots & warehouse logistics** — Fleet simulation in Isaac Sim, sim-to-real with Isaac Lab, OTA model rollout to Jetson, telemetry back to the cloud. Reference: [microsoft/physical-ai-toolchain](https://github.com/microsoft/physical-ai-toolchain) is an Azure + NVIDIA AMR blueprint; use [Cosmos 3](https://github.com/NVIDIA/Cosmos) to evaluate synthetic navigation data and physical reasoning.
- **Robotic manipulation & pick-and-place** — Teleop data collection → VLA fine-tune → eval in sim → deploy on real hardware. Reference: [LeRobot](https://github.com/huggingface/lerobot) gives you the full loop (teleop → dataset → `lerobot-train --policy=pi0`); [ManiSkill 3](https://github.com/haosulab/ManiSkill) for standardized eval.
- **Embodied AI & humanoid deployment** — Cosmos for synthetic data → Isaac Lab for training and evaluation → GR00T for the policy → Jetson Thor for inference. [Isaac GR00T N1.7](https://github.com/NVIDIA/Isaac-GR00T) offers an open, commercially usable integrated path. [Gemini Robotics 2](https://deepmind.google/models/gemini-robotics/) is a managed-model counterpoint.

---

## 2. Platform

A Physical AI platform has four jobs: capture trustworthy demonstrations, simulate edge cases, train and evaluate policies, and run them safely at the edge. In 2026 the useful abstraction is a local-first loop that can graduate to cloud training and fleet operations without changing data contracts.

### Simulation & robot learning

- **[NVIDIA Isaac Sim](https://developer.nvidia.com/isaac/sim)** — NVIDIA. RTX-accelerated physics simulator on Omniverse for AMR, manipulation, and humanoid sim-to-real.
- **[NVIDIA Isaac Lab](https://github.com/isaac-sim/IsaacLab)** — NVIDIA. GPU-accelerated RL/IL framework on Isaac Sim 5.x; the substrate the GR00T training workflow assumes.
- **[Genesis](https://github.com/Genesis-Embodied-AI/Genesis)** — community. GPU-parallel, Python-native physics across rigid bodies, MPM, SPH, FEM, and PBD.
- **[MuJoCo Playground](https://github.com/google-deepmind/mujoco_playground)** — Google DeepMind. Curated GPU-accelerated RL environments on MJX; DeepMind's recommended successor to Brax envs.
- **[MuJoCo / MJX](https://github.com/google-deepmind/mujoco)** — Google DeepMind. A differentiable, JAX-native physics backend for accelerated simulation and optimization.
- **[ManiSkill 3](https://github.com/haosulab/ManiSkill)** — UCSD / Hillbot. SAPIEN-backed parallel manipulation simulator with real2sim examples; baselines for Octo, RDT-1B, and RT-X.
- **[Isaac Lab-Arena](https://github.com/isaac-sim/IsaacLab-Arena)** — NVIDIA. A simulation evaluation framework for repeatable policy comparisons across tasks, embodiments, and environments.

### Edge runtimes & inference

- **[ONNX Runtime](https://github.com/microsoft/onnxruntime)** — Microsoft. The portability standard from PyTorch/TF training to heterogeneous edge hardware.
- **[TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM)** — NVIDIA. An optimized NVIDIA path for serving language and multimodal model components.
- **[NVIDIA Triton Inference Server](https://github.com/triton-inference-server/server)** — NVIDIA. A multi-framework inference server for multi-model pipelines such as perception → reasoning → action.
- **[ExecuTorch](https://github.com/pytorch/executorch)** — Meta / PyTorch. PyTorch ahead-of-time export with a 50 KB runtime; 12+ hardware backends from MCUs to mobile to robots.

### Cloud, orchestration & edge management

- **[Azure IoT Operations](https://learn.microsoft.com/azure/iot-operations/)** — Microsoft. Kubernetes-native edge data plane (MQTT broker, OPC UA connector, dataflows) on Arc-enabled clusters. GA Nov 2024.
- **[Azure Arc](https://learn.microsoft.com/azure/azure-arc/)** — Microsoft. Hybrid management plane that lets you treat edge sites as first-class Azure resources.
- **[NVIDIA OSMO](https://developer.nvidia.com/osmo)** — NVIDIA. Workflow orchestration glue between Cosmos data generation, Isaac Lab training, and model evaluation on DGX Cloud.
- **[K3s](https://github.com/k3s-io/k3s)** — Rancher / SUSE. Lightweight certified Kubernetes for edge sites, including Arc-enabled deployments.
- **[ROS 2](https://github.com/ros2/ros2)** — Open Source Robotics Foundation. The robotics middleware; assume it for any real-robot integration unless you have a strong reason not to.
- **[NVIDIA Jetson (Orin, Thor)](https://developer.nvidia.com/embedded-computing)** — NVIDIA. An edge GPU platform for VLA and VLM inference at the robot.

---

## 3. Time-series & sensor foundation models

Time-series foundation models are now credible zero-shot baselines. They are not a substitute for backtesting against seasonal naïve, gradient-boosted, and domain-specific models. Compare point accuracy, calibration, latency, memory, missing-data behavior, and performance after distribution shifts.

- **[TimesFM 3.0](https://github.com/google-research/timesfm)** — Google Research. A 330M-parameter model trained on more than 1T points, with native multivariate targets and past or known-future covariates. *August 2026.* Current checkpoint weights are restricted to non-commercial, non-production use.
- **[TimesFM 2.5](https://github.com/google-research/timesfm)** — Google Research. A 200M-parameter univariate model with 16k context and quantile forecasts. Apache-2.0 weights and the safer TimesFM choice for product evaluation.
- **[Chronos-2](https://github.com/amazon-science/chronos-forecasting)** — Amazon. A 120M-parameter, Apache-2.0 model for zero-shot univariate, multivariate, and covariate-informed forecasting. Managed deployment paths exist through SageMaker.
- **[Toto 2.0](https://github.com/DataDog/toto)** — Datadog. An Apache-2.0 family from 4M to 2.5B parameters, trained on observability and synthetic data. Evaluate the small checkpoints when edge memory matters.
- **[Chronos-Bolt](https://github.com/amazon-science/chronos-forecasting)** — Amazon. Patch-based direct multi-step models optimized for low-latency forecasting. Still a strong constrained-runtime baseline.
- **[MOIRAI 2.0 / Moirai-MoE](https://github.com/SalesforceAIResearch/uni2ts)** — Salesforce AI Research. Universal forecasting models trained on LOTSA, with sparse MoE variants and broad benchmark coverage.
- **[MOMENT](https://github.com/moment-timeseries-foundation-model/moment)** — CMU Auton Lab. A useful multi-task baseline spanning forecasting, reconstruction, anomaly detection, classification, and imputation.

---

## 4. Computer vision & vision-language models

The useful 2026 pattern is a shared visual backbone with small task adapters, plus a specialized real-time detector where latency demands it. For industrial vision, test the actual defect distribution, camera geometry, lighting changes, and smallest important feature—not only public benchmarks.

- **[SAM 3.1](https://github.com/facebookresearch/sam3)** — Meta. Promptable concept detection, segmentation, and tracking from text or visual examples. The 3.1 release adds a much faster multi-object video path; validate export support before choosing it for an edge target.
- **[SAM 3D](https://github.com/facebookresearch/sam-3d-objects)** — Meta. Open checkpoints and inference code for reconstructing objects and scenes from a single image. Useful for bootstrapping spatial assets and pose, not a metrology replacement.
- **[DINOv3](https://github.com/facebookresearch/dinov3)** — Meta. Self-supervised visual backbones from edge-sized ConvNeXt variants to ViT-7B. A strong frozen backbone when industrial labels are scarce.
- **[Qwen3-VL](https://github.com/QwenLM/Qwen3-VL)** — Alibaba / Qwen. Dynamic-resolution VLM with native video, grounding, document parsing, and tool use; available in Dense and MoE variants.
- **[InternVL 3](https://github.com/OpenGVLab/InternVL)** — Shanghai AI Lab. Open VLM family with tile-based dynamic resolution for high-resolution imagery.
- **[Florence-2](https://huggingface.co/microsoft/Florence-2-large)** — Microsoft. A compact seq-to-seq VLM that unifies captioning, grounding, detection, segmentation, and OCR.
- **[Anomalib](https://github.com/open-edge-platform/anomalib)** — Intel / OpenVINO. A library of anomaly-detection algorithms with OpenVINO export and a no-code Studio UI.
- **[YOLO26](https://docs.ultralytics.com/models/yolo26/)** — Ultralytics. Edge-first, end-to-end detection without NMS, plus segmentation, pose, classification, and oriented boxes. Review AGPL-3.0 or enterprise licensing before product use.
- **[RF-DETR](https://github.com/roboflow/RF-DETR)** — Roboflow. A real-time deformable-DETR family for fine-tuned detection and segmentation.
- **[Grounded SAM 2](https://github.com/IDEA-Research/Grounded-SAM-2)** — IDEA Research. Grounding DINO + SAM 2 for text-prompted detection, segmentation, and tracking in video.

---

## 5. Vision-language-action models

VLAs are moving robotics from one policy per task toward adaptable policies across tasks and embodiments. Access still matters: an impressive closed demo and an open checkpoint with a reproducible fine-tuning path belong in different decision columns.

- **[π₀ / π₀-FAST / π₀.5 (openpi)](https://github.com/Physical-Intelligence/openpi)** — Physical Intelligence. Flow-matching and autoregressive VLA families with public base and task checkpoints. π₀.5 is the current supported path for open-world generalization.
- **[Isaac GR00T N1.7](https://github.com/NVIDIA/Isaac-GR00T)** — NVIDIA. An Apache-2.0, 3B generalist VLA with a Cosmos-Reason2/Qwen3-VL backbone and ONNX/TensorRT export. The tightest open integration across Isaac Lab, LeRobot, and Jetson Thor.
- **[Gemini Robotics 2](https://deepmind.google/models/gemini-robotics/)** — Google DeepMind. A family covering whole-body VLA control, ER 2 high-level planning, and an on-device VLA. ER 2 is available through a public API; VLA access is currently limited to partners.
- **[LeRobot](https://github.com/huggingface/lerobot)** — Hugging Face. The practical integration layer for hardware, `LeRobotDataset`, training, and evaluation. Version 0.6 includes policies such as π₀.5, GR00T N1.7, SmolVLA, XVLA, and world-model integrations.
- **[SmolVLA](https://huggingface.co/lerobot/smolvla_base)** — Hugging Face. A compact VLA designed for consumer-grade GPUs and Jetson-class edge deployment.
- **[RDT-1B](https://github.com/thu-ml/RoboticsDiffusionTransformer)** — Tsinghua THUML. 1B-parameter diffusion transformer for bimanual manipulation with language + multi-image conditioning. ICLR 2025 oral; the open bimanual baseline.
- **[OpenVLA](https://github.com/openvla/openvla)** — Stanford / UC Berkeley. 7B Prismatic-VLM fine-tuned on Open X-Embodiment. The established open 7B baseline — well-benchmarked, widely fine-tuned, with LoRA scripts.
- **[Octo](https://github.com/octo-models/octo)** — UC Berkeley RAIL. A 27M–93M generalist diffusion policy pretrained on 800k trajectories and designed for downstream fine-tuning.

---

## 6. World foundation models

World models expand the long tail of training and evaluation scenarios. They do not remove the need for real demonstrations or a physics simulator. Treat generated data as a versioned dataset with provenance, filters, and downstream acceptance tests.

- **[Cosmos 3](https://github.com/NVIDIA/Cosmos)** — NVIDIA. A unified mixture-of-transformers family that can reason over text and vision or generate vision, sound, and actions. Super is 64B, Nano is 16B, and Edge is 4B; models use the OpenMDW-1.1 license.
- **[Cosmos Curator](https://github.com/NVIDIA-NeMo/Curator)** / **[Cosmos Evaluator](https://github.com/nvidia-cosmos/cosmos-evaluator)** — NVIDIA. The data-processing and automated-evaluation layers around Cosmos. These matter as much as the generator when synthetic data enters a training loop.
- **[Cosmos Reason 2](https://github.com/nvidia-cosmos/cosmos-reason2)** — NVIDIA. The prior-generation physical-reasoning model remains relevant because its 2B variant is the visual backbone inside GR00T N1.7.
- **[Genie 3](https://deepmind.google/blog/genie-3-a-new-frontier-for-world-models/)** — Google DeepMind. A research system for interactive 720p worlds at 20–24 FPS with minute-scale consistency. It is a frontier signal, not an open deployment option.
- **[1X World Model](https://github.com/1x-technologies/1xgpt)** — 1X Technologies. A compact action-conditioned model trained on real humanoid robot data.
- **[Open-Sora 2.0](https://github.com/hpcaitech/Open-Sora)** — HPC-AI Tech. An open generative-video backbone that can be adapted to robot footage, although it is not robotics-specific.

---

## 7. Data & connectivity

A model is only as useful as the operational context around its inputs and outputs. Preserve timestamps, units, coordinate frames, calibration, asset identity, operator actions, model version, and decision outcome. Without that lineage, you have a demo—not a learning system.

- **[OPC UA](https://opcfoundation.org/)** — OPC Foundation. The industrial interoperability standard; assume it for any greenfield OT integration.
- **[MQTT](https://mqtt.org/)** — OASIS. Lightweight pub/sub for constrained devices and links.
- **[ROS 2 / DDS](https://docs.ros.org/en/jazzy/)** — The robot message graph and middleware layer. Treat message schemas and coordinate frames as durable data contracts.
- **[MCAP](https://mcap.dev/)** — An indexed multimodal log container for robotics and sensor data. A practical boundary between capture, replay, inspection, and training.
- **[LeRobotDataset](https://github.com/huggingface/lerobot)** — A shared dataset contract for observations, actions, video, metadata, and policy training.
- **[OpenUSD](https://openusd.org/)** — A composable scene description for geometry, semantics, materials, and simulation assets.
- **[Eclipse Mosquitto](https://github.com/eclipse-mosquitto/mosquitto)** — Eclipse. The canonical open-source MQTT broker.
- **[Apache Kafka](https://github.com/apache/kafka)** — Apache. Distributed event streaming for high-throughput, real-time pipelines once data crosses into the cloud.
- **[Apache Parquet](https://parquet.apache.org/)** — The durable analytical boundary for sensor features, labels, and fleet-scale training data.
- **[InfluxDB](https://github.com/influxdata/influxdb)** / **[TimescaleDB](https://github.com/timescale/timescaledb)** — Operational time-series stores; pick by workload and team familiarity.
- **[Apache PLC4X](https://github.com/apache/plc4x)** — Apache. Universal protocol adapter for talking to industrial PLCs through one API.

---

## 8. Datasets

A short list of benchmark datasets that remain useful — the canonical ones for industrial time-series and visual inspection. There are many more; these are the ones I'd actually use to baseline a new project.

- **[NASA C-MAPSS](https://data.nasa.gov/Aerospace/CMAPSS-Jet-Engine-Simulated-Data/ff5v-kuh6)** — Turbofan engine degradation; the standard for remaining-useful-life (RUL) benchmarks.
- **[CWRU Bearing Dataset](https://engineering.case.edu/bearingdatacenter)** — Vibration data with seeded bearing faults; the de facto fault-diagnosis baseline.
- **[MVTec AD](https://www.mvtec.com/company/research/datasets/mvtec-ad)** — Industrial texture and object anomaly detection; the canonical benchmark Anomalib reports on.
- **[Open X-Embodiment](https://robotics-transformer-x.github.io/)** — A 22-embodiment robot dataset used to pretrain OpenVLA, Octo, and RT-X.
- **[DROID](https://droid-dataset.github.io/)** — 76k teleoperated trajectories across 564 scenes and 86 tasks; the standard manipulation pretraining set referenced by π₀-FAST.

---

## 9. Evaluation & operations

The production question is not “does the model work?” It is “under which conditions should the system trust, defer, stop, or recover?” Build evaluation and operations into the architecture before the first live action.

1. **Record a replayable episode.** Keep raw observations, actions, timestamps, calibration, asset context, and software versions together.
2. **Evaluate offline and in simulation.** Track task success, constraint violations, latency, uncertainty, recovery behavior, and slices that represent the long tail.
3. **Run in shadow mode.** Compare model decisions with the existing process without controlling equipment.
4. **Gate authority.** Start with recommendations, then bounded actions, and only expand the envelope with evidence.
5. **Make rollback physical.** Version models, data contracts, transforms, and safety policies. Preserve a deterministic safe state outside the learned model.
6. **Close the loop.** Route failures and human interventions back into curation, labeling, simulation, and retraining.

Useful starting points: **[Isaac Lab-Arena](https://github.com/isaac-sim/IsaacLab-Arena)** for policy evaluation, **[LeRobot](https://github.com/huggingface/lerobot)** for common eval flows, and **[Cosmos Evaluator](https://github.com/nvidia-cosmos/cosmos-evaluator)** for world-model outputs.

---

## 10. Reference architectures

End-to-end blueprints that wire the model, platform, and data layers above into something you can actually deploy. These are the repos I'd point a team at on day one if they were building a new Physical AI workload — they short-circuit weeks of architecture decisions.

- **[microsoft/physical-ai-toolchain](https://github.com/microsoft/physical-ai-toolchain)** — Microsoft. A local-first T0 path through a T0–T5 adoption model for data capture, simulation, Azure ML/OSMO training, evaluation, ONNX/TensorRT packaging, GitOps deployment, and fleet operations. Start here for an Azure + NVIDIA reference.
- **[udtri/aio-edge-intelligence](https://github.com/udtri/aio-edge-intelligence)** — A smaller sensor-side pattern for MOMENT and Google TimesFM inference on Kubernetes, with MQTT and optional Azure IoT Operations integration.
- **[Azure-Samples/explore-iot-operations](https://github.com/Azure-Samples/explore-iot-operations)** — Microsoft. Official samples and quickstarts for Azure IoT Operations; the right starting point for getting the data plane online before adding models on top.

---

## Contributing

This is a curated, opinionated guide rather than a community-edited awesome-list. If you think something belongs here (or shouldn't), open an issue. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the contributors have waived all copyright and related or neighboring rights to this work. See [LICENSE](LICENSE) for details.

## Disclaimer

This is a personal, curated collection for educational and reference purposes. It is not an official product, service, or recommendation from any company named here.

## Trademark notice

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft trademarks or logos is subject to and must follow [Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/legal/intellectualproperty/trademarks/usage/general). Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship. Any use of third-party trademarks or logos is subject to those third parties' policies.
