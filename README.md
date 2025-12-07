# 🕷️ Aljazari: Autonomous Rescue Hexapod Robot

![Project Status](https://img.shields.io/badge/Status-Active_Development-brightgreen)
![Platform](https://img.shields.io/badge/Platform-Raspberry_Pi_5_%7C_SeedXiaoESP32S3-blue)
![AI Accelerator](https://img.shields.io/badge/AI-Hailo_AI_8-violet)
![Docker](https://img.shields.io/badge/Container-Docker-2496ED?logo=docker&logoColor=white)

> **"Bridging the gap between Inverse Kinematics and Edge AI for Search and Rescue (SAR) missions."**

<div align="center">
  <img src="https://via.placeholder.com/800x400?text=Aljazari+Robot+Prototype+Image" alt="Aljazari Robot" width="100%">
</div>

---

## 📖 Table of Contents
- [Overview](#-overview)
- [Key Features](#-key-features)
- [System Architecture](#-system-architecture)
- [Tech Stack](#-tech-stack)
- [Getting Started](#-getting-started)
- [Hardware Design](#-hardware-design)
- [Roadmap](#-roadmap)
- [License](#-license)

---

## 🧐 Overview
**Aljazari** is a research-grade hexapod robot designed to navigate uneven terrain where wheeled robots fail. The project aims to develop a low-cost, high-performance rescue assistant capable of identifying victims in disaster zones.

Currently, the system is undergoing a major hardware migration from **NVIDIA Jetson Nano** to a **Raspberry Pi 5 + Hailo-8 AI Accelerator** architecture to improve power efficiency and inference speed.

## ✨ Key Features
* **Adaptive Locomotion:** 18-DOF (Degrees of Freedom) utilizing Inverse Kinematics for Tripod and Wave gaits.
* **Edge AI Vision:** Real-time object detection (human/survivor) using **YOLOv8** optimized for Hailo-8 NPU (HEF format).
* **Containerized Environment:** Fully reproducible software stack using **Docker** on WSL 2 / Linux.
* **IoT Monitoring:** Wireless telemetry for battery voltage and system temperature.

---

## ⚙️ System Architecture

The robot operates on a dual-controller architecture:

| Component | Function | Communication |
| :--- | :--- | :--- |
| **High-Level (Brain)** | Raspberry Pi 5 (8GB) + Hailo-8 | Runs OS, Docker, AI Vision, Path Planning. |
| **Low-Level (Limb)** | SeedXiao ESP32S Series | Inverse Kinematics, Servo PWM Control, IMU Feedback. |
| **Protocol** | Data Exchange | UART / Serial Communication (115200 baud). |

---

## 💻 Tech Stack

### Software
* **Languages:** Python 3.9 (Vision), C/C++ (SeedXiao ESP32S3 Firmware).
* **AI Frameworks:** Ultralytics YOLOv8, Hailo Dataflow Compiler.
* **Deployment:** Docker, Docker Compose.
* **OS:** Raspberry Pi OS (Bookworm), Windows 11 (Development/Sim).

### Hardware
* **Actuators:** 18x High-Torque Metal Gear Servos.
* **Power:** 3S LiPo Battery with Custom Buck Converter (5V/8A for servos, 5V/5A for RPi).
* **Chassis:** Custom 3D Printed Parts (PLA+/PETG) designed in **SolidWorks**.

---

## 🚀 Getting Started

### Prerequisites
* Docker Desktop (with WSL 2 enabled) or Native Linux.
* Hailo Runtime Software (if running on RPi 5).

### Installation
1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/aljazari-hexapod.git](https://github.com/your-username/aljazari-hexapod.git)
    cd aljazari-hexapod
    ```

2.  **Set up the Docker Environment:**
    We provide a pre-configured Dockerfile for the vision system.
    ```bash
    cd 03_Software/docker
    docker compose up -d --build
    ```

3.  **Enter the Development Container:**
    ```bash
    docker exec -it aljazari_vision_1 /bin/bash
    ```

### Model Conversion (YOLOv8 to HEF)
To run the model on Hailo-8, convert your `.pt` file using our script:
```bash
python scripts/export_hailo.py --model models/yolov8n.pt --target hailo8
