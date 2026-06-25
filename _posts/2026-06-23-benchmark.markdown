---
layout: post
title: "YOLOv8 CPU Inference Benchmark: Which Engine Really Wins?"
date: 2026-03-31
img: AI_img.webp
categories: [portfolio, computer-vision, mlops]
tags: [YOLOv8, CPU-Inference, TFLite, ONNX, OpenVINO, PyTorch, Benchmarking, Model-Optimization]
---

> *"You've trained your model. Now, how do you run it fast — really fast — on a CPU, without a GPU in sight?"*

I built a rigorous benchmark to compare YOLOv8n inference across four different engines: **PyTorch, ONNX Runtime, OpenVINO, and TensorFlow Lite**. The results surprised me — and they might surprise you too.

*Documentation* : [Github](https://github.com/ihauss/benckmark-yolo-inference-on-CPU)

---

## The Problem: Choosing the Right Inference Engine

In production computer vision, the model is only half the story. The inference engine you choose can make the difference between a system that's responsive and one that's unusable — especially when you're deploying on **CPU-only hardware**.

Most developers default to PyTorch, the framework they trained with. But is it the fastest? What about ONNX? OpenVINO? TFLite?

**The Core Questions:**

- Which engine gives the **lowest latency** on CPU?
- Which one is **most stable** (lowest variance)?
- Does **OpenVINO** always win on Intel hardware?
- How do these engines perform in **constrained environments** (like virtualized cloud instances)?

---

## The Solution: A Rigorous, Reproducible Benchmark

> *"I designed a benchmark that controls for every variable — same model, same input size, same preprocessing, same post-processing — so the only difference is the inference engine."*

### Methodology

To ensure fairness and reproducibility, I applied a strict, consistent methodology:

| Factor | Specification |
|--------|---------------|
| **Model** | YOLOv8n (same weights, same export) |
| **Input size** | 640×640 pixels |
| **Preprocessing** | Identical pipeline across all engines |
| **Post-processing** | NMS (Non-Maximum Suppression) included in all exports |
| **Runs** | 30 inference iterations per engine |
| **Warmup** | Multiple warmup runs before measurement |
| **Metrics** | Latency (ms), FPS, Standard Deviation (stability) |

### Compared Engines

1. **PyTorch (Ultralytics)** — The baseline, using the native framework
2. **ONNX Runtime** — The open standard, optimized for cross-platform deployment
3. **OpenVINO** — Intel's proprietary optimization toolkit, designed for Intel CPUs
4. **TensorFlow Lite (TFLite)** — Google's lightweight runtime, typically used for mobile and edge

---

## Results: The Numbers Don't Lie

Here are the raw results from 30 runs per engine:

| Engine | Mean Time (s) | Std Dev | FPS | Latency (ms) |
|--------|---------------|---------|-----|--------------|
| **TFLite** | 0.1607 | 0.0051 | **6.22** | **160.71** |
| ONNX | 0.1997 | 0.0383 | 5.01 | 199.72 |
| PyTorch | 0.2214 | 0.0078 | 4.52 | 221.44 |
| OpenVINO | 0.2640 | 0.0901 | 3.79 | 264.02 |

**The Winner: TFLite** — delivering **6.22 FPS**, the lowest latency, and the **lowest standard deviation** (most stable performance).

---

## Key Insights: Why TFLite Won

> *"In a constrained CPU environment, lightweight runtimes like TFLite can outperform traditionally optimized frameworks like OpenVINO."*

### Why TFLite Came First

- **Lightweight design** — TFLite is built for resource-constrained devices, making it naturally efficient on limited CPU resources
- **Low overhead** — minimal runtime footprint compared to full-featured frameworks
- **Stable performance** — the lowest standard deviation (0.0051) means consistent, predictable inference

### Why ONNX Came Second

- **Strong balance** between speed and portability
- **Practical choice** for production systems where flexibility matters
- **Good enough** performance with broad hardware support

### Why PyTorch Was Slower

- **Dynamic execution model** introduces runtime overhead
- **Higher memory footprint** compared to exported formats
- Not optimized for pure inference — it's a training-first framework

### Why OpenVINO Underperformed

This was the biggest surprise. OpenVINO is Intel's flagship optimization toolkit, designed specifically for Intel CPUs. Yet it came last.

**The explanation**:

- **Limited CPU resources** — the benchmark ran on a single core / 2 threads
- **Virtualized environment** (KVM) — reduced efficiency of low-level optimizations
- **SIMD and threading optimizations** couldn't be fully leveraged in this constrained setup

**The takeaway**: OpenVINO's advanced optimizations require **real hardware** to shine. In virtualized or resource-constrained environments, they can actually become a liability.

---

## What This Means for Production

This benchmark answers a critical question for anyone deploying YOLO on CPU:

> *"Which inference engine provides the best performance on CPU under realistic conditions?"*

**For constrained or virtualized environments**:

- **Choose TFLite** — lightweight, stable, and consistently fast
- **ONNX is a solid second** — especially if you need broad compatibility
- **Avoid PyTorch** for pure inference — it's not built for this
- **Only use OpenVINO** if you have dedicated Intel hardware with full resource access

**For bare-metal Intel CPUs with full resources**:

- OpenVINO *may* outperform — but this benchmark shows it's not a guarantee
- Always benchmark in your own environment before committing

---

## Tech Stack

| Layer | Technologies |
|-------|--------------|
| **Model** | YOLOv8n (Ultralytics) |
| **Frameworks** | PyTorch, ONNX Runtime, OpenVINO, TensorFlow Lite |
| **Environment** | Google Colab (virtualized CPU) |
| **Metrics** | Custom timing harness with warmup and iterations |

---

## Key Lessons Learned

This project reinforced that **benchmarking is not optional** — assumptions about performance are dangerous.

1. **Don't assume OpenVINO always wins:** Even on Intel CPUs, virtualization and resource constraints can flip the rankings.
2. **TFLite is underrated for CPU:** Most people think of it for mobile, but it's a powerhouse on constrained x86 CPUs too.
3. **Stability matters as much as speed:** Low latency is useless if it's wildly inconsistent. TFLite's low std dev makes it the safest choice.
4. **Reproducibility is everything:** By controlling preprocessing, post-processing, and warmup, I ensured the results are meaningful — not just noise.

---

## Business Impact & Use Cases

This isn't just an academic exercise — it has real implications for production deployments:

- **Edge AI deployments:** Choose TFLite for CPU-only edge devices (Raspberry Pi, industrial PCs, etc.)
- **Cloud inference:** ONNX offers a great balance of speed and portability for cloud-based vision services
- **Cost optimization:** Faster inference means lower compute costs — TFLite's 6.22 FPS vs OpenVINO's 3.79 FPS is a **40% improvement**
- **Hardware planning:** Don't assume you need GPUs — with the right engine, CPU-only can be viable

**Why this matters for Asia:**

With the rapid growth of edge computing and smart city deployments across Singapore, Japan, and China, engineers are constantly choosing between inference engines. This benchmark provides **hard data** to inform those decisions — saving time, money, and compute resources.

---

## Conclusion

> *"This benchmark proved that in constrained CPU environments, the lightest engine wins — and that assumptions about 'optimized' frameworks can be dangerously wrong."*

This project demonstrates my ability to:

- **Design rigorous, reproducible benchmarks** for ML models
- **Compare and evaluate** multiple inference engines objectively
- **Extract actionable insights** from raw performance data
- **Think critically** about deployment constraints and trade-offs

---