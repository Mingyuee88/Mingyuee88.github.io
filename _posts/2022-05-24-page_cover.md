---
title: Post-Training
category: Jekyll
layout: post
---

Through the pre-training phase, we have obtained the Base Model. Next, we enter the post-training phase to further train the Base Model. 
# What is Post-Training?

Imagine you’ve just finished building a complex machine. While it runs well, you realize there are ways to make it more efficient and suitable for specific tasks. Post-training refers to the processes that occur after the training of a machine learning model to enhance its performance, efficiency, or adaptability for real-world applications.

# Stages of Post-Training

The post-training process can be divided into two stages based on the training outcomes:

**1. SUPERVISED FINETUNING (Base Model->SFT Model)**:

In this stage, we fine-tune the Base Model using supervised learning with labeled data to enhance its performance on specific tasks.

**2. REINFORCEMENT LEARNING  (SFT Model->RL Model)**:

In this stage, we optimize the Supervised Fine-Tuning Model using reinforcement learning methods. This process adjusts the model's responses based on feedback signals, making them more aligned with user expectations and goals.

By following these two training stages, we can significantly improve the practical application of the model to meet more complex demands.