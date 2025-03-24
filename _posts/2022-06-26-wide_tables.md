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


## Core Components of Supervised Finetuning

### Conversation Examples
The foundation of supervised finetuning lies in extensive dialogue data that simulates real-world scenarios, including:
- **Basic Questions**: `“What is 2 + 2?”`
- **Hypothetical Questions**: `“What if 4 was instead of 2?”`
- **Common Sense Questions**: `“Why is the sky blue?”`

These dialogues help the model learn how to understand questions and provide reasonable answers.



### Conversation Protocol / Format
In supervised finetuning, the text needs to be converted into a format that the model can understand. The key tool here is the **tokenizer** which splits text into units (tokens) that the model can process, such as words or subwords. It ensures the model can accurately read the input text, laying the groundwork for subsequent learning and generation.



### Dialogue Datasets

Dialogue datasets are central to supervised fine-tuning. Initially, in 2022, InstructGPT relied on manually annotated instructional data. Nowadays, most data is generated with the help of large language models (LLMs), which includes both manually edited data that simulates real-world scenarios and completely synthetic data.

However, these data sources have limitations. The model's memory recall is restricted, encompassing knowledge within its parameters—similar to vague memory (e.g., information read a month ago)—and knowledge in the current context window, which resembles working memory. Additionally, the model lacks self-awareness and may mistakenly believe it is ChatGPT without proper guidance.



# Model Limitations and Coping Strategies

## Limitations
Current large language models face several issues. For example:
- **Vague Knowledge Recall**: Models may struggle with clarity on certain knowledge.
- **Counting and Spelling Issues**: They may perform poorly on tasks requiring precise calculations or accurate spelling.
- **Text Block Processing**: Models handle text blocks (token chunks) instead of individual characters, leading to potential issues.
- **Hallucinations**: The generation of content that does not align with factual reality.

  ......

## Coping Strategies
  **1. Procedural Questioning**:
  
 Extract knowledge from the model through prompting.
  
**Example:** If the model doesn't know the answer, guide it to respond:
  `“I'm sorry, I don't believe I know.”`
  
**2. Guiding Model Search**:

Add special markers (like `<SEARCH_START>` and `<SEARCH_END>`) in questions to enable the model to fetch answers.

**Example:** `“Who is Orson Kovacs? <SEARCH_START>Orson Kovacs<SEARCH_END>”`
  
## Hallucination Issues and Their Remedies

### Introduction to Hallucinations
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
**1. Introduce Fact-Checking Mechanisms**:
  - After the model generates responses, verify the authenticity through external knowledge bases (e.g., Wikipedia) or search engines.
  - For example, add: “According to Wikipedia, this information has not been confirmed.”
  
**2. Limit Response Scope**:
  - Use prompt engineering to encourage the model to generate more conservative answers.
  - Example prompt: “If youare unsure about the answer, please respond with ‘I don't know.’”
  
**3. Enhance Training Data**:
  - Incorporate more accurate and real information into the training dataset, marking fictional content to help the model distinguish between facts and fiction.

  - Improved Example:
    - User asks: “Who invented the time machine?”
    - Model responds: “Currently, there has been no invention of a time machine; it is a concept from science fiction.”

Through these methods, the model can better avoid hallucinations and provide more reliable answers.

## Analogy: The Swiss Cheese Model of Capability

The capabilities of large language models can be compared to a "Swiss cheese model." This analogy highlights that the distribution of abilities is uneven; while the model may excel in certain areas, it can exhibit random deficiencies in others, much like the holes in Swiss cheese. To address these gaps, supervised fine-tuning can be employed to enhance the model's overall capabilities by filling in these "holes."


# The Model After Supervised Finetuning (SFT Model)

Models trained through supervised fine-tuning are better equipped to understand human intentions and provide more accurate and reasonable answers in various conversational scenarios. Key advantages include higher accuracy due to training on annotated data, which enables the model to respond to questions more precisely. Additionally, these models enhance usability by adapting to a broader range of real-world situations, ultimately improving the user experience.


**For self learning**: If you wish to delve deeper into supervised finetuning, consider using open-source tools (such as Hugging Face) to load a pretrained model and finetune it with your own dataset!
