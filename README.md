GPU Smart Scheduler (Microsoft Tiresias Replication)
Overview

An intelligent GPU cluster scheduler inspired by Microsoft Tiresias, designed to minimize job completion time (JCT) and reduce head-of-line blocking for deep learning workloads when exact job durations or resource needs are unknown in advance.

Core Mechanism & Design

Attainment-Based Scheduling: Prioritizes jobs based on service attainment (the cumulative GPU-time a job has received so far, measured in GPU-seconds), favoring jobs with lower attainment so shorter or less-consumed jobs make rapid progress without requiring upfront runtime estimates.

Multi-Level Feedback Queue (MLFQ) / Priority Queues: Groups jobs into priority tiers based on service attainment thresholds, moving jobs down to lower-priority queues as they accumulate more GPU compute time.

Starvation Prevention & Aging / Fairness Controls: Uses time-slicing, queue aging/demotion rules, or quota/fair-share mechanisms so long-running training jobs continue advancing rather than being indefinitely preempted.

Placement & Contention Awareness: Assigns queued tasks to available GPUs/nodes while accounting for allocation constraints, preemption overhead, and multi-tenant cluster contention.

Key Capabilities & Workflow

Job Ingestion & Queueing: Accepts incoming training jobs with dynamic arrival times and GPU requirements into the scheduling pool.

Attainment Tracking: Continuously tracks GPU-time consumed per job across scheduling intervals/ticks.

Priority & Preemption Decisions: Ranks active/queued jobs by attainment tier and dispatches or preempts workloads cleanly when higher-priority jobs become eligible.

Telemetry & Evaluation: Logs metrics such as average/tail Job Completion Time (JCT), queue wait time, preemption count, and GPU utilization across synthetic or trace-driven workloads.

Why It Matters

In shared GPU clusters, First-Come-First-Served (FCFS) or static priority schedulers often trap short experiments behind long training runs. Replicating Tiresias provides a practical, attainment-driven baseline that improves responsiveness and average turnaround time without relying on fragile runtime predictions—making it straightforward to extend with workload forecasting, gang/topology-aware placement, or cluster simulation benchmarks.

If you'd like to tailor this for a specific format—such as a GitHub README, a resume bullet point, or a research/project report with your exact stack, metrics, or simulation setup—share those details and I can refine it for you.
