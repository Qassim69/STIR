# ☕ STIR — The AI-Powered Robotic Coffee & Tea Stirrer 🤖
### *Because stirring your own coffee is so last decade*

[![Framework: LeRobot](https://img.shields.io/badge/Robotics-LeRobot-blueviolet?style=flat-square&logo=huggingface)](https://huggingface.co/docs/lerobot/installation)
[![Policy: SmolVLA](https://img.shields.io/badge/Brain-SmolVLA-orange?style=flat-square)](https://huggingface.co/docs/lerobot/smolvla)
[![Inference: RTC](https://img.shields.io/badge/Inference-Real--Time%20Chunking-green?style=flat-square)](https://huggingface.co/docs/lerobot/rtc)
[![Hardware: SO-101](https://img.shields.io/badge/Hardware-SO--101-yellow?style=flat-square)](https://huggingface.co/docs/lerobot/installation)

---

## 💡 The Problem

It's 3 AM. You're deep in a flow state. The code is *finally* making sense. You pour yourself a hot tea, drop in two sugars, and then... you stand there. Spoon in hand. Stirring.

**Enter STIR.** 🌀

Just hand the robot your mug, tell it how many spoons of sugar you need, and go back to doing what matters. STIR handles the vortex. You handle the code.

---

## 🛠️ The Tech Stack

STIR isn't just a motor glued to a spoon. It's a full Vision-Language-Action (VLA) pipeline running on a real robotic arm. Here's what's under the hood:

### 🦾 The Muscle: LeRobot SO-101

We use the **SO-101 Robot Arm** from [LeRobot by Hugging Face](https://huggingface.co/docs/lerobot/installation) — a capable, low-latency servo-driven arm with the fine motor control needed to grip a spoon and stir a mug without recreating the Titanic disaster on your desk.

### 🧠 The Brain: SmolVLA

The decision-making is powered by **[SmolVLA](https://huggingface.co/docs/lerobot/smolvla)**, Hugging Face's compact Vision-Language-Action model.

- **Pre-training**: Trained across **40+ robotics datasets** to build a solid understanding of spatial reasoning and object manipulation
- **Fine-tuning**: We ran custom training for **6–7 hours** on stirring demonstrations.
- **Multimodal Inputs**: SmolVLA takes in:
  - 📷 Camera feed (watching the mug and spoon)
  - 🦿 Robot joint states (knowing where the arm is)

### ⚡ The Secret Sauce: Real-Time Chunking (RTC)

Raw VLA inference is slow. A robot waiting on its brain between every movement looks like it's having an existential crisis.

We solve this with **[Real-Time Chunking (RTC)](https://huggingface.co/docs/lerobot/rtc)**. Instead of computing one action at a time, SmolVLA predicts a whole **chunk of actions** at once. While the robot executes the current chunk, RTC silently prepares the next one in the background — blending the transition seamlessly so movements are fluid, continuous, and spill-free. 🥄✨

No stuttering. No pausing. Just smooth, hypnotic stirring.

---

## 🚀 Getting Started

> **Note:** You are not cloning this repository to run STIR — you are setting up the **LeRobot framework**, collecting your own demonstrations, training a SmolVLA policy, and deploying it on your SO-101. This repo documents our approach and serves as a reference. Follow the steps below in order.

---

### 🖥️ Step 1 — Set Up Your Environment (WSL)

STIR was developed and tested on **Windows Subsystem for Linux (WSL)**. Start by setting up LeRobot inside a WSL environment by following the official installation guide:

👉 [LeRobot Installation Guide](https://huggingface.co/docs/lerobot/installation)

This covers:
- Setting up WSL and a compatible Python environment
- Cloning the **LeRobot repository** (not this one)
- Installing all core dependencies and hardware drivers for the SO-101 arm

```bash
# Clone LeRobot's official repository
git clone https://github.com/huggingface/lerobot.git
cd lerobot

# Install with SmolVLA support
pip install -e ".[smolvla]"
```

---

### 🎓 Step 2 — Collect Data & Train via Imitation Learning

Once your environment and SO-101 arm are configured, the next step is to **collect demonstration episodes** and train the SmolVLA policy using Imitation Learning.

👉 [Imitation Learning with LeRobot](https://huggingface.co/docs/lerobot/il_robots)

The workflow looks like this:

1. **Teleoperate** the SO-101 leader arm to physically demonstrate the stirring task across multiple episodes (different mug positions, sugar quantities, condiments)
2. **Record the dataset** — LeRobot captures camera frames, joint states, and actions automatically
3. **Push the dataset** to Hugging Face Hub for training
4. **Train the SmolVLA policy** using the collected data

#### 💪 Training Compute Options

You have two options for the training step:

- **Local GPU** — If you have a powerful NVIDIA GPU (e.g. RTX 3090 / 4090), you can train directly on your machine. Expect ~6–7 hours for a well-converged stirring policy.
- **Hugging Face Cloud** ☁️ — No beefy GPU? No problem. You can push your dataset to the Hub and use [Hugging Face's ZeroGPU / training servers](https://huggingface.co) to access stronger compute without burning your electricity bill.

---

### 🤖 Step 3 — Deploy the Policy on the SO-101

Once training is complete and your model is available (locally or on the Hub), deploy it on the robot using `lerobot-rollout` with RTC inference:

```bash
lerobot-rollout --strategy.type=base \
  --policy.path=dd-template/smolvla_spoon_stir \
  --inference.type=rtc \
  --inference.rtc.execution_horizon=16 \
  --inference.rtc.max_guidance_weight=10.0 \
  --interpolation_multiplier=2 \
  --robot.type=so101_follower --robot.port=/dev/tty.usbmodemXXXX \
  --robot.cameras="{ camera1: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 30}}" \
  --task="Stir the cup" --use_torch_compile=true --duration=60
```

> 🔌 Replace `/dev/tty.usbmodemXXXX` with the actual serial port of your connected SO-101 arm.  
> 📷 The camera is configured at **1920×1080 @ 30fps** for high-fidelity visual input.  
> ⏱️ Each rollout runs for **60 seconds** — more than enough time for a perfectly stirred cup.

---

## 📈 Training Details

| Detail | Value |
|---|---|
| Base Model | `smolvla_base` |
| Datasets Used | 40+ robotics datasets |
| Fine-tuning Time | ~6–7 hours |
| Hardware | SO-101 Follower Arm + webcam |
| Tasks Covered | Stirring coffee & tea with sugar |

---

## 🤝 Contributing

Want to extend STIR to dip biscuits 🍪, froth milk 🥛, or add a splash of oat milk with artistic precision? Open a PR — let's build the ultimate robotic beverage station together.

---

*Built with caffeine, curiosity, and a mild obsession with not interrupting flow states.* ☕💻
