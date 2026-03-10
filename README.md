# Cognitive Insider Threat Detection System

## Overview

This project implements an **Insider Threat Detection System based on cognitive behavior modeling**.  
Instead of relying only on rule-based monitoring or simple anomaly detection, the system attempts to **understand the behavioral pattern and decision trajectory of a user interacting with organizational resources**.

By modeling how users explore, access, and manipulate data over time, the system can identify **suspicious behavioral patterns that may indicate data exfiltration, misuse of access privileges, or malicious insider activity**.

The approach combines **behavioral analytics, probabilistic modeling, and risk scoring** to detect threats with greater reliability.

---

# Problem

Organizations invest heavily in protecting systems from external attackers, yet **insider threats remain one of the most difficult security problems**.

An insider threat occurs when a legitimate user — employee, contractor, or partner — abuses authorized access to steal or misuse sensitive information.

Traditional detection systems often fail because:

- Insiders operate with **valid credentials**
- Their actions may initially appear **similar to normal work behavior**
- Rule-based systems generate **large numbers of false positives**
- Security teams struggle to distinguish **normal data usage from malicious behavior**

This creates two major issues:

1. **Data theft may go undetected for long periods**
2. **Security teams waste time investigating harmless alerts**

A more intelligent behavioral model is required.

---

# Solution

This project introduces a **cognitive behavioral model for insider threat detection**.

Instead of only detecting anomalies, the system attempts to capture **how a user's behavior evolves over time**, including:

- exploration of data
- repeated access to sensitive files
- preparation activities (compression, copying)
- potential exfiltration actions

The system models user activity as a **behavioral trajectory**, where each action contributes evidence about the user's intent.

By analyzing patterns such as **novelty of actions, sensitivity of accessed data, and effort invested in operations**, the model can infer whether a user is:

- performing normal work
- exploring unfamiliar resources
- preparing data extraction
- executing a suspicious activity

This cognitive interpretation significantly improves detection capability.

---

# Why Cognitive Pattern Modeling Works

Traditional systems analyze **individual events**, while insider attacks typically occur as **a sequence of coordinated actions**.

Cognitive modeling captures these patterns by observing:

- how behavior changes over time
- whether the user is moving toward sensitive assets
- whether actions indicate planning or execution

This approach provides two major advantages.

### Reduced False Positives

Many anomaly detection systems flag normal behavior as suspicious.  
Cognitive pattern modeling considers **behavioral context and progression**, reducing unnecessary alerts.

### Better Detection of Real Threats

Insider attackers usually follow a progression:

1. Explore the system
2. Identify valuable data
3. Collect or prepare files
4. Extract information

By modeling this progression, the system can detect threats **before the final data theft occurs**.

---

# Key Features

- Behavioral modeling of user activity
- Cognitive state tracking
- Sensitivity-aware data access monitoring
- Novelty-based anomaly scoring
- Effort-based activity analysis
- Hidden Markov Model phase inference
- Risk score based threat detection
- Reduced false positive alerting

---

# System Architecture

The system processes activity data through several analytical stages.
Raw Logs
->
Canonical Event Generation
->
Cognitive Observation Vectors
->
HMM Phase Inference
->
Trajectory & Intent Reasoning
->
Risk Scoring
->
Threat Alert Generation

![flow](https://github.com/user-attachments/assets/78ab379d-ff76-44fd-b76d-d530b32b28ae)


The detailed mathematical formulation of the model is provided in the **`math_decode/` folder** of this repository.

This folder includes explanations for:

- behavioral novelty calculation
- effort cost modeling
- sensitivity scoring
- Hidden Markov Model formulation
- risk score computation

---

# Installation

Clone the repository

```bash
git clone https://github.com/yourusername/insider-threat-detection.git
Move to the project directory

cd insider-threat-detection

Install required dependencies

pip install -r requirements.txt
```
## Running the Project

The project pipeline is divided into multiple stages.  
Each stage performs a specific transformation on the data and must be executed **in sequence**.

To successfully run the system, execute the `main.py` file for each stage **one after another in order**.

### Execution Order

Run the following stages sequentially:

1. **Stage 2 – Canonical Event Generation**

Convert raw activity logs into standardized canonical events.

```bash
python stage2/main.py
````

2. **Stage 3 – Cognitive Observation Vectors**

Transform canonical events into cognitive observation vectors containing behavioral features such as sensitivity, novelty, and effort.

```bash
python stage3/main.py
```

3. **Stage 4 – HMM Phase Inference**

Apply the Hidden Markov Model to infer behavioral phases of users based on the observation sequence.

```bash
python stage4/main.py
```

4. **Stage 5 – Trajectory & Intent Reasoning**

Analyze the sequence of inferred phases to understand behavioral trajectories and detect possible malicious intent.

```bash
python stage5/main.py
```

5. **Stage 6 – Alert Generation**

Compute the final risk score and generate insider threat alerts if the risk threshold is exceeded.

```bash
python stage6/main.py
```

