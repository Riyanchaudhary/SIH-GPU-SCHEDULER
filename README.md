# GPU Smart Scheduler
### An Attainment-Based GPU Cluster Scheduler Inspired by Microsoft Tiresias

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Scheduling](https://img.shields.io/badge/Scheduling-MLFQ-orange)
![Research](https://img.shields.io/badge/Inspired%20By-Microsoft%20Tiresias-purple)
![Status](https://img.shields.io/badge/Status-Development-yellow)

## Overview

GPU Smart Scheduler is an intelligent GPU cluster scheduling system inspired by Microsoft's Tiresias research. It aims to improve resource allocation efficiency in shared GPU environments by dynamically prioritizing deep learning workloads.

Traditional scheduling algorithms such as First-Come-First-Served (FCFS) can cause head-of-line blocking, where shorter jobs remain queued behind computationally intensive training workloads.

This project implements an attainment-based scheduling mechanism that prioritizes jobs according to the cumulative GPU compute time they have received, without requiring prior knowledge of their execution durations.

The primary objectives are to minimize Job Completion Time (JCT), improve GPU utilization, and maintain fairness across competing workloads.

## Key Features

- **Attainment-Based Scheduling:** Tracks cumulative GPU-seconds consumed by each job and prioritizes jobs with lower service attainment.
- **Multi-Level Feedback Queue (MLFQ):** Dynamically assigns jobs to priority levels based on accumulated GPU compute time.
- **Preemptive Scheduling:** Allows higher-priority jobs to interrupt lower-priority workloads when necessary.
- **Starvation Prevention:** Incorporates aging and fairness mechanisms to ensure long-running jobs receive computational resources.
- **GPU Resource Allocation:** Manages GPU availability and job resource requirements during scheduling decisions.
- **Workload Simulation:** Supports synthetic workloads with varying arrival times, GPU requirements, and execution durations.
- **Performance Evaluation:** Measures JCT, waiting time, GPU utilization, throughput, and preemption overhead.

## System Architecture

```text
         Job Submission
                |
                v
         Job Ingestion
                |
                v
        Job Queue Manager
                |
                v
       Attainment Tracker
                |
                v
       Priority Assignment
                |
                v
        MLFQ Scheduler
                |
                v
       GPU Resource Manager
                |
          +-----+-----+
          |           |
      GPU Free     GPU Busy
          |           |
          v           v
       Dispatch   Preemption
          |           |
          +-----+-----+
                |
                v
      Execution Monitoring
                |
                v
       Metrics & Evaluation
```

## Scheduling Algorithm

### 1. Job Ingestion

Each incoming job is represented using the following attributes:

- Job ID
- Arrival time
- GPU requirement
- Execution duration (for simulation and evaluation)
- Current execution state

### 2. GPU Attainment Tracking

The scheduler continuously tracks the cumulative GPU service received by each job.

\[
A_i = \sum_{t=1}^{T} G_i(t)\Delta t
\]

Where:

- \(A_i\): GPU service attainment of job i
- \(G_i(t)\): Number of GPUs allocated during interval t
- \(\Delta t\): Scheduling interval

Jobs with lower service attainment receive higher scheduling priority.

### 3. Multi-Level Feedback Queue

Jobs are assigned to priority queues based on accumulated GPU service.

| Queue | Attainment | Priority |
|---|---|---|
| Q0 | Lowest | Highest |
| Q1 | Moderate | Medium |
| Q2 | High | Low |
| Q3 | Highest | Lowest |

As jobs consume more GPU time, they transition into lower-priority queues.

### 4. Preemption

When a higher-priority job requires resources, the scheduler evaluates whether an executing workload should be preempted.

Decisions consider:

- Relative job priority
- GPU availability
- Resource requirements
- Preemption overhead

### 5. Starvation Prevention

Fairness mechanisms ensure that high-attainment jobs are not indefinitely denied GPU resources.

Potential mechanisms include:

- Queue aging
- Maximum waiting-time thresholds
- Fair-share allocation
- Controlled time-slicing

## Performance Metrics

| Metric | Description |
|---|---|
| Average JCT | Average time between job arrival and completion |
| Tail JCT | High-percentile job completion time |
| Waiting Time | Time spent waiting for GPU allocation |
| GPU Utilization | Percentage of available GPU capacity in use |
| Preemption Count | Number of job interruptions |
| Throughput | Number of completed jobs per unit time |

## Baseline Comparison

The scheduler can be evaluated against traditional scheduling policies:

**FCFS (First-Come-First-Served):** Executes jobs in arrival order.

**SJF (Shortest Job First):** Prioritizes jobs with shorter execution durations, requiring runtime estimates.

**Tiresias-Inspired Scheduler:** Prioritizes workloads using GPU service attainment rather than predicted execution duration.

Comparing these approaches helps
