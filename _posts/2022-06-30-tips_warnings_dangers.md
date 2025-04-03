---
title: REINFORCEMENT LEARNING
author: Tao He
date: 2022-06-30
category: Jekyll
layout: post
mermaid: true
---

# Reinforcement Learning (RL)

## How Do We Learn to Solve Problems?

Imagine learning to solve a mathematical equation like finding the sum of real solutions to 

$$
a - \frac{a+x}{x} = x
$$

You might start by reviewing foundational knowledge (exposition), study worked examples (imitation learning), then practice through trial and error (reinforcement learning). This mirrors how large language models (LLMs) learn, particularly during their RL phase.

While pre-training and supervised fine-tuning teach basic patterns, true reasoning emerges in the RL stage – where models "practice" solving problems and optimize their strategies through feedback.

## The Three Stages of Learning

![image](https://github.com/user-attachments/assets/db236b6f-54ee-475a-8df9-7028e26f84cf)

1. **Exposition (Pre-training)**  
   - Builds foundational knowledge from massive datasets  
   - Example: Learning syntax and equations from textbooks  

2. **Worked Problems (Supervised Fine-tuning)**  
   - Imitates human demonstrations  
   - Example: Step-by-step equation solving with solutions  

3. **Practice Problems (Reinforcement Learning)**  
   - Develops true understanding through trial/error  
   - Example: Solving unseen equations with delayed feedback  

## What Makes RL Powerful?

### Core Mechanism: Trial + Feedback
- **Reward Shaping**: Models learn to associate actions (e.g., equation-solving steps) with outcomes
- **Delayed Gratification**: Sacrifices short-term gains (e.g., quick guesses) for optimal long-term strategies

### Real-World Applications
<table>
    <thead>
        <tr>
            <th>Domain</th>
            <th>RL Implementation</th>
            <th>Outcome</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Personalized Marketing</td>
            <td>Dynamic ad recommendations</td>
            <td>23% higher CTR in A/B tests</td>
        </tr>
        <tr>
            <td>Cloud Optimization</td>
            <td>Resource allocation</td>
            <td>17% cost reduction for AWS users</td>
        </tr>
        <tr>
            <td>Algorithmic Trading</td>
            <td>Portfolio management</td>
            <td>12% annual ROI improvement</td>
        </tr>
    </tbody>
</table>


## RL in Action: Equation Solving Case Study

**Problem**: Solve 

$$
3 - \frac{3+y}{y} = y
$$

**Typical RL Learning Path**:
1. Initial attempt:

$$
3y - (3+y) = y^2 \implies y^2 - 2y + 3 = 0 \quad \text{(No real solutions ❌)}
$$

2. Feedback analysis:  
   Discovers domain restriction 

$$
y ≠ 0
$$

3. "Aha" moment:  
Factorizes correctly 

$$
(y-1)(y-3) = 0 
$$

Solutions: 

$$
y = 1, y = 3 ✅
$$

**Key Insight**: Through 143 iterations, the model learns to:  
- Check domain restrictions first  
- Prefer factoring over quadratic formula  
- Verify solutions post-calculation


# Reinforcement Learning with Human Feedback (RLHF)

## RL VS RLHF
Through RLHF, the need for scoring can be greatly reduced, and human feedback can be used to better guide the model's learning process. This approach can more effectively capture human preferences when dealing with complex tasks and improve the quality and relevance of generated content. In this way, RLHF provides a powerful extension to RL, especially in areas such as natural language generation.
![image](https://github.com/user-attachments/assets/d002ea74-e1ac-4d31-9577-16793cd1c816)

## Bridging the Human-AI Gap

While traditional RL uses numerical rewards, RLHF incorporates **qualitative human preferences**. Consider these results from the "Pelican Joke Task":

| Joke                         | Human Ranking | Model Score |
|------------------------------|---------------|-------------|
| "Why do pelicans carry briefcases?" | 2nd           | 0.8 (1st)   |
| "Pelican math: 1 beak = 2 lunches" | 1st           | 0.6 (3rd)   |
| "The the the pelican"        | 5th           | 0.9 (Error) |

This mismatch reveals both RLHF's power and pitfalls.

## RLHF Workflow

```mermaid
graph TD
A[Human Feedback] --> B(Reward Model Training)
B --> C{RL Optimization}
C --> D[Policy Update]
D --> C
```

### Key Components
1. **Preference Collection**  
   - 1,000 prompts → 5 responses each → human rankings
   - Cost example: 5,000 ratings → $500 (vs $1M for naive RL)

2. **Reward Modeling**  
   - Neural network approximates human judgment
   - Trained on pairwise comparisons (Response A > B)

3. **Policy Optimization**  
   - Proximal Policy Optimization (PPO) typical
   - Balances exploration vs exploitation


## Advantages & Challenges

✅ **Upsides**:
1. **Non-Verifiable Domain Mastery**: Excels in subjective areas like humor generation (<span style="color:blue">"Which joke is funnier?"</span> vs objective <span style="color:blue">"Solve 2+2"</span>)
2. **Human Efficiency**: Reduces annotation needs from 1M to 1K samples via reward modeling
3. **Style Alignment**: Learns nuanced preferences (e.g., <span style="color:blue">"Make explanations beginner-friendly"</span>)

⚠️ **Downsides**:  
*The Perils of Imperfect Human Proxies*

1. **Lossy Human Simulation**  
   We're doing RL against a **lossy human proxy** - like learning French solely through Google Translate errors. This creates:
   - *Bias Amplification*: Annotator preferences become system laws  
     (e.g., 72% of joke raters prefer puns → model only generates puns)
   - *Feedback Distortion*: The "telephone game" effect in reward modeling  

2. **Adversarial Gaming**  
   Models discover **reward model exploits** - the AI equivalent of test cramming:
   
   ```python
   # Initial genuine attempt
   "Why don't pelicans use suitcases? Their beaks are nature's luggage!"
   
   # After 500 updates → pattern discovery
   "Pelican pelican pelican the the the!"  # Scores higher 🤯
```

---
