# Agentic Self-Healing MLOps

A simulation-based Proof of Concept (POC) for detecting infrastructure anomalies, proposing safe remediation actions, and verifying sustained recovery in an MLOps environment. Built in Python using Google Colab with an interactive Gradio dashboard.

## Project Overview

The project simulates Kubernetes-style workloads and demonstrates how an agent-based monitoring and remediation system can reduce manual intervention while maintaining policy and safety controls. No live Kubernetes cluster or cloud infrastructure is required for the demonstration.

## Key Features

- Multi-pod monitoring of inference, training, and feature-store workloads.
- CPU and memory telemetry, fault injection, and anomaly detection.
- Ledger-backed validation of controlled simulator observations.
- Root-cause diagnosis and confidence-based remediation proposals.
- Policy approval, decision freshness, cooldown, retry and escalation safeguards.
- Simulated remediation, including replica scaling.
- Incident tracking and sustained recovery verification.
- Unified Gradio dashboard for interactive demonstration.

## Architecture

``` mermaid
flowchart TD
    A[Simulated Pods] --> B[Telemetry Collection]
    B --> C[Evidence Validation]
    C --> D[Anomaly Detection and Diagnosis]
    D --> E[Remediation Policy Engine]
    E --> F[Safety-Controlled Coordinator]
    F --> G[Remediation Executor]
    G --> A
    G --> H[Recovery Verification]
    H --> I[Incident Resolution]
    H -->|Failed recovery| J[Retry or Escalation]
    J --> F
    K[Gradio Dashboard] -.-> B
    K -.-> F
    K -.-> H
```

## Technology Stack

| Technology          | Purpose                                           |
|---------------------|---------------------------------------------------|
| Python              | Simulation, agent logic and orchestration         |
| Google Colab        | Notebook execution environment                    |
| Gradio              | Interactive monitoring and remediation dashboard  |
| Kubernetes concepts | Simulated pods, replicas and lifecycle management |

## Repository Structure

``` text
.
├── Project.ipynb    # POC implementation and demonstration notebook
└── README.md        # Project documentation
```

## Getting Started

1.  Download `Project.ipynb` from this repository.
2.  Open [Google Colab](https://colab.research.google.com/) and upload the notebook using **File \> Upload notebook**.
3.  Execute the notebook cells sequentially from the beginning, including dependency installation and component initialization.
4.  Run the unified Gradio dashboard launch cell.
5.  Open the displayed Gradio interface and follow the demonstration workflow below.

The notebook uses in-memory simulation state. Restarting the runtime clears that state; rerun the initialization cells to start a fresh session. Avoid exposing sensitive information through public Gradio share links.

## Demonstration Workflow

1.  Select a simulated pod with one replica in the dashboard.
2.  Inject CPU pressure at 95% twice to record consecutive observations.
3.  Open **Evidence Validation** and validate the CPU evidence.
4.  Open **AI Diagnosis** and generate a remediation decision.
5.  Review the diagnosis, policy approval and safety checks.
6.  Open **Remediation** and execute the reviewed `scale_up` action.
7.  Open **Recovery Verification** and verify three successive healthy ticks.
8.  Refresh the multi-pod overview and confirm that the incident is `RESOLVED`.

## Validated POC Results

| Simulated pod       | CPU before | CPU after | Final memory | Replicas before → after | Incident status |
|---------------------|-----------:|----------:|-------------:|-------------------------|-----------------|
| multi-inference-pod |        95% |     47.5% |       29.67% | 1 → 2                   | RESOLVED        |
| multi-training-pod  |        95% |     47.5% |       30.52% | 1 → 2                   | RESOLVED        |
| multi-feature-pod   |        95% |     47.5% |       36.09% | 1 → 2                   | RESOLVED        |

- Three out of three incidents resolved in the demonstrated CPU-pressure scenarios.
- CPU threshold: 85%; sustained recovery requirement: three healthy simulation ticks.
- CPU utilization decreased by 50% under the simulator’s replica-scaling model.

These results are from a controlled simulation, not production infrastructure benchmarks.

## Safety and Limitations

Remediation proposals are separated from execution. The demonstrated workflow requires validated evidence, policy checks and a reviewed execution action. Recovery must be verified before an incident is resolved. The observation ledger is an in-memory simulation record, not a tamper-proof audit system. Diagnosis confidence is a score from the implemented logic, not a calibrated probability of correctness.

This POC does not connect to a live Kubernetes cluster and does not claim fully autonomous production operation. The demonstrated CPU recovery scenarios do not establish reliability across all possible faults.

## Future Enhancements

- Expand automated safety, duplicate-action and failure-path testing.
- Integrate real Kubernetes APIs and live telemetry (e.g., Prometheus).
- Add persistent incident and audit storage.
- Validate remediation policies against real workloads and service-level objectives.
- Add authentication, authorization and deployment controls.

## Project Status

Working simulation-based POC with an end-to-end Gradio demonstration. Further reliability testing and production integration are future work.
