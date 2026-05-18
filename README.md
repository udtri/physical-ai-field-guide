# Physical AI Field Guide

[![License: CC0-1.0](https://img.shields.io/badge/License-CC0_1.0-lightgrey.svg)](http://creativecommons.org/publicdomain/zero/1.0/)

> [!IMPORTANT]
> A personal, opinionated guide for practitioners shipping Physical AI. Not affiliated with, endorsed by, or supported by Microsoft Corporation.

**Physical AI** is the layer of AI that closes the loop with reality — perceiving, reasoning about, and acting in the physical world through sensor data, edge compute, and real-time inference. It runs on the factory floor, in the warehouse, on the operating table, and on the humanoid form factor. It is constrained by latency, safety, and reliability in ways that cloud AI is not, and it ships only when you have all three of: the right **model**, the right **platform**, and a working **production pattern**.

This guide is curated, not exhaustive. It catalogs the work I think practitioners building Physical AI systems should know about right now — the models that meaningfully advance the state of the art, the platforms that production teams actually run on, and the reference architectures that turn the two into something deployable. Entries that haven't shipped public code, weights, or a serious reference implementation are omitted on purpose.

For Azure + NVIDIA production work, the headline reference is **[microsoft/physical-ai-toolchain](https://github.com/microsoft/physical-ai-toolchain)** — an open-source, production-ready blueprint for running robotics and Physical AI workloads end-to-end (Isaac Sim/Lab → Azure ML training → Jetson edge deployment, with managed identities, IaC, and GitOps).

---

## The stack

| Layer | What it is | Look for |
|---|---|---|
| **Use cases** | Production deployment patterns | Visual inspection, predictive maintenance, AMRs, manipulation, humanoids |
| **Models** | The intelligence layer | Time-series FMs, VLMs, VLAs, world FMs |
| **Platform** | Simulation, edge runtimes, orchestration | Isaac Sim/Lab, ROS 2, Azure IoT Operations, Jetson, ONNX/TensorRT |
| **Data plane** | Connectivity between physical and digital | OPC UA, MQTT, time-series stores |
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
9. [Reference architectures](#9-reference-architectures)

---

## 1. Use cases

The hard part of Physical AI is not the model — it is the deployment pattern around it. Each use case below is a pattern that has crossed from research demo to production reference, with a public blueprint you can read, fork, or learn from. Pair every pattern with a platform decision (section 2) and an operational reference architecture (section 9).

- **Visual inspection / quality control** — Edge VLM or anomaly model behind a real-time API, with operator-in-the-loop labelling. Reference: [Anomalib](https://github.com/open-edge-platform/anomalib) for the model layer; [microsoft/physical-ai-toolchain](https://github.com/microsoft/physical-ai-toolchain) for the inspection-on-Azure-IoT-Operations pattern; pair with Florence-2 or Grounding DINO when you need zero-shot defect description.
- **Predictive maintenance & anomaly detection on time-series** — Time-series foundation model running zero-shot on sensor streams, alerting through the IIoT data plane. Reference: [Chronos-2](https://github.com/amazon-science/chronos-forecasting) or [TimesFM 2.5](https://github.com/google-research/timesfm) behind Azure ML managed endpoints; [aio-edge-intelligence](https://github.com/udtri/aio-edge-intelligence) for the deployment pattern on Arc-enabled Kubernetes.
- **Autonomous mobile robots & warehouse logistics** — Fleet simulation in Isaac Sim, sim-to-real with Isaac Lab, OTA model rollout to Jetson, telemetry back to the cloud. Reference: [microsoft/physical-ai-toolchain](https://github.com/microsoft/physical-ai-toolchain) is the primary Azure + NVIDIA AMR blueprint; use [Cosmos-Predict2](https://github.com/nvidia-cosmos/cosmos-predict2) for synthetic navigation augmentation.
- **Robotic manipulation & pick-and-place** — Teleop data collection → VLA fine-tune → eval in sim → deploy on real hardware. Reference: [LeRobot](https://github.com/huggingface/lerobot) gives you the full loop (teleop → dataset → `lerobot-train --policy=pi0`); [ManiSkill 3](https://github.com/haosulab/ManiSkill) for standardized eval.
- **Embodied AI & humanoid deployment** — Cosmos for synthetic training data → Isaac Lab for locomotion RL → GR00T for the policy → Jetson Thor for inference. Reference: [Isaac-GR00T](https://github.com/NVIDIA/Isaac-GR00T) is the only end-to-end open-weight stack for this pattern; `microsoft/physical-ai-toolchain` extends it to managed Azure deployment.

---

## 2. Platform

A Physical AI platform has three jobs: simulate the world cheaply, train at cloud scale, and serve inference on the edge with latency and safety guarantees. The 2024–2025 landscape has settled around the NVIDIA stack for simulation and edge GPU, the Azure stack for cloud training and edge orchestration, and a handful of inference runtimes you'll always end up using.

### Simulation & robot learning

- **[NVIDIA Isaac Sim](https://developer.nvidia.com/isaac/sim)** — NVIDIA. RTX-accelerated physics simulator on Omniverse; standard for AMR, manipulation, and humanoid sim-to-real.
- **[NVIDIA Isaac Lab](https://github.com/isaac-sim/IsaacLab)** — NVIDIA. GPU-accelerated RL/IL framework on Isaac Sim 5.x; the substrate the GR00T training workflow assumes.
- **[Genesis](https://github.com/Genesis-Embodied-AI/Genesis)** — community. Universal physics engine (rigid + MPM + SPH + FEM + PBD) in one Python-native framework; 43M FPS on a single 4090.
- **[MuJoCo Playground](https://github.com/google-deepmind/mujoco_playground)** — Google DeepMind. Curated GPU-accelerated RL environments on MJX; DeepMind's recommended successor to Brax envs.
- **[MuJoCo / MJX](https://github.com/google-deepmind/mujoco)** — Google DeepMind. The differentiable JAX-native physics backend; standard for analytic policy gradients.
- **[ManiSkill 3](https://github.com/haosulab/ManiSkill)** — UCSD / Hillbot. SAPIEN-backed parallel manipulation simulator with real2sim examples; baselines for Octo, RDT-1B, and RT-X.

### Edge runtimes & inference

- **[ONNX Runtime](https://github.com/microsoft/onnxruntime)** — Microsoft. The portability standard from PyTorch/TF training to heterogeneous edge hardware.
- **[TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM)** — NVIDIA. Required for serving VLA and world-model components at Jetson Orin/Thor throughput.
- **[NVIDIA Triton Inference Server](https://github.com/triton-inference-server/server)** — NVIDIA. Multi-framework production inference server; the right primitive for multi-model robot pipelines (perception → reasoning → action).
- **[ExecuTorch](https://github.com/pytorch/executorch)** — Meta / PyTorch. PyTorch ahead-of-time export with a 50 KB runtime; 12+ hardware backends from MCUs to mobile to robots.

### Cloud, orchestration & edge management

- **[Azure IoT Operations](https://learn.microsoft.com/azure/iot-operations/)** — Microsoft. Kubernetes-native edge data plane (MQTT broker, OPC UA connector, dataflows) on Arc-enabled clusters. GA Nov 2024.
- **[Azure Arc](https://learn.microsoft.com/azure/azure-arc/)** — Microsoft. Hybrid management plane that lets you treat edge sites as first-class Azure resources.
- **[NVIDIA OSMO](https://developer.nvidia.com/osmo)** — NVIDIA. Workflow orchestration glue between Cosmos data generation, Isaac Lab training, and model evaluation on DGX Cloud.
- **[K3s](https://github.com/k3s-io/k3s)** — Rancher / SUSE. Lightweight certified Kubernetes for edge sites; the most common substrate for Arc-enabled edge.
- **[ROS 2](https://github.com/ros2/ros2)** — Open Source Robotics Foundation. The robotics middleware; assume it for any real-robot integration unless you have a strong reason not to.
- **[NVIDIA Jetson (Orin, Thor)](https://developer.nvidia.com/embedded-computing)** — NVIDIA. The dominant edge GPU for VLA and VLM inference at the robot.

---

## 3. Time-series & sensor foundation models

For two decades, time-series forecasting and anomaly detection were per-dataset bespoke modelling exercises. Since 2024, a small set of foundation models trained on hundreds of billions of points across diverse domains have become the right default — zero-shot performance now matches or beats hand-tuned baselines on many sensor workloads, and fine-tuning is fast. The list below is the short list I'd actually evaluate today.

- **[TimesFM 2.5](https://github.com/google-research/timesfm)** — Google Research. Decoder-only TS foundation model; 200M params, 16k context, continuous quantile head. *Sept 2025.* Strongest open zero-shot univariate baseline; production paths via BigQuery ML and Vertex Model Garden.
- **[Chronos-2](https://github.com/amazon-science/chronos-forecasting)** — Amazon. Zero-shot univariate, multivariate, **and** covariate-informed forecasting in one model. *Oct 2025.* SOTA on fev-bench and GIFT-Eval; SageMaker JumpStart deployment path.
- **[Chronos-Bolt](https://github.com/amazon-science/chronos-forecasting)** — Amazon. Patch-based direct multi-step variant; 250× faster and 20× more memory-efficient than original Chronos. *Nov 2024.* The latency-constrained drop-in.
- **[MOIRAI 2.0 / Moirai-MoE](https://github.com/SalesforceAIResearch/uni2ts)** — Salesforce AI Research. Universal TS transformer trained on LOTSA; MoE variant adds sparse routing; Moirai 2.0-R-small released Aug 2025. The GIFT-Eval benchmark workhorse.
- **[MOMENT](https://huggingface.co/AutonLab)** — CMU Auton Lab. Open TS foundation models (up to 385M) pretrained on MIMIC-III, ETT, ECG, and sensor data — the only major TS FM with explicit healthcare and physiological-signal coverage.
- **[Lag-Llama](https://github.com/time-series-foundation-models/lag-llama)** — Mila / IIT Delhi. Probabilistic LLaMA-style TS FM with lag-based tokenization; the lightweight OSS baseline.
- **[Toto](https://www.datadoghq.com/blog/introducing-toto/)** — Datadog. 1B-param decoder-only model pretrained exclusively on observability telemetry. *Jan 2025.* Not open-weights yet, but the only model purpose-built for cloud-infrastructure sensor data and worth tracking.

---

## 4. Computer vision & vision-language models

The shift to evaluate in 2024–2025 was from task-specific CV models (one detector, one segmenter, one classifier) to general-purpose VLMs that handle detection, grounding, segmentation, captioning, and document parsing from one checkpoint. For Physical AI specifically, the question is rarely "which model is best on COCO" — it's "which model survives fine-tuning on my industrial dataset and runs at the latency my line needs."

- **[SAM 2.1](https://github.com/facebookresearch/sam2)** — Meta FAIR. Streaming-memory transformer for promptable image and video segmentation; the de facto segmentation backbone for robot perception.
- **[Qwen3-VL](https://github.com/QwenLM/Qwen3-VL)** — Alibaba / Qwen. Dynamic-resolution VLM with native video, grounding, document parsing, and agentic tool use; available in Dense and MoE up to 72B+; top OSS scores on DocVQA, VideoMME, and spatial reasoning.
- **[InternVL 3](https://github.com/OpenGVLab/InternVL)** — Shanghai AI Lab. Fully open VLM family (1B → 78B) with tile-based dynamic resolution; the most capable fully-open option for high-resolution industrial imagery.
- **[Florence-2](https://huggingface.co/microsoft/Florence-2-large)** — Microsoft. 0.77B seq-to-seq VLM that unifies captioning, grounding, detection, segmentation, and OCR in a single checkpoint; uniquely versatile at the edge.
- **[Anomalib](https://github.com/open-edge-platform/anomalib)** — Intel / OpenVINO. 30+ anomaly detection algorithms (PatchCore, STFPM, WinCLIP, Dinomaly) with direct OpenVINO export and a no-code Studio UI. The production-default for visual defect detection.
- **[YOLO11](https://github.com/ultralytics/ultralytics)** — Ultralytics. Latest YOLO generation; still the industry default for real-time detection on embedded targets, with OBB and pose estimation out of the box.
- **[RF-DETR](https://github.com/roboflow/RF-DETR)** — Roboflow. Real-time deformable-DETR (55.3 AP COCO at >60 FPS); best choice for fine-tuned open-set detection without per-class retraining overhead.
- **[Grounded SAM 2](https://github.com/IDEA-Research/Grounded-SAM-2)** — IDEA Research. Grounding DINO 1.5/1.6 + SAM 2 for text-prompted detect-and-segment-in-video; the strongest open zero-shot pipeline for unstructured environments.

---

## 5. Vision-language-action models

VLAs are the model class that turned robotics from "one policy per task" into "one policy per embodiment, sometimes per category." 2024–2025 was the year they actually started working — both as research artifacts and as production-deployable code with open weights. The list below is the set of VLAs you can pick up, fine-tune on your data, and deploy on real hardware today.

- **[π₀ / π₀-FAST / π₀.5 (openpi)](https://github.com/Physical-Intelligence/openpi)** — Physical Intelligence. Flow-matching VLA with fast autoregressive variant; π₀.5 adds open-world generalization via knowledge insulation. The empirical state of the art for dexterous manipulation, with fully open weights.
- **[Isaac GR00T N1.7](https://github.com/NVIDIA/Isaac-GR00T)** — NVIDIA. Open generalist humanoid VLA (VLM perception + diffusion action head); now built on a Cosmos-Reason / Qwen3-VL backbone. The only humanoid foundation model with end-to-end NVIDIA stack integration (Isaac Lab → Jetson Thor) under Apache 2.0.
- **[LeRobot](https://github.com/huggingface/lerobot)** — Hugging Face. The practical integration layer: hardware-agnostic robot API, `LeRobotDataset` format, and out-of-the-box policies including π₀.5, GR00T N1.5, SmolVLA, and Diffusion Policy. If you need to deploy a VLA on real hardware without writing a custom stack, start here.
- **[SmolVLA](https://huggingface.co/lerobot/smolvla_base)** — Hugging Face. Compact VLA designed for consumer-grade GPUs and Jetson-class edge — the only VLA explicitly engineered for resource-constrained deployment.
- **[RDT-1B](https://github.com/thu-ml/RoboticsDiffusionTransformer)** — Tsinghua THUML. 1B-parameter diffusion transformer for bimanual manipulation with language + multi-image conditioning. ICLR 2025 oral; the open bimanual baseline.
- **[OpenVLA](https://github.com/openvla/openvla)** — Stanford / UC Berkeley. 7B Prismatic-VLM fine-tuned on Open X-Embodiment. The established open 7B baseline — well-benchmarked, widely fine-tuned, with LoRA scripts.
- **[Octo](https://github.com/octo-models/octo)** — UC Berkeley RAIL. 27M–93M generalist diffusion policy pretrained on 800k trajectories; the go-to lightweight option for fast sim-to-real transfer.

---

## 6. World foundation models

World foundation models are the answer to the "where does training data come from?" problem in robotics. Instead of collecting another 10,000 hours of teleop, you generate physically plausible video and sensor data from a world model and use it to train or augment your policy. NVIDIA Cosmos is currently the only open, complete suite for this; the rest of the field is research-stage but moving fast.

- **[Cosmos-Predict2](https://github.com/nvidia-cosmos/cosmos-predict2)** — NVIDIA. Text2World and Video2World generation up to 14B params, with post-training scripts for custom robot and AV domains. The only complete open-weight world model suite usable for robotics synthetic data generation at scale.
- **[Cosmos-Transfer1](https://github.com/nvidia-cosmos/cosmos-transfer1)** — NVIDIA. ControlNet-style world-to-world transfer that converts depth/segmentation/edge/LiDAR/HDMap signals into photorealistic video; single-step distilled variant released Aug 2025. The key sim-to-photoreal bridge for domain randomization.
- **[Cosmos-Reason1](https://github.com/nvidia-cosmos/cosmos-reason1)** — NVIDIA. 7B VLM (Qwen2.5-VL base + SFT + RL) for physical-AI reasoning — spatial-temporal understanding and trajectory critic for plausibility evaluation.
- **[Genie 2](https://deepmind.google/discover/blog/genie-2-a-large-scale-foundation-world-model/)** — Google DeepMind. Action-conditioned world model that generates playable 3D environments from a single image. No public code or weights yet, but the directional bet that large-scale video pretraining can produce controllable physics without an explicit simulator.
- **[1X World Model](https://github.com/1x-technologies/1xgpt)** — 1X Technologies. Compact (~500M) action-conditioned model trained on real humanoid robot data — the only public world model built on humanoid telemetry rather than AV or game footage.
- **[Open-Sora 2.0](https://github.com/hpcaitech/Open-Sora)** — HPC-AI Tech. Highest-quality open generative video backbone; not robotics-specific, but the right starting point for fine-tuning your own world model on robot footage.

---

## 7. Data & connectivity

A model is only as good as the data plane underneath it. For Physical AI that means industrial protocols (OPC UA, MQTT) for getting bits off the equipment, a broker that can hold up under fan-out and fan-in, and a store that handles time-series at scale. Keep this layer boring on purpose — exotic choices here are how you get paged at 3 a.m.

- **[OPC UA](https://opcfoundation.org/)** — OPC Foundation. The industrial interoperability standard; assume it for any greenfield OT integration.
- **[MQTT](https://mqtt.org/)** — OASIS. Lightweight pub/sub for constrained devices and links.
- **[Eclipse Mosquitto](https://github.com/eclipse-mosquitto/mosquitto)** — Eclipse. The canonical open-source MQTT broker.
- **[Apache Kafka](https://github.com/apache/kafka)** — Apache. Distributed event streaming for high-throughput, real-time pipelines once data crosses into the cloud.
- **[InfluxDB](https://github.com/influxdata/influxdb)** / **[TimescaleDB](https://github.com/timescale/timescaledb)** — InfluxData / Timescale. The two defaults for time-series storage; pick by team familiarity and SQL needs.
- **[Apache PLC4X](https://github.com/apache/plc4x)** — Apache. Universal protocol adapter for talking to industrial PLCs through one API.

---

## 8. Datasets

A short list of benchmark datasets that remain useful — the canonical ones for industrial time-series and visual inspection. There are many more; these are the ones I'd actually use to baseline a new project.

- **[NASA C-MAPSS](https://data.nasa.gov/Aerospace/CMAPSS-Jet-Engine-Simulated-Data/ff5v-kuh6)** — Turbofan engine degradation; the standard for remaining-useful-life (RUL) benchmarks.
- **[CWRU Bearing Dataset](https://engineering.case.edu/bearingdatacenter)** — Vibration data with seeded bearing faults; the de facto fault-diagnosis baseline.
- **[MVTec AD](https://www.mvtec.com/company/research/datasets/mvtec-ad)** — Industrial texture and object anomaly detection; the canonical benchmark Anomalib reports on.
- **[Open X-Embodiment](https://robotics-transformer-x.github.io/)** — 22-embodiment robot dataset; the largest open trajectory corpus used to pretrain OpenVLA, Octo, and RT-X.
- **[DROID](https://droid-dataset.github.io/)** — 76k teleoperated trajectories across 564 scenes and 86 tasks; the standard manipulation pretraining set referenced by π₀-FAST.

---

## 9. Reference architectures

End-to-end blueprints that wire the model, platform, and data layers above into something you can actually deploy. These are the repos I'd point a team at on day one if they were building a new Physical AI workload — they short-circuit weeks of architecture decisions.

- **[microsoft/physical-ai-toolchain](https://github.com/microsoft/physical-ai-toolchain)** — Microsoft. **The production reference for running robotics and Physical AI workloads on Azure + NVIDIA.** Open-source, IaC-driven (Terraform), and integrates Isaac Sim / Isaac Lab, NVIDIA OSMO, Azure ML, AKS, Azure Arc, MLflow, and Jetson edge deployment with Entra ID and managed identities. Covers simulation, edge data capture, ROS-to-LeRobot conversion, training, ONNX/TensorRT packaging, and GitOps deployment to the edge. If you are evaluating Azure for Physical AI, start here.
- **[udtri/aio-edge-intelligence](https://github.com/udtri/aio-edge-intelligence)** — Deploys time-series foundation models for sensor anomaly detection on Arc-enabled Kubernetes with Azure IoT Operations as the data plane. The smaller, sensor-side counterpart to physical-ai-toolchain.
- **[Azure-Samples/explore-iot-operations](https://github.com/Azure-Samples/explore-iot-operations)** — Microsoft. Official samples and quickstarts for Azure IoT Operations; the right starting point for getting the data plane online before adding models on top.

---

## Contributing

This is a curated, opinionated guide rather than a community-edited awesome-list. If you think something belongs here (or shouldn't), open an issue. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the contributors have waived all copyright and related or neighboring rights to this work. See [LICENSE](LICENSE) for details.

## Disclaimer

This is a personal, curated collection for educational and reference purposes. It is not an official Microsoft product, service, or recommendation.

## Trademark notice

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft trademarks or logos is subject to and must follow [Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/legal/intellectualproperty/trademarks/usage/general). Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship. Any use of third-party trademarks or logos is subject to those third parties' policies.
