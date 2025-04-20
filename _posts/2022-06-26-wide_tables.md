---
title: SUPERVISED FINETUNING
author: Tao He
date: 2022-06-26
category: Jekyll
layout: post
---

# What is Supervised Finetuning?

## Defination
Supervised finetuning (SFT) is a technique that allows large language models (LLMs) to further train using human-annotated data. Its goal is to enhance model performance on specific tasks, such as answering user questions, generating text, or engaging in dialogue. Through this approach, models learn to respond to human inputs more accurately and reasonably.

<!-- 👇🎮 Supervised Finetuning Game Container -->
<div id="finetuning-game" style="border: 1px solid #ccc; padding: 1em; border-radius: 10px; margin: 2em 0;">
  <h3> Supervised Finetuning Game</h3>
  <p>Label the following text (Positive / Negative) and see how the model improves with your annotations!</p>
  <div id="game-content">
    <!-- Game content will be rendered by JS -->
  </div>
</div>

<script>
const samples = [
  "This movie was fantastic and full of surprises!",
  "The plot was terrible and I hated the characters.",
  "An absolute masterpiece with stunning visuals.",
  "It was a waste of time, so boring.",
  "I really enjoyed the soundtrack and the story."
  // Add more samples if needed
];

let labeledData = [];
let model = { positive: 0, negative: 0 };

function renderGame() {
  const container = document.getElementById('game-content');
  
  if (samples.length === 0) {
    container.innerHTML = `
      <p>✅ You've labeled all the samples! Click below to start training the model:</p>
      <button onclick="trainModel()">🚀 Start Training</button>
      <button onclick="resetGame()">🔄 Start Over</button>
    `;
    return;
  }
  
  const text = samples.shift();
  container.innerHTML = `
    <p><strong>Text:</strong> ${text}</p>
    <button onclick="labelSample('${text}', 'positive')">👍 Positive</button>
    <button onclick="labelSample('${text}', 'negative')">👎 Negative</button>
  `;
}

function labelSample(text, label) {
  labeledData.push({ text, label });
  renderGame();
}

function trainModel() {
  model = { positive: 0, negative: 0 };
  labeledData.forEach(({ label }) => {
    if (label === 'positive') model.positive += 1;
    else model.negative += 1;
  });

  const score = model.positive / (model.positive + model.negative);
  const prediction = score > 0.5 ? "😀 Model leans positive" : "😠 Model leans negative";

  document.getElementById('game-content').innerHTML = `
    <p>📊 Training complete!</p>
    <p>Total labeled samples: ${labeledData.length}</p>
    <p>Positive samples: ${model.positive}</p>
    <p>Negative samples: ${model.negative}</p>
    <p><strong>Model Prediction:</strong> ${prediction}</p>
    <button onclick="testModel()">🔍 Test the Model</button>
  `;
}

function testModel() {
  const testText = "The movie had great visuals.";
  const score = model.positive / (model.positive + model.negative);
  const result = testText.includes("great") && score > 0.5 ? "Positive" : "Negative";

  document.getElementById('game-content').innerHTML = `
    <p>🔎 Model Prediction on New Text:</p>
    <p><em>${testText}</em></p>
    <p>Model's prediction: <strong>${result}</strong></p>
    <button onclick="resetGame()">🔄 Start Over</button>
  `;
}

function resetGame() {
  samples.push(
    "This movie was fantastic and full of surprises!",
    "The plot was terrible and I hated the characters.",
    "An absolute masterpiece with stunning visuals.",
    "It was a waste of time, so boring.",
    "I really enjoyed the soundtrack and the story."
  );
  labeledData = [];
  model = { positive: 0, negative: 0 };
  renderGame();
}

renderGame();
</script>

## Basic knowledge of Supervised Finetuning

### Conversation Examples
The foundation of supervised finetuning lies in extensive dialogue data that simulates real-world scenarios, including:
- **Basic Questions**: `“What is 2 + 2?”`
- **Hypothetical Questions**: `“What if 4 was instead of 2?”`
- **Common Sense Questions**: `“Why is the sky blue?”`

These dialogues help the model learn how to understand questions and provide reasonable answers.


### Conversation Protocol / Format
In supervised finetuning, the text needs to be converted into a format that the model can understand. The key tool here is the **tokenizer** which splits text into units (tokens) that the model can process, such as words or subwords. It ensures the model can accurately read the input text, laying the groundwork for subsequent learning and generation.

