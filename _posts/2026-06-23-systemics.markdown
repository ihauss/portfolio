---
layout: post
title: "Hydrogen Fuel Cell Control: Embedded C for a Le Mans World Record"
date: 2026-06-09
img: tuktuk.png
categories: [portfolio, embedded-systems, control-engineering]
tags: [Embedded-C, Bare-Metal, CAN-bus, I2C, Microchip-MCU, Safety-Critical, Fuel-Cell]
---

> *"A hydrogen-powered tuk-tuk. 24 hours. 800+ kilometers. One embedded C program keeping the fuel cell stack alive."*

I wrote the control software that stabilized a hydrogen fuel cell stack—enabling a light mobility vehicle to set a world record at Le Mans.

*Documentation* : [Github](https://github.com/ihauss/Systemics.Energy-Hydrogen-Fuel-Cell-Control-System)

*Official Company Vlog* : [Watch the Le Mans record attempt](https://www.youtube.com/watch?v=N4Ja7tXK798)

---

## The Problem: Keeping a Hydrogen Stack Alive

A hydrogen fuel cell stack is a delicate electrochemical system. It requires **precise, real‑time regulation** of multiple interdependent parameters to operate safely and efficiently:

- **Temperature** — too low kills efficiency; too high becomes dangerous
- **Hydrogen leaks** — H₂ is odourless and invisible; a leak can go unnoticed until it's too late
- **Voltage stability** — output must remain within tight bounds
- **Oxygen‑hydrogen mix** — the stack uses ambient air for both cooling *and* oxygen supply; that balance is critical

> ⚠️ On a small, resource‑constrained embedded system (Microchip MCU), there is no room for heavy OS abstractions or ML models — just fast, deterministic C code talking directly to sensors and actuators via CAN bus and I2C.

---

## The Solution: Bare-Metal Control, Two Phases

I joined the project as an **embedded systems developer**, taking ownership of the hydrogen stack control module in two distinct phases.

### Phase 1 — Codebase Rehabilitation

The existing codebase was functional but **undocumented, unversioned, and inconsistent in style** — a silent risk for any safety‑critical system.

**What I did**:

- Wrote **complete documentation** (module descriptions, function behaviours, expected I/O ranges)
- Added **inline comments** for non‑obvious logic and hardware interactions
- Introduced **Git version control** to track changes and enable collaboration
- Standardised the code to meet **professional embedded coding standards** (naming, error handling, static analysis readiness)

> *"This phase was about turning a fragile prototype into a maintainable, auditable codebase — essential before any further development could happen safely."*

### Phase 2 — System Rescaling for a Smaller Stack

The hydrogen and electronics engineers needed to **scale down the technology** for a smaller stack — different thermal mass, flow rates, and voltage curves. I worked directly alongside them to adapt the control logic:

- Re‑tuned **temperature regulation loops** for the smaller stack's faster thermal dynamics
- Adjusted **voltage stabilisation parameters** to maintain output quality under varying load
- Validated **leak detection thresholds** and fail‑safe responses
- Updated **air flow management** (since the same air serves both cooling and oxygen supply)

**The result:** a stable, production‑ready embedded system that doubled the vehicle's usable energy capacity while maintaining safety margins.

---

## How It Works — Control Overview

```
[Hydrogen Stack]
    │
    ├─ Temperature sensor → PWM fan control → [Cooling]
    ├─ Voltage sensor   → load balancing  → [Stable output]
    ├─ H₂ leak sensor   → emergency stop  → [Safety]
    ├─ O₂ sensor (ambient air) → flow regulation → [Mix control]
    └─ CAN/I2C bus → communication with vehicle main controller
```


**No RTOS** — bare‑metal C with careful interrupt prioritisation for deterministic timing.

---

## Tech Stack

| Layer | Technologies |
|-------|--------------|
| **Microcontroller** | Microchip MCU (embedded C) |
| **Communication** | CAN bus, I2C |
| **Low‑level I/O** | PWM, GPIO, ADC (sensor reading), timer interrupts |
| **Development** | Git, embedded C toolchain (Microchip IDE / compiler) |
| **Target vehicle** | Hydrogen‑powered tuk‑tuk (light mobility platform) |

---

## Results & Impact

**The World Record:**

- **800+ km** in 24 hours at Le Mans
- Powered by just **2 kg of hydrogen**
- The vehicle ran continuously, demonstrating the reliability of the control system under extreme conditions

**Engineering Outcomes:**

- ✅ **Doubled usable energy capacity** after system rescaling
- ✅ **Production‑ready codebase** with full documentation and version control
- ✅ **Safety‑critical validation** — leak detection, emergency stops, thermal management
- ✅ **Deterministic real‑time performance** on resource‑constrained hardware

---

## Key Lessons Learned

This project taught me that **in embedded systems, reliability is not a feature — it's the only thing that matters**.

1. **Documentation is safety:** On a safety‑critical system, undocumented code is a liability. Writing clear, auditable documentation isn't overhead — it's risk mitigation.
2. **Cross‑disciplinary collaboration matters:** Tuning control loops required working side‑by‑side with hydrogen and electronics engineers to understand the physical system, not just the code.
3. **Constraints breed creativity:** Bare‑metal C with no RTOS forces you to think deeply about interrupt prioritisation, timing, and memory — skills that translate to any embedded domain.
4. **Version control is non‑negotiable:** Even for embedded projects, Git enables collaboration, traceability, and rollback capabilities that are essential for production systems.

---

## Business Impact & Use Cases

This isn't just a prototype — it's a proven control system for **real‑world hydrogen mobility**:

- **Light Electric Vehicles:** The control logic can be adapted for any small‑scale hydrogen fuel cell vehicle — tuk‑tuks, delivery bots, last‑mile logistics.
- **Stationary Power Systems:** Backup power units and off‑grid generators using hydrogen fuel cells require the same type of regulation.
- **Safety‑Critical Industrial Controls:** The principles (redundant sensing, fail‑safe responses, deterministic timing) apply broadly to industrial automation.

**Why this matters for Asia:**

Singapore and other Asian markets are actively investing in hydrogen as a clean energy vector. A proven, lightweight control system that runs on cheap, off‑the‑shelf microcontrollers is exactly what's needed to accelerate adoption at scale.

---

## Conclusion

> *"Writing the control software for a hydrogen fuel cell stack that set a world record at Le Mans taught me that the best embedded systems are invisible — they just work, reliably, every single time, without fail."*

This project demonstrates my ability to:

- Write **deterministic, bare‑metal C** for safety‑critical applications
- **Rescue and professionalise** fragile legacy codebases
- **Collaborate across disciplines** (electrical, mechanical, hydrogen engineering)
- Deliver **production‑ready, auditable** embedded software under real‑world constraints

---