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
   y = 1, \quad y = 3 \quad ✅
   $$

**Key Insight**: Through 143 iterations, the model learns to:  
- Check domain restrictions first  
- Prefer factoring over quadratic formula  
- Verify solutions post-calculation

# Let's play a game to experience RL

<div class="rl-game-module">
    <h3>互动演示：猜数字游戏</h3>
    
    <div class="rl-explanation">
        <p>这个简单的猜数字游戏展示了强化学习的基本原理。尝试用最少的次数猜出1-100之间的随机数字！</p>
        <ul>
            <li><strong>探索与利用</strong>: 你需要在尝试新数字(探索)和利用已知信息(利用)之间找到平衡</li>
            <li><strong>奖励信号</strong>: "大了"或"小了"的反馈就像RL中的奖励信号</li>
            <li><strong>策略优化</strong>: 最优策略(二分查找)可以保证在最多7次内找到答案</li>
        </ul>
    </div>

    <div class="game-container">
        <div class="game-controls">
            <label for="guess">你的猜测:</label>
            <input type="number" id="guess" min="1" max="100">
            <button onclick="checkGuess()">提交</button>
            <button onclick="resetGame()">重置</button>
        </div>
        
        <div id="feedback" class="feedback"></div>
        
        <div class="game-stats">
            <p>猜测次数: <span id="attempts">0</span> | 最优次数: <span id="optimal">7</span></p>
        </div>
    </div>

    <style>
        .rl-game-module {
            background-color: #f8f9fa;
            border-radius: 8px;
            padding: 20px;
            margin: 30px 0;
            box-shadow: 0 2px 6px rgba(0,0,0,0.1);
        }
        .rl-explanation {
            background-color: #fff8e1;
            padding: 15px;
            border-left: 4px solid #ffc107;
            margin-bottom: 20px;
            font-size: 0.95em;
        }
        .game-container {
            background-color: white;
            padding: 15px;
            border-radius: 6px;
            box-shadow: 0 1px 4px rgba(0,0,0,0.08);
        }
        .game-controls {
            display: flex;
            align-items: center;
            gap: 10px;
            flex-wrap: wrap;
            margin-bottom: 15px;
        }
        .feedback {
            font-weight: bold;
            margin: 10px 0;
            padding: 8px;
            border-radius: 4px;
            min-height: 20px;
        }
        .too-high {
            background-color: #ffebee;
            color: #c62828;
        }
        .too-low {
            background-color: #e8f5e9;
            color: #2e7d32;
        }
        .correct {
            background-color: #e3f2fd;
            color: #1565c0;
        }
        button {
            background-color: #3498db;
            color: white;
            border: none;
            padding: 6px 12px;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #2980b9;
        }
        input {
            padding: 6px;
            border: 1px solid #ddd;
            border-radius: 4px;
            width: 80px;
        }
        .game-stats {
            font-size: 0.9em;
            color: #555;
        }
    </style>

    <script>
        // 确保函数名称不与页面上其他脚本冲突
        (function() {
            let targetNumber;
            let attempts;
            let gameActive = true;
            
            // 将函数暴露到全局作用域
            window.checkGuess = function() {
                if (!gameActive) return;
                
                const guessInput = document.getElementById('guess');
                const guess = parseInt(guessInput.value);
                
                if (isNaN(guess) || guess < 1 || guess > 100) {
                    document.getElementById('feedback').textContent = '请输入1到100之间的有效数字！';
                    document.getElementById('feedback').className = 'feedback';
                    return;
                }
                
                attempts++;
                document.getElementById('attempts').textContent = attempts;
                
                const feedback = document.getElementById('feedback');
                
                if (guess < targetNumber) {
                    feedback.textContent = `${guess} 太小了！试试更大的数字。`;
                    feedback.className = 'feedback too-low';
                } else if (guess > targetNumber) {
                    feedback.textContent = `${guess} 太大了！试试更小的数字。`;
                    feedback.className = 'feedback too-high';
                } else {
                    feedback.textContent = `正确！${guess} 是目标数字。你用了 ${attempts} 次猜测。`;
                    feedback.className = 'feedback correct';
                    gameActive = false;
                }
                
                guessInput.value = '';
            };
            
            window.resetGame = function() {
                initGame();
            };
            
            function initGame() {
                targetNumber = Math.floor(Math.random() * 100) + 1;
                attempts = 0;
                gameActive = true;
                document.getElementById('attempts').textContent = attempts;
                document.getElementById('feedback').textContent = '';
                document.getElementById('feedback').className = 'feedback';
                console.log("目标数字(仅调试用): " + targetNumber);
            }
            
            // 等待DOM加载完成后初始化游戏
            if (document.readyState === 'loading') {
                document.addEventListener('DOMContentLoaded', initGame);
            } else {
                initGame();
            }
        })();
    </script>
</div>

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
