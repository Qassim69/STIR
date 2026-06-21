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

## 📈 Training Details

| Detail | Value |
|---|---|
| Base Model | `smolvla_base` |
| Datasets Used | 40+ robotics datasets |
| Fine-tuning Time | ~6–7 hours |
| Hardware | SO-101 Follower Arm + webcam |
| Tasks Covered | Stirring coffee & tea with sugar, honey, milk, and other condiments |

---

## 🤝 Contributing

Want to extend STIR to dip biscuits 🍪, froth milk 🥛, or add a splash of oat milk with artistic precision? Open a PR — let's build the ultimate robotic beverage station together.

---

*Built with caffeine, curiosity, and a mild obsession with not interrupting flow states.* ☕💻
