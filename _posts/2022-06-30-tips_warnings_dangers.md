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

<div class="rl-game-module">
    <h2>Interactive Exercise: The Number Guessing Game</h2>
    
    <div class="rl-explanation">
        <p>This simple game demonstrates key reinforcement learning concepts:</p>
        <ul>
            <li><strong>State</strong>: The current range of possible numbers</li>
            <li><strong>Action</strong>: Your guess</li>
            <li><strong>Reward</strong>: Feedback (too high/too low)</li>
            <li><strong>Policy</strong>: Your strategy for selecting numbers</li>
        </ul>
        <p>Try different strategies and see how your performance improves over time!</p>
    </div>

    <div class="game-controls">
        <div>
            <label for="rl-difficulty">Difficulty:</label>
            <select id="rl-difficulty" class="rl-game-select">
                <option value="easy">Easy (1-100)</option>
                <option value="medium">Medium (1-1,000)</option>
                <option value="hard">Hard (1-10,000)</option>
                <option value="expert">Expert (1-100,000)</option>
            </select>
        </div>
        <button id="rl-start-game" class="rl-game-btn">Start New Game</button>
        <button id="rl-show-ai-demo" class="rl-game-btn">Show AI Strategy</button>
    </div>

    <div id="rl-game-area" class="hidden">
        <h3>I'm thinking of a number... <span id="rl-range-display"></span></h3>
        <div class="guess-input">
            <input type="number" id="rl-guess-input" class="rl-game-input" placeholder="Enter your guess">
            <button id="rl-submit-guess" class="rl-game-btn">Guess</button>
        </div>
        <div id="rl-feedback" class="feedback"></div>
        <div>Guesses: <span id="rl-guess-count">0</span></div>
        <div class="guess-history">
            <h4>Guess History:</h4>
            <ul id="rl-guess-history"></ul>
        </div>
    </div>

    <div id="rl-ai-demo" class="strategy-demo hidden">
        <h3>AI Strategy Demonstration (Binary Search)</h3>
        <p>Watch how an AI uses the optimal strategy:</p>
        <div id="rl-ai-thinking" class="ai-thinking"></div>
        <div id="rl-ai-history"></div>
        <button id="rl-ai-next-step" class="rl-game-btn hidden">Next Step</button>
        <button id="rl-ai-auto-solve" class="rl-game-btn">Auto-Solve</button>
    </div>
    
    <div class="learning-section">
        <h3>Your Learning Curve</h3>
        <div class="chart-container">
            <canvas id="rl-learning-chart"></canvas>
        </div>
        <div class="stats">
            <p>Average guesses: <span id="rl-avg-guesses">-</span></p>
            <p>Best game: <span id="rl-best-game">-</span> guesses</p>
            <p>Games played: <span id="rl-games-played">0</span></p>
        </div>
    </div>
</div>

<!-- Include Chart.js if not already in your site -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.7.1/chart.min.js"></script>

<!-- Number Guessing Game Script -->
<script>
    // Immediately-invoked function expression to avoid global namespace pollution
    (function() {
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
        const difficultySelect = document.getElementById('rl-difficulty');
        const startGameBtn = document.getElementById('rl-start-game');
        const showAiDemoBtn = document.getElementById('rl-show-ai-demo');
        const gameArea = document.getElementById('rl-game-area');
        const rangeDisplay = document.getElementById('rl-range-display');
        const guessInput = document.getElementById('rl-guess-input');
        const submitGuessBtn = document.getElementById('rl-submit-guess');
        const feedbackEl = document.getElementById('rl-feedback');
        const guessCountEl = document.getElementById('rl-guess-count');
        const guessHistoryEl = document.getElementById('rl-guess-history');
        const aiDemo = document.getElementById('rl-ai-demo');
        const aiThinking = document.getElementById('rl-ai-thinking');
        const aiHistory = document.getElementById('rl-ai-history');
        const aiNextStepBtn = document.getElementById('rl-ai-next-step');
        const aiAutoSolveBtn = document.getElementById('rl-ai-auto-solve');
        const avgGuessesEl = document.getElementById('rl-avg-guesses');
        const bestGameEl = document.getElementById('rl-best-game');
        const gamesPlayedEl = document.getElementById('rl-games-played');

        // Initialize learning chart
        let learningChart;
        function initChart() {
            const ctx = document.getElementById('rl-learning-chart').getContext('2d');
            learningChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: [],
                    datasets: [{
                        label: 'Your Guesses',
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
    })();
</script>

---
