---
title: REINFORCEMENT LEARNING
author: Tao He
date: 2022-06-30
category: Jekyll
layout: post
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
   y = 1, \quad y = 3 \quad ✅
   $$

**Key Insight**: Through 143 iterations, the model learns to:  
- Check domain restrictions first  
- Prefer factoring over quadratic formula  
- Verify solutions post-calculation

# Let's play a game to experience RL



---