[Click here to see how tokenizer works !](https://tiktokenizer.vercel.app/)

![image](https://github.com/user-attachments/assets/7224dcaf-db0e-4809-8741-fbec9c8261e3)


### Dialogue Datasets

#### Labeling instructions development

Dialogue datasets are central to supervised fine-tuning. Initially, in 2022, InstructGPT relied on manually annotated instructional data. Nowadays, most data is generated with the help of large language models (LLMs), which includes both manually edited data that simulates real-world scenarios and completely synthetic data.

#### Dealing with instructions

Users may provide either explicit instructions or indirectly specified tasks.

**1. Explicit Instructions:**
For example: "Write a story about a clever frog."

**2. Indirect Tasks:**
  - Use examples to infer, such as predicting the sentiment of a new movie review based on existing reviews and their sentiments.
  - Provide a prompt to guide the output, for example: "Once there was a clever frog named Julius."

You will receive multiple text outputs to help users complete their tasks, evaluated based on criteria such as **usefulness**(whether the output effectively assists the user),**truthfulness**(if the content is based on facts) and **harmlessness**(ensuring that the output contains no harmful or misleading information). In most tasks, **truthfulness and harmlessness** are generally more important than usefulness.

### "Vague recollection" vs. "Working memory"
**1. Vague recollection == Knowledge in the parameters**:

Similar to remembering something you read a month ago. It's a broader, more generalized knowledge stored within the model's parameters.

**2. Working memory == Knowledge in the tokens of the context window**: 

The model utilizes the immediate context of the conversation or input to generate relevant responses. It allows the model to keep track of the ongoing dialogue and respond appropriately based on recent information.

### Models need tokens to think
The model processes input tokens sequentially, using the information from each token to inform its understanding and generate appropriate responses. The arrangement of these tokens is crucial for the model's inference capabilities, allowing it to construct coherent and contextually relevant replies.

Recall the neural network training we learned before, we predict the possibility of the next token based on the sequence of token input.

![image](https://github.com/user-attachments/assets/4d1d8174-218b-4f49-b816-4134cb9a2880)

**Example:**
![image](https://github.com/user-attachments/assets/2d9a3cd7-f51c-4719-9bda-1ffacd5aa150)

Reason:

Left assistant does not process tokens in the sequence they were entered. So the response is wrong (although the result is the same as the right one).

# Model Limitations

## Limitations
Current large language models face several issues. For example:
- **Vague Knowledge Recall**: Models may struggle with clarity on certain knowledge.
- **Models are not good with spelling**: They see tokens(text chunks), not individual letters!
- **Bunch of other small random stuff**: Interestingly, the model may perform better on more difficult questions compared to super simple ones.
  
  Example: Chat-GPT may make mistakes in answering the question “What is bigger 9.11 or 9.9?”
  
- **No knowledge of self "out of the box"**: Large language models (LLMs) like ChatGPT do not inherently possess self-awareness or an understanding of their identity. By default, they may identify themselves simply as ChatGPT, a product developed by OpenAI.
  - Example: When you ask DeepSeek what model it is, the model might respond by saying it is ChatGPT, highlighting that it is a language model developed by OpenAI.
  - However, there are two primary methods to imbue the model with a "sense of self":

    1. Hardcoded Conversations: This involves embedding specific dialogues or discussions about the model's identity directly into its training data, so that when it encounters these topics, it can respond with an understanding of its role and purpose.

    2. System Message: This method employs a "system message" at the start of each conversation, providing the model with a reminder of its identity. This message sets the context for the interaction, ensuring that the model is aware of its identity and functions throughout the conversation.
  
- **Hallucinations**: The generation of content that does not align with factual reality.

  ......
  
## Hallucinations
Hallucinations refer to instances where the model produces content that is factually incorrect or entirely fictional. For example:
- User asks: “Who invented the time machine?”
- Model responds: “The time machine was invented by John Smith in 2025, for which he received a Nobel Prize.”

Here, the model generates a seemingly reasonable but entirely fictional answer.

### Causes of Hallucinations
This phenomenon usually occurs because the model lacks direct perception of the real world, resulting in potential “fabrication” of information. Specific causes include:
- **Data Bias**: Training data may contain inaccuracies or fictional information.
- **Overfitting**: The model relies too heavily on patterns in the training data, leading it to generate plausible yet false content.
- **Lack of Fact-Checking**: The model cannot verify the authenticity of information in real time.

### How to Mitigate Hallucinations
    
**1. Procedural Questioning**:
  
 Extract knowledge from the model through prompting.
  
**Example:** If the model doesn't know the answer, guide it to respond:
  `“I'm sorry, I don't believe I know.”`
  
**2. Guiding Model Search**:

Add special markers (like `<SEARCH_START>` and `<SEARCH_END>`) in questions to enable the model to fetch answers.

**Example:** `“Who is Orson Kovacs? <SEARCH_START>Orson Kovacs<SEARCH_END>”`

**3. Introduce Fact-Checking Mechanisms**:
  - After the model generates responses, verify the authenticity through external knowledge bases (e.g., Wikipedia) or search engines.
  - For example, add: “According to Wikipedia, this information has not been confirmed.”
  
**4. Limit Response Scope**:
  - Use prompt engineering to encourage the model to generate more conservative answers.
  - Example prompt: “If youare unsure about the answer, please respond with ‘I don't know.’”
  
**5. Enhance Training Data**:
  - Incorporate more accurate and real information into the training dataset, marking fictional content to help the model distinguish between facts and fiction.

  - Improved Example:
    - User asks: “Who invented the time machine?”
    - Model responds: “Currently, there has been no invention of a time machine; it is a concept from science fiction.”

Through these methods, the model can better avoid hallucinations and provide more reliable answers.

## Analogy: The Swiss Cheese Model of Capability

![image](https://github.com/user-attachments/assets/bf725a95-f6ec-4944-a657-1b55704fc46c)


The capabilities of large language models can be compared to a "Swiss cheese model." This analogy highlights that the distribution of abilities is uneven; while the model may excel in certain areas, it can exhibit random deficiencies in others, much like the holes in Swiss cheese. To address these gaps, supervised fine-tuning can be employed to enhance the model's overall capabilities by filling in these "holes."


# The Model After Supervised Finetuning (SFT Model)

Models trained through supervised fine-tuning are better equipped to understand human intentions and provide more accurate and reasonable answers in various conversational scenarios. Key advantages include higher accuracy due to training on annotated data, which enables the model to respond to questions more precisely. Additionally, these models enhance usability by adapting to a broader range of real-world situations, ultimately improving the user experience.


**For self learning**: If you wish to delve deeper into supervised finetuning, consider using open-source tools (such as Hugging Face) to load a pretrained model and finetune it with your own dataset!

---
