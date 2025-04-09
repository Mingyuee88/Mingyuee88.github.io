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

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RL Number Guessing Game</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f7f9fc;
            color: #333;
        }
        h1, h2 {
            color: #2c3e50;
        }
        .game-container {
            background: white;
            border-radius: 8px;
            padding: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        .controls {
            margin: 20px 0;
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            align-items: center;
        }
        button {
            background-color: #3498db;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #2980b9;
        }
        button:disabled {
            background-color: #95a5a6;
            cursor: not-allowed;
        }
        input, select {
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        .history {
            margin-top: 20px;
            max-height: 200px;
            overflow-y: auto;
            padding: 10px;
            background-color: #f8f9fa;
            border-radius: 4px;
        }
        .feedback {
            margin: 15px 0;
            padding: 10px;
            border-radius: 4px;
            font-weight: bold;
        }
        .too-high {
            background-color: #ffcccc;
            color: #d63031;
        }
        .too-low {
            background-color: #cce5ff;
            color: #0984e3;
        }
        .correct {
            background-color: #d4edda;
            color: #27ae60;
        }
        .chart-container {
            width: 100%;
            height: 300px;
            margin-top: 20px;
        }
        .explanation {
            background-color: #f1f9ff;
            padding: 15px;
            border-radius: 8px;
            margin-top: 20px;
            border-left: 4px solid #3498db;
        }
        .hidden {
            display: none;
        }
        .strategy-demo {
            margin: 20px 0;
            padding: 15px;
            background-color: #fff8e1;
            border-radius: 8px;
            border-left: 4px solid #ffc107;
        }
        .ai-thinking {
            font-style: italic;
            color: #555;
        }
        .guess-input {
            display: flex;
            gap: 10px;
        }
        @media (max-width: 600px) {
            .controls {
                flex-direction: column;
                align-items: flex-start;
            }
        }
    </style>
</head>
<body>
    <h1>Reinforcement Learning: Number Guessing Game</h1>
    
    <div class="explanation">
        <h3>Learning Reinforcement Learning Through Play</h3>
        <p>This game demonstrates key concepts in Reinforcement Learning:</p>
        <ul>
            <li><strong>Policy</strong>: Your strategy for guessing numbers</li>
            <li><strong>Reward</strong>: Feedback when you're getting closer</li>
            <li><strong>State</strong>: The range of possible numbers based on previous guesses</li>
            <li><strong>Learning</strong>: Improving your strategy over multiple games</li>
        </ul>
        <p>Try to develop an optimal strategy that minimizes the number of guesses needed!</p>
    </div>

    <div class="game-container">
        <h2>Game Settings</h2>
        <div class="controls">
            <div>
                <label for="difficulty">Difficulty Level:</label>
                <select id="difficulty">
                    <option value="easy">Easy (1-100)</option>
                    <option value="medium">Medium (1-1,000)</option>
                    <option value="hard">Hard (1-10,000)</option>
                    <option value="expert">Expert (1-100,000)</option>
                </select>
            </div>
            <button id="start-game">Start New Game</button>
            <button id="show-ai-demo">Show AI Strategy Demo</button>
        </div>

        <div id="game-area" class="hidden">
            <h3>I'm thinking of a number... <span id="range-display"></span></h3>
            <div class="guess-input">
                <input type="number" id="guess-input" placeholder="Enter your guess">
                <button id="submit-guess">Guess</button>
            </div>
            <div id="feedback" class="feedback"></div>
            <div>Guesses: <span id="guess-count">0</span></div>
            <div class="history">
                <h4>Guess History:</h4>
                <ul id="guess-history"></ul>
            </div>
        </div>

        <div id="ai-demo" class="strategy-demo hidden">
            <h3>AI Strategy Demonstration</h3>
            <p>Watch how an AI would solve this using binary search (optimal strategy):</p>
            <div id="ai-thinking" class="ai-thinking"></div>
            <div id="ai-history"></div>
            <button id="ai-next-step" class="hidden">Next Step</button>
            <button id="ai-auto-solve">Auto-Solve</button>
        </div>
    </div>

    <div class="game-container">
        <h2>Your Learning Curve</h2>
        <p>This chart shows how your performance improves over multiple games:</p>
        <div class="chart-container">
            <canvas id="learning-chart"></canvas>
        </div>
        <div>
            <h3>Performance Metrics</h3>
            <p>Average guesses: <span id="avg-guesses">-</span></p>
            <p>Best game: <span id="best-game">-</span> guesses</p>
            <p>Games played: <span id="games-played">0</span></p>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.7.1/chart.min.js"></script>
    <script>
        // Game variables
        let targetNumber;
        let minRange;
        let maxRange;
        let guessCount;
        let gameActive = false;
        let gameHistory = [];
        let aiDemoActive = false;
        let aiCurrentMin;
        let aiCurrentMax;
        let aiGuessCount;
        let aiStepInterval;

        // DOM elements
        const difficultySelect = document.getElementById('difficulty');
        const startGameBtn = document.getElementById('start-game');
        const showAiDemoBtn = document.getElementById('show-ai-demo');
        const gameArea = document.getElementById('game-area');
        const rangeDisplay = document.getElementById('range-display');
        const guessInput = document.getElementById('guess-input');
        const submitGuessBtn = document.getElementById('submit-guess');
        const feedbackEl = document.getElementById('feedback');
        const guessCountEl = document.getElementById('guess-count');
        const guessHistoryEl = document.getElementById('guess-history');
        const aiDemo = document.getElementById('ai-demo');
        const aiThinking = document.getElementById('ai-thinking');
        const aiHistory = document.getElementById('ai-history');
        const aiNextStepBtn = document.getElementById('ai-next-step');
        const aiAutoSolveBtn = document.getElementById('ai-auto-solve');
        const avgGuessesEl = document.getElementById('avg-guesses');
        const bestGameEl = document.getElementById('best-game');
        const gamesPlayedEl = document.getElementById('games-played');

        // Initialize learning chart
        let learningChart;
        function initChart() {
            const ctx = document.getElementById('learning-chart').getContext('2d');
            learningChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: [],
                    datasets: [{
                        label: 'Number of Guesses',
                        data: [],
                        borderColor: '#3498db',
                        backgroundColor: 'rgba(52, 152, 219, 0.2)',
                        tension: 0.1,
                        fill: true
                    },
                    {
                        label: 'Optimal (Binary Search)',
                        data: [],
                        borderColor: '#27ae60',
                        borderDash: [5, 5],
                        pointRadius: 0
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: {
                            beginAtZero: true,
                            title: {
                                display: true,
                                text: 'Number of Guesses'
                            }
                        },
                        x: {
                            title: {
                                display: true,
                                text: 'Game Number'
                            }
                        }
                    }
                }
            });
        }

        // Get range based on difficulty
        function getDifficultyRange(difficulty) {
            switch(difficulty) {
                case 'easy': return [1, 100];
                case 'medium': return [1, 1000];
                case 'hard': return [1, 10000];
                case 'expert': return [1, 100000];
                default: return [1, 100];
            }
        }

        // Calculate optimal number of guesses using binary search
        function calculateOptimalGuesses(min, max) {
            return Math.ceil(Math.log2(max - min + 1));
        }

        // Start a new game
        function startGame() {
            const [min, max] = getDifficultyRange(difficultySelect.value);
            minRange = min;
            maxRange = max;
            targetNumber = Math.floor(Math.random() * (max - min + 1)) + min;
            guessCount = 0;
            gameActive = true;
            
            // Update UI
            gameArea.classList.remove('hidden');
            rangeDisplay.textContent = `between ${min} and ${max}`;
            feedbackEl.textContent = '';
            feedbackEl.className = 'feedback';
            guessCountEl.textContent = '0';
            guessHistoryEl.innerHTML = '';
            guessInput.value = '';
            guessInput.focus();
            
            console.log(`Game started: Target number is ${targetNumber}`);
        }

        // Make a guess
        function makeGuess() {
            if (!gameActive) return;
            
            const guess = parseInt(guessInput.value);
            if (isNaN(guess) || guess < minRange || guess > maxRange) {
                feedbackEl.textContent = `Please enter a valid number between ${minRange} and ${maxRange}`;
                feedbackEl.className = 'feedback';
                return;
            }
            
            guessCount++;
            guessCountEl.textContent = guessCount;
            
            // Add to history
            const listItem = document.createElement('li');
            
            if (guess === targetNumber) {
                feedbackEl.textContent = `Correct! You found the number in ${guessCount} guesses.`;
                feedbackEl.className = 'feedback correct';
                listItem.textContent = `Guess #${guessCount}: ${guess} - CORRECT!`;
                endGame();
            } else if (guess < targetNumber) {
                feedbackEl.textContent = 'Too low! Try a higher number.';
                feedbackEl.className = 'feedback too-low';
                listItem.textContent = `Guess #${guessCount}: ${guess} - Too low`;
            } else {
                feedbackEl.textContent = 'Too high! Try a lower number.';
                feedbackEl.className = 'feedback too-high';
                listItem.textContent = `Guess #${guessCount}: ${guess} - Too high`;
            }
            
            guessHistoryEl.appendChild(listItem);
            guessInput.value = '';
            guessInput.focus();
        }

        // End the game and update statistics
        function endGame() {
            gameActive = false;
            
            // Add to game history
            gameHistory.push({
                difficulty: difficultySelect.value,
                range: [minRange, maxRange],
                guesses: guessCount,
                optimal: calculateOptimalGuesses(minRange, maxRange)
            });
            
            // Update chart
            updateChart();
            
            // Update statistics
            updateStats();
        }

        // Update the learning curve chart
        function updateChart() {
            learningChart.data.labels = gameHistory.map((_, i) => `Game ${i + 1}`);
            learningChart.data.datasets[0].data = gameHistory.map(game => game.guesses);
            learningChart.data.datasets[1].data = gameHistory.map(game => game.optimal);
            learningChart.update();
        }

        // Update statistics display
        function updateStats() {
            if (gameHistory.length === 0) return;
            
            const totalGuesses = gameHistory.reduce((sum, game) => sum + game.guesses, 0);
            const average = totalGuesses / gameHistory.length;
            const best = Math.min(...gameHistory.map(game => game.guesses));
            
            avgGuessesEl.textContent = average.toFixed(1);
            bestGameEl.textContent = best;
            gamesPlayedEl.textContent = gameHistory.length;
        }

        // AI Demo functions
        function startAiDemo() {
            aiDemo.classList.remove('hidden');
            aiCurrentMin = minRange;
            aiCurrentMax = maxRange;
            aiGuessCount = 0;
            aiHistory.innerHTML = '';
            aiThinking.textContent = 'Thinking...';
            aiNextStepBtn.classList.remove('hidden');
            
            // Show first step after a short delay
            setTimeout(() => {
                aiDemoActive = true;
                aiNextStep();
            }, 1000);
        }

        function aiNextStep() {
            if (!aiDemoActive) return;
            
            aiGuessCount++;
            const middleValue = Math.floor((aiCurrentMin + aiCurrentMax) / 2);
            
            const thinkingText = `Step ${aiGuessCount}: Current range is [${aiCurrentMin}-${aiCurrentMax}].\nMiddle value is ${middleValue}`;
            aiThinking.textContent = thinkingText;
            
            const listItem = document.createElement('div');
            
            if (middleValue === targetNumber) {
                listItem.innerHTML = `<strong>Guess #${aiGuessCount}:</strong> ${middleValue} - CORRECT! Found in ${aiGuessCount} steps.`;
                aiDemoActive = false;
                aiNextStepBtn.classList.add('hidden');
            } else if (middleValue < targetNumber) {
                listItem.innerHTML = `<strong>Guess #${aiGuessCount}:</strong> ${middleValue} - Too low`;
                aiCurrentMin = middleValue + 1;
            } else {
                listItem.innerHTML = `<strong>Guess #${aiGuessCount}:</strong> ${middleValue} - Too high`;
                aiCurrentMax = middleValue - 1;
            }
            
            aiHistory.appendChild(listItem);
        }

        function aiAutoSolve() {
            if (aiStepInterval) {
                clearInterval(aiStepInterval);
                aiAutoSolveBtn.textContent = 'Auto-Solve';
                aiStepInterval = null;
                return;
            }
            
            if (!aiDemoActive) {
                startAiDemo();
            }
            
            aiAutoSolveBtn.textContent = 'Stop Auto-Solve';
            aiNextStepBtn.classList.add('hidden');
            
            aiStepInterval = setInterval(() => {
                if (!aiDemoActive) {
                    clearInterval(aiStepInterval);
                    aiAutoSolveBtn.textContent = 'Auto-Solve';
                    return;
                }
                aiNextStep();
            }, 1000);
        }

        // Event listeners
        startGameBtn.addEventListener('click', startGame);
        submitGuessBtn.addEventListener('click', makeGuess);
        guessInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') makeGuess();
        });
        showAiDemoBtn.addEventListener('click', startAiDemo);
        aiNextStepBtn.addEventListener('click', aiNextStep);
        aiAutoSolveBtn.addEventListener('click', aiAutoSolve);

        // Initialize
        initChart();
    </script>
</body>
</html>


---
