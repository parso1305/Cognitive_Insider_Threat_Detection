# Mathematical Model

The Insider Threat Detection system models user behavior using a **cognitive risk framework**.  
Instead of analyzing events independently, the system estimates a **dynamic cognitive state of the user** and evaluates risk based on sensitivity, novelty, and effort of actions.

---

## 1. Cognitive State Representation

Each user is represented by a cognitive state vector that evolves over time.

$$
S_u(t) = \{K(t), C(t), I(t), U(t), R(t), E(t), G(t)\}
$$

Where:

- **K(t)** – Knowledge of the system or domain  
- **C(t)** – Capability (technical ability of the user)  
- **I(t)** – Intent strength  
- **U(t)** – Uncertainty in behavior  
- **R(t)** – Risk tolerance  
- **E(t)** – Effort invested in actions  
- **G(t)** – Goal proximity (distance to sensitive assets)

The system estimates the probability of the user’s cognitive state given observed events:

$$
P(S_u(t) \mid E_{1:t})
$$

Where:

- $E_{1:t}$ represents all observed events up to time $t$.

Each event updates the cognitive state:

$$
S_u(t+1) = f(S_u(t), e_t) + \epsilon
$$

Where:

- $e_t$ = observed event  
- $f(\cdot)$ = state update function  
- $\epsilon$ = noise or uncertainty

---

# 2. Data Sensitivity Model

Each accessed object has an associated **sensitivity score** based on its domain and type.

$$
S_{obj} = f(\text{Object Type}, \text{Domain})
$$

If the file is dynamic or frequently modified, sensitivity is adjusted:

$$
S_{eff} = \text{Clamp}(S_{base} \times (0.6 + 0.4 \times variability))
$$

Where:

- $S_{base}$ = base sensitivity score  
- $variability$ = frequency of changes in the object  

The value is restricted to:

$$
S_{eff} \in [0,1]
$$

---

# 3. Novelty Score

Novelty measures how unusual an action is for a user.

$$
Novelty = 1 - \frac{Count_u(o)}{H_u}
$$

Where:

- $Count_u(o)$ = number of times user $u$ accessed object $o$  
- $H_u$ = recent object access history of the user  

Interpretation:

- **Novelty ≈ 0** → Normal behavior  
- **Novelty ≈ 1** → Highly unusual behavior

---

# 4. Effort Cost Model

Each action requires a certain effort level.

| Action | Base Effort |
|------|------|
| Read | 0.2 |
| Copy | 0.5 |
| Zip | 0.6 |
| Upload | 0.8 |

Effort cost is calculated as:

$$
E_{cost} = base\_effort \times (1 + retries) \times time\_density
$$

Where:

- **base_effort** = base cost of the action  
- **retries** = repeated attempts  
- **time_density** = number of actions in a short time window  

Effort values are normalized:

$$
E_{cost} \in [0,1]
$$

---

# 5. Risk Score Calculation

The final insider threat risk score is calculated as a weighted combination of:

- Data sensitivity
- Effort cost
- Behavioral novelty

$$
R_{cost} = 0.5 \cdot S_{eff} + 0.3 \cdot E_{cost} + 0.2 \cdot N
$$

Where:

- $S_{eff}$ = effective sensitivity  
- $E_{cost}$ = effort cost  
- $N$ = novelty score  

---

# 6. Detection Threshold

An insider threat alert is generated if the risk score exceeds a predefined threshold:

$$
R_{cost} > \tau
$$

Typical threshold:

$$
\tau = 0.7
$$

This threshold can be tuned depending on organizational security policies.

---

# Behavioral Interpretation

The model captures behavioral patterns such as:

- **High novelty + low effort → Exploration phase**
- **Low novelty + high effort → Execution or exfiltration phase**

This helps distinguish between normal activity and potential malicious behavior.

---
