# Physical AI Field Guide

[![License: CC0-1.0](https://img.shields.io/badge/License-CC0_1.0-lightgrey.svg)](http://creativecommons.org/publicdomain/zero/1.0/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

> A practitioner's reference for Physical AI, Edge Computing, Industrial IoT, and AI/ML at the manufacturing edge — foundation models, frameworks, datasets, and architectures for building AI systems that operate in the real world.

**Physical AI** = AI systems that perceive, reason about, and act in the physical world through sensor data, edge computing, and real-time inference — not in a data center, but on the factory floor.

This covers the full stack: from foundation models trained on physical-world data, through edge inference frameworks, to the sensors and actuators that connect AI to reality.

---

## Contents

- [Foundation Models for Physical Data](#-foundation-models-for-physical-data)
- [Edge AI Frameworks & Platforms](#-edge-ai-frameworks--platforms)
- [Industrial IoT & Sensor Intelligence](#-industrial-iot--sensor-intelligence)
- [Predictive Maintenance & Anomaly Detection](#-predictive-maintenance--anomaly-detection)
- [Datasets](#-datasets)
- [Robotics & Embodied AI](#-robotics--embodied-ai)
- [Vision-Language-Action (VLA) Models](#-vision-language-action-vla-models)
- [Computer Vision for Manufacturing](#-computer-vision-for-manufacturing)
- [Digital Twins & Simulation](#-digital-twins--simulation)
- [Edge Kubernetes & Infrastructure](#-edge-kubernetes--infrastructure)
- [Reference Architectures](#-reference-architectures)
- [Learning Resources](#-learning-resources)
- [Community](#-community)

---

## 🧠 Foundation Models for Physical Data

*Foundation models purpose-built for time-series, sensor data, and physical-world understanding.*

| Project | Description |
|---------|-------------|
| [MOMENT](https://github.com/moment-timeseries-foundation-model/moment) ![GitHub stars](https://img.shields.io/github/stars/moment-timeseries-foundation-model/moment?style=flat-square) | A family of open-source foundation models for general-purpose time-series analysis (CMU). |
| [Chronos](https://github.com/amazon-science/chronos-forecasting) ![GitHub stars](https://img.shields.io/github/stars/amazon-science/chronos-forecasting?style=flat-square) | Pretrained probabilistic time-series forecasting models based on language model architectures (Amazon). |
| [MOIRAI](https://github.com/SalesforceAIResearch/uni2ts) ![GitHub stars](https://img.shields.io/github/stars/SalesforceAIResearch/uni2ts?style=flat-square) | Universal time-series forecasting transformer — Masked Encoder-based Universal Time Series Forecasting (Salesforce). |
| [TimesFM](https://github.com/google-research/timesfm) ![GitHub stars](https://img.shields.io/github/stars/google-research/timesfm?style=flat-square) | Time Series Foundation Model for time-series forecasting (Google Research). |
| [Lag-Llama](https://github.com/time-series-foundation-models/lag-llama) ![GitHub stars](https://img.shields.io/github/stars/time-series-foundation-models/lag-llama?style=flat-square) | Towards foundation models for probabilistic time-series forecasting using LLaMA-inspired architecture. |
| [NVIDIA Cosmos](https://github.com/NVIDIA/Cosmos) ![GitHub stars](https://img.shields.io/github/stars/NVIDIA/Cosmos?style=flat-square) | World foundation model platform for generating physics-aware videos and world states for Physical AI. |
| [Newton](https://www.archetypeai.io/) | Foundation model for understanding the physical world from sensor data (Archetype AI — commercial). |

## ⚡ Edge AI Frameworks & Platforms

*Frameworks and platforms for deploying and running AI models at the edge.*

| Project | Description |
|---------|-------------|
| [Azure IoT Operations](https://learn.microsoft.com/azure/iot-operations/) | Microsoft's edge-native IoT platform — unified data plane, MQTT broker, and edge-native operations on Arc-enabled Kubernetes. |
| [NVIDIA Isaac](https://developer.nvidia.com/isaac) | End-to-end robotics platform with GPU-accelerated libraries, AI models, and simulation tools. |
| [NVIDIA PhysicsNeMo](https://github.com/NVIDIA/physicsnemo) ![GitHub stars](https://img.shields.io/github/stars/NVIDIA/physicsnemo?style=flat-square) | Open-source framework for building physics-informed machine learning models (formerly Modulus). |
| [KubeEdge](https://github.com/kubeedge/kubeedge) ![GitHub stars](https://img.shields.io/github/stars/kubeedge/kubeedge?style=flat-square) | Kubernetes-native edge computing framework extending cloud-native capabilities to the edge. |
| [OpenVINO](https://github.com/openvinotoolkit/openvino) ![GitHub stars](https://img.shields.io/github/stars/openvinotoolkit/openvino?style=flat-square) | Open-source toolkit for optimizing and deploying AI inference on Intel hardware at the edge. |
| [ONNX Runtime](https://github.com/microsoft/onnxruntime) ![GitHub stars](https://img.shields.io/github/stars/microsoft/onnxruntime?style=flat-square) | Cross-platform, high-performance ML inference and training accelerator. |
| [Triton Inference Server](https://github.com/triton-inference-server/server) ![GitHub stars](https://img.shields.io/github/stars/triton-inference-server/server?style=flat-square) | Open-source inference serving software for any AI model on any GPU or CPU (NVIDIA). |
| [TensorRT](https://github.com/NVIDIA/TensorRT) ![GitHub stars](https://img.shields.io/github/stars/NVIDIA/TensorRT?style=flat-square) | High-performance deep learning inference optimizer and runtime for NVIDIA GPUs. |
| [Apache TVM](https://github.com/apache/tvm) ![GitHub stars](https://img.shields.io/github/stars/apache/tvm?style=flat-square) | Open deep learning compiler stack for CPUs, GPUs, and specialized accelerators. |
| [TensorFlow Lite](https://github.com/tensorflow/tensorflow) ![GitHub stars](https://img.shields.io/github/stars/tensorflow/tensorflow?style=flat-square) | Lightweight solution for deploying ML models on mobile and embedded edge devices. |
| [ExecuTorch](https://github.com/pytorch/executorch) ![GitHub stars](https://img.shields.io/github/stars/pytorch/executorch?style=flat-square) | PyTorch platform for on-device AI across mobile and edge devices. |

## 📡 Industrial IoT & Sensor Intelligence

*Protocols, brokers, and data infrastructure for connecting sensors to AI.*

| Project | Description |
|---------|-------------|
| [OPC UA](https://opcfoundation.org/) | The industrial interoperability standard — machine-to-machine communication protocol for industrial automation. |
| [MQTT](https://mqtt.org/) | Lightweight, publish-subscribe messaging protocol designed for constrained devices and low-bandwidth networks. |
| [Eclipse Mosquitto](https://github.com/eclipse-mosquitto/mosquitto) ![GitHub stars](https://img.shields.io/github/stars/eclipse-mosquitto/mosquitto?style=flat-square) | Open-source MQTT broker implementing MQTT protocol versions 5.0, 3.1.1, and 3.1. |
| [NanoMQ](https://github.com/nanomq/nanomq) ![GitHub stars](https://img.shields.io/github/stars/nanomq/nanomq?style=flat-square) | Ultra-lightweight and blazing-fast MQTT broker for IoT edge devices. |
| [Node-RED](https://github.com/node-red/node-red) ![GitHub stars](https://img.shields.io/github/stars/node-red/node-red?style=flat-square) | Low-code, flow-based programming tool for wiring together IoT devices, APIs, and services. |
| [Apache Kafka](https://github.com/apache/kafka) ![GitHub stars](https://img.shields.io/github/stars/apache/kafka?style=flat-square) | Distributed event streaming platform for high-throughput, real-time data pipelines. |
| [InfluxDB](https://github.com/influxdata/influxdb) ![GitHub stars](https://img.shields.io/github/stars/influxdata/influxdb?style=flat-square) | Purpose-built time-series database for high-write and high-query workloads from sensors and IoT. |
| [TimescaleDB](https://github.com/timescale/timescaledb) ![GitHub stars](https://img.shields.io/github/stars/timescale/timescaledb?style=flat-square) | Open-source time-series SQL database optimized for fast ingest and complex queries on PostgreSQL. |
| [QuestDB](https://github.com/questdb/questdb) ![GitHub stars](https://img.shields.io/github/stars/questdb/questdb?style=flat-square) | High-performance time-series database with SQL and InfluxDB line protocol support. |
| [Apache PLC4X](https://github.com/apache/plc4x) ![GitHub stars](https://img.shields.io/github/stars/apache/plc4x?style=flat-square) | Universal protocol adapter for communicating with industrial PLCs using a unified API. |

## 🔧 Predictive Maintenance & Anomaly Detection

*Tools and libraries for detecting equipment failures, anomalies, and degradation patterns.*

| Project | Description |
|---------|-------------|
| [PyOD](https://github.com/yzhao062/pyod) ![GitHub stars](https://img.shields.io/github/stars/yzhao062/pyod?style=flat-square) | Comprehensive Python library for detecting anomalies in multivariate data with 50+ algorithms. |
| [ADTK](https://github.com/arundo/adtk) ![GitHub stars](https://img.shields.io/github/stars/arundo/adtk?style=flat-square) | Anomaly Detection Toolkit — unsupervised/rule-based time-series anomaly detection. |
| [Alibi Detect](https://github.com/SeldonIO/alibi-detect) ![GitHub stars](https://img.shields.io/github/stars/SeldonIO/alibi-detect?style=flat-square) | Algorithms for outlier, adversarial, and drift detection across tabular, text, image, and time-series data. |
| [Luminaire](https://github.com/zillow/luminaire) ![GitHub stars](https://img.shields.io/github/stars/zillow/luminaire?style=flat-square) | ML-powered time-series anomaly detection framework (Zillow/LinkedIn). |
| [DeepXDE](https://github.com/lululxvi/deepxde) ![GitHub stars](https://img.shields.io/github/stars/lululxvi/deepxde?style=flat-square) | Library for physics-informed neural networks (PINNs) and deep operator learning. |
| [Merlion](https://github.com/salesforce/Merlion) ![GitHub stars](https://img.shields.io/github/stars/salesforce/Merlion?style=flat-square) | ML library for time-series intelligence — forecasting, anomaly detection, and change point detection (Salesforce). |
| [TODS](https://github.com/datamllab/tods) ![GitHub stars](https://img.shields.io/github/stars/datamllab/tods?style=flat-square) | Automated Time-series Outlier Detection System with end-to-end pipeline support. |
| [tslearn](https://github.com/tslearn-team/tslearn) ![GitHub stars](https://img.shields.io/github/stars/tslearn-team/tslearn?style=flat-square) | Machine learning toolkit for time-series data — clustering, classification, and preprocessing. |

## 📊 Datasets

*Benchmark datasets for predictive maintenance, fault detection, and industrial process monitoring.*

| Dataset | Description |
|---------|-------------|
| [NASA C-MAPSS](https://data.nasa.gov/Aerospace/CMAPSS-Jet-Engine-Simulated-Data/ff5v-kuh6) | Turbofan engine degradation simulation — the gold standard for remaining useful life (RUL) prediction. |
| [CWRU Bearing Dataset](https://engineering.case.edu/bearingdatacenter) | Vibration data from bearings with seeded faults — widely used for fault diagnosis research. |
| [SECOM Semiconductor](https://archive.ics.uci.edu/dataset/179/secom) | Semiconductor manufacturing process data with pass/fail labels from 590 sensors. |
| [Tennessee Eastman Process](https://github.com/camaramm/tennessee-eastman-profBraworskiri) | Chemical process simulation with 20 fault types — classic benchmark for process monitoring. |
| [PHM Society Datasets](https://phmsociety.org/phm-datasets/) | Collection of prognostics and health management datasets from annual PHM conferences. |
| [Kaggle Predictive Maintenance](https://www.kaggle.com/datasets/shivamb/machine-predictive-maintenance-classification) | Synthetic dataset reflecting real predictive maintenance patterns with machine failure modes. |
| [MIMII Dataset](https://zenodo.org/records/3384388) | Malfunctioning Industrial Machine Investigation and Inspection — sound-based anomaly detection. |
| [Bosch Production Line](https://www.kaggle.com/c/bosch-production-line-performance) | Manufacturing line data for predicting internal failures from thousands of measurements. |
| [Steel Plates Faults](https://archive.ics.uci.edu/dataset/198/steel+plates+faults) | Classification of surface defects in stainless steel plates (7 fault types). |

## 🤖 Robotics & Embodied AI

*Foundation models and platforms for robots that operate in the physical world.*

| Project | Description |
|---------|-------------|
| [NVIDIA GR00T](https://developer.nvidia.com/isaac/groot) | Foundation model for humanoid robots — multimodal understanding and action generation. |
| [LeRobot](https://github.com/huggingface/lerobot) ![GitHub stars](https://img.shields.io/github/stars/huggingface/lerobot?style=flat-square) | State-of-the-art machine learning for real-world robotics (Hugging Face). |
| [Octo](https://github.com/octo-models/octo) ![GitHub stars](https://img.shields.io/github/stars/octo-models/octo?style=flat-square) | Open-source generalist robot policy trained on 800K+ trajectories (UC Berkeley). |
| [ROS 2](https://github.com/ros2/ros2) ![GitHub stars](https://img.shields.io/github/stars/ros2/ros2?style=flat-square) | The Robot Operating System — middleware for robot software development. |
| [MoveIt](https://github.com/moveit/moveit2) ![GitHub stars](https://img.shields.io/github/stars/moveit/moveit2?style=flat-square) | Motion planning framework for robotic manipulation in ROS 2. |
| [Open X-Embodiment](https://robotics-transformer-x.github.io/) | Large-scale dataset and models from 22 robot embodiments for generalist robot policies (Google DeepMind). |
| [NVIDIA Isaac Lab](https://github.com/isaac-sim/IsaacLab) ![GitHub stars](https://img.shields.io/github/stars/isaac-sim/IsaacLab?style=flat-square) | Modular framework for robot learning built on NVIDIA Isaac Sim and Omniverse. |

## 🎯 Vision-Language-Action (VLA) Models

*Models that bridge vision, language understanding, and physical action for robotics.*

| Project | Description |
|---------|-------------|
| [RT-2](https://robotics-transformer2.github.io/) | Robotics Transformer 2 — vision-language-action model that transfers web knowledge to robotic control (Google DeepMind). |
| [OpenVLA](https://github.com/openvla/openvla) ![GitHub stars](https://img.shields.io/github/stars/openvla/openvla?style=flat-square) | Open-source vision-language-action model for generalist robot manipulation (Stanford). |
| [pi0](https://www.physicalintelligence.company/blog/pi0) | General-purpose robot foundation model for dexterous tasks (Physical Intelligence). |
| [Octo](https://github.com/octo-models/octo) ![GitHub stars](https://img.shields.io/github/stars/octo-models/octo?style=flat-square) | Generalist robot policy with language-conditioned and goal-conditioned action prediction. |
| [SayCan](https://say-can.github.io/) | Grounding large language models in robotic affordances for real-world task execution (Google). |
| [LLARVA](https://github.com/Dantong88/LLARVA) ![GitHub stars](https://img.shields.io/github/stars/Dantong88/LLARVA?style=flat-square) | Vision-action instruction tuning for robotic manipulation via large language models. |

## 👁️ Computer Vision for Manufacturing

*Visual inspection, defect detection, and video analytics for industrial environments.*

| Project | Description |
|---------|-------------|
| [Roboflow](https://roboflow.com/) | End-to-end platform for building, deploying, and managing computer vision models for inspection. |
| [LandingAI](https://landing.ai/) | Visual inspection platform purpose-built for manufacturing quality control (Andrew Ng). |
| [NVIDIA Metropolis](https://developer.nvidia.com/metropolis) | Application framework for building GPU-accelerated video analytics and IoT solutions. |
| [Ultralytics YOLOv8](https://github.com/ultralytics/ultralytics) ![GitHub stars](https://img.shields.io/github/stars/ultralytics/ultralytics?style=flat-square) | State-of-the-art object detection, segmentation, and classification for real-time inspection. |
| [Anomalib](https://github.com/openvinotoolkit/anomalib) ![GitHub stars](https://img.shields.io/github/stars/openvinotoolkit/anomalib?style=flat-square) | Deep learning library for anomaly detection in images — ideal for visual defect detection (Intel). |
| [MVTec AD](https://www.mvtec.com/company/research/datasets/mvtec-ad) | Benchmark dataset for unsupervised anomaly detection in industrial textures and objects. |
| [Grounding DINO](https://github.com/IDEA-Research/GroundingDINO) ![GitHub stars](https://img.shields.io/github/stars/IDEA-Research/GroundingDINO?style=flat-square) | Open-set object detection with language grounding — detect anything with text prompts. |

## 🌐 Digital Twins & Simulation

*Platforms for creating virtual replicas of physical systems and simulating industrial environments.*

| Project | Description |
|---------|-------------|
| [NVIDIA Omniverse](https://developer.nvidia.com/omniverse) | Platform for building and operating 3D simulation and digital twin applications. |
| [Azure Digital Twins](https://learn.microsoft.com/azure/digital-twins/) | IoT platform for creating digital models of real-world environments with live data connections. |
| [Gazebo](https://github.com/gazebosim/gz-sim) ![GitHub stars](https://img.shields.io/github/stars/gazebosim/gz-sim?style=flat-square) | Open-source 3D robot simulation with accurate physics, rendering, and sensor models. |
| [NVIDIA Isaac Sim](https://developer.nvidia.com/isaac/sim) | Robotics simulation platform built on Omniverse for training and testing robots in digital twins. |
| [MuJoCo](https://github.com/google-deepmind/mujoco) ![GitHub stars](https://img.shields.io/github/stars/google-deepmind/mujoco?style=flat-square) | Fast and accurate physics engine for research and development in robotics and biomechanics (Google DeepMind). |
| [PyBullet](https://github.com/bulletphysics/bullet3) ![GitHub stars](https://img.shields.io/github/stars/bulletphysics/bullet3?style=flat-square) | Physics simulation for robotics, games, visual effects, and machine learning. |
| [Open3D](https://github.com/isl-org/Open3D) ![GitHub stars](https://img.shields.io/github/stars/isl-org/Open3D?style=flat-square) | Library for 3D data processing — point clouds, meshes, and RGBD images for digital twin construction. |

## ☸️ Edge Kubernetes & Infrastructure

*Lightweight Kubernetes distributions and infrastructure for running AI workloads at the edge.*

| Project | Description |
|---------|-------------|
| [K3s](https://github.com/k3s-io/k3s) ![GitHub stars](https://img.shields.io/github/stars/k3s-io/k3s?style=flat-square) | Lightweight, certified Kubernetes distribution built for IoT and edge computing (Rancher/SUSE). |
| [Azure Arc](https://learn.microsoft.com/azure/azure-arc/) | Hybrid and multi-cloud management platform — extend Azure services to any infrastructure. |
| [AKS Edge Essentials](https://learn.microsoft.com/azure/aks/hybrid/aks-edge-overview) | Microsoft's lightweight Kubernetes for edge devices with Azure Arc integration. |
| [MicroK8s](https://github.com/canonical/microk8s) ![GitHub stars](https://img.shields.io/github/stars/canonical/microk8s?style=flat-square) | Low-ops, minimal-footprint Kubernetes for developers, IoT, and edge (Canonical). |
| [KubeEdge](https://github.com/kubeedge/kubeedge) ![GitHub stars](https://img.shields.io/github/stars/kubeedge/kubeedge?style=flat-square) | Extend native Kubernetes capabilities to edge nodes with edge-cloud syncing. |
| [k0s](https://github.com/k0sproject/k0s) ![GitHub stars](https://img.shields.io/github/stars/k0sproject/k0s?style=flat-square) | Zero-friction Kubernetes — single binary, no host OS dependencies. |
| [EdgeX Foundry](https://github.com/edgexfoundry/edgex-go) ![GitHub stars](https://img.shields.io/github/stars/edgexfoundry/edgex-go?style=flat-square) | Vendor-neutral open framework for IoT edge computing hosted by the Linux Foundation. |

## 🏗️ Reference Architectures

*End-to-end examples and reference implementations for deploying AI at the industrial edge.*

| Project | Description |
|---------|-------------|
| [aio-sensor-intelligence](https://github.com/udtri/aio-sensor-intelligence) | Deploy time-series foundation models for sensor anomaly detection with Azure IoT Operations. |
| [Azure IoT Operations Samples](https://github.com/Azure-Samples/explore-iot-operations) ![GitHub stars](https://img.shields.io/github/stars/Azure-Samples/explore-iot-operations?style=flat-square) | Official samples and quickstarts for Azure IoT Operations. |
| [AWS IoT Greengrass](https://github.com/aws-greengrass) | Deploy and run ML models on edge devices with AWS IoT Greengrass components. |
| [Eclipse IoT](https://iot.eclipse.org/) | Open-source IoT projects ecosystem — protocols, gateways, and device management frameworks. |
| [Industrial Edge by Siemens](https://github.com/industrial-edge) | Reference apps and examples for Siemens Industrial Edge platform. |

## 📚 Learning Resources

### Papers

| Paper | Description |
|-------|-------------|
| [A Survey on Edge Intelligence](https://arxiv.org/abs/1905.10083) | Comprehensive survey on the convergence of AI and edge computing. |
| [TinyML: Machine Learning with TensorFlow Lite](https://arxiv.org/abs/2010.08678) | Overview of ML deployment on microcontrollers and resource-constrained devices. |
| [Physics-Informed Neural Networks](https://www.sciencedirect.com/science/article/pii/S0021999118307125) | Raissi et al. — seminal paper on PINNs for solving forward and inverse problems. |
| [MOMENT: A Family of Open Time-Series Foundation Models](https://arxiv.org/abs/2402.03885) | Foundation model for time-series trained on diverse public data (CMU). |
| [RT-2: Vision-Language-Action Models](https://arxiv.org/abs/2307.15818) | Transfer web knowledge to robotic control via vision-language-action models (Google DeepMind). |
| [Digital Twin: Values, Challenges and Enablers](https://ieeexplore.ieee.org/document/9103025) | IEEE survey on digital twin technology for Industry 4.0. |

### Courses

| Course | Description |
|--------|-------------|
| [TinyML Specialization (edX/Harvard)](https://www.edx.org/certificates/professional-certificate/harvardx-tiny-machine-learning) | Three-course professional certificate on deploying ML on embedded systems. |
| [Introduction to IoT (Cisco)](https://www.netacad.com/courses/iot/introduction-iot) | Foundational course on IoT concepts, networking, and edge intelligence. |
| [Edge AI and Vision (Coursera)](https://www.coursera.org/) | Courses on deploying computer vision models at the edge. |
| [NVIDIA Deep Learning Institute](https://www.nvidia.com/en-us/training/) | Hands-on training for edge AI, robotics, and GPU-accelerated inference. |

### Blogs & Publications

| Resource | Description |
|----------|-------------|
| [NVIDIA Technical Blog](https://developer.nvidia.com/blog/) | Deep dives on edge AI, robotics, Omniverse, and Physical AI. |
| [Azure IoT Blog](https://techcommunity.microsoft.com/category/azure-iot/blog/internet-of-things-blog) | Updates on Azure IoT Operations, digital twins, and edge computing. |
| [The Gradient](https://thegradient.pub/) | In-depth perspectives on machine learning research and applications. |
| [Towards Data Science — IoT/Edge](https://towardsdatascience.com/) | Community articles on deploying ML at the edge. |

### YouTube Channels

| Channel | Description |
|---------|-------------|
| [NVIDIA Developer](https://www.youtube.com/@NVIDIADeveloper) | GTC talks, tutorials on Isaac, Omniverse, and Physical AI. |
| [Edge Impulse](https://www.youtube.com/@EdgeImpulse) | TinyML and edge AI deployment tutorials and demos. |
| [Andreas Spiess](https://www.youtube.com/@AndreasSpiess) | IoT and sensor projects — practical edge computing tutorials. |

## 🤝 Community

### Conferences

| Conference | Description |
|------------|-------------|
| [Sensors Converge](https://www.sensorsconverge.com/) | Leading event for sensor innovation and IoT — Silicon Valley. |
| [IoT World](https://tmt.knect365.com/iot-world/) | Global conference on IoT strategy, technology, and implementation. |
| [NVIDIA GTC](https://www.nvidia.com/gtc/) | GPU Technology Conference — Physical AI, robotics, digital twins. |
| [Embedded World](https://www.embedded-world.de/en) | Largest embedded systems conference — Nuremberg, Germany. |
| [ROSCon](https://roscon.ros.org/) | Annual conference for the Robot Operating System (ROS) community. |

### Forums & Communities

| Community | Description |
|-----------|-------------|
| [r/embedded](https://www.reddit.com/r/embedded/) | Reddit community for embedded systems and edge computing. |
| [r/robotics](https://www.reddit.com/r/robotics/) | Reddit community for robotics enthusiasts and researchers. |
| [ROS Discourse](https://discourse.ros.org/) | Official discussion forum for the ROS community. |
| [Edge AI Foundation](https://www.edgeai.foundation/) | Industry consortium advancing edge AI interoperability and standards. |
| [KubeEdge Slack](https://kubeedge.io/docs/community/slack/) | Community channel for KubeEdge development and deployment. |

---

## Contributing

Contributions welcome! Read the [contribution guidelines](CONTRIBUTING.md) first.

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the contributors have waived all copyright and related or neighboring rights to this work. See [LICENSE](LICENSE) for details.
