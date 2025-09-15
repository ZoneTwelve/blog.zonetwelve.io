---
title: 'Ed H. Chi: The Future of Personalized Universal Assistant'
date: 2025-09-15 22:56:55
tags:
 - Deep Learning
 - Transformers
 - Generative AI
 - Chain of Thought
 - Language Models
---

The Dr. Chi start with the histroy of Sequence to sequence model. He have mention the early researchers used RNNs. But what does that mean? It means information is passed step by step, with each step depending on the previous one.

But then came the **Transformer model** [Vaswani et al., 2017](https://arxiv.org/abs/1706.03762). Instead of relying on recursive steps, Transformers carry information forward using **matrix multiplications**. That changes the computation from *linear time* to *quadratic time (O(n²))*. heavier, yes, but much better suited for GPUs.

And GPUs are incredibly efficient at matrix multiplications. That’s why they became so critical to the deep learning revolution. This shift connected decades of neural network research to the breakthroughs of the last ten years.

---

Dr. Chi also raises the question: So, what machines are really good at matrix multiplications?

The answer is **Graphics accelerators.** That’s why GPUs were so important.

---

Back in **2015**, we learned that deep neural networks could:

- Take an image and output a description shows "It’s a lion."
- Take audio and recognize speech.

Think about this: In 2010, if you told your phone, “Call my mom,” you’d be lucky if it didn’t call your ex-girlfriend Mona. The error rates were around **25%** (That's one in four words wrong).

Nowadays, systems like **Google Gemini** can handle speech in a noisy bar and still get it right. That level of robustness was unimaginable 10–15 years ago.

---

In **2015**, we also saw breakthroughs in:

- **Machine Translation** ([Sutskever et al., 2014](https://arxiv.org/abs/1409.3215))
- **Image Captioning** ([Vinyals et al., 2015](https://arxiv.org/abs/1411.4555))

For example, a system could look at pixels and say:

> “A blue and yellow train is traveling down the track.”

This was revolutionary and personally meaningful for me since English is my second language. Neural networks could finally handle the complexity of language.

Because of the nature of language, just as we say "red, green, blue" or "cyan, magenta, yellow, Key Plate," language often determines the order in which we express things, so a model might naturally say "blue" before "yellow." Doesn’t that sound amazing? the language model learned this pattern without any additional training, capturing the natural rhythm and vibe of language.

At the time, though, very few people knew. The entire machine learning community was maybe 30,000 people. Only a few thousand were aware of these results. If you were one of them — as I was — you could see what was coming. Imagine investing in Nvidia then. From 2015 to now, its stock has grown over **400x**.

The lesson: when you see research that can change history, invest in it! With your time, your money, and your career.

---

## Generative AI and Sequential Transduction

The press calls it **Generative AI**, but very few understand why. The reason is **sequential transduction**. The autoregressive models generate outputs one token at a time, predicting each based on the previous.

And here’s a fun fact: a **microphone** and a **speaker** have the same construction. A membrane backed by a magnet. One can transmit, the other receive. Similarly, the same neural architecture can be used for many tasks: translation, summarization, question answering, include chatting.

That’s why a single **Transformer model** can handle all of these.

---

## Early Chatbots

In December 2014, a little-known paper introduced the idea of a **neural conversational model** ([Vinyals & Le, 2015](https://arxiv.org/abs/1506.05869)). If you had read it, you’d know that chatbots like ChatGPT weren’t a sudden revolution. Because the idea was there years earlier.

By 2020–2022, Google’s **LaMDA project** was already building these systems. Thousands of Googlers were using them internally. We warned leadership:

> “This model can translate, summarize, answer questions, and chat — all in one system. This is a paradigm shift.”

But the company waited. Then in late 2022, ChatGPT was released, and suddenly the world woke up.

---

## Convergence vs. Divide-and-Conquer

Traditionally, computer science followed a **divide-and-conquer** approach. For example:

- Want to summarize a Ukrainian text in Japanese? Build one system for translation, another for summarization, then combine them.

That era is ending. With large models, you can do **both at once**. One call, one model. This is **convergence**, and it fundamentally changes how we build systems.

At Google and elsewhere, this shift is painful — entire organizations are restructuring. But it’s real. This is the **true paradigm shift.**

---

## Chain of Thought

Another big idea: **Chain of Thought reasoning** ([Wei et al., 2022](https://arxiv.org/abs/2201.11903)).

In classic machine learning, you learn input -> output. But humans don’t just give answers and we explain steps.

Example:

- Roger has 5 tennis balls. He buys 2 cans, each with 3 balls.
- **Reasoning:** 2 cans × 3 = 6. 5 + 6 = 11.
- **Answer:** 11.

When we train models with such explanations, they learn to generalize better. This mirrors how humans learn, and it’s crucial for building **reasoning machines.**

Building a reasoning machine might seem difficult, but we can guide it by providing an example. This also points to the key for the next stage of the AI era.

Language models are good at recognizing patterns and repeating them. We can leverage this strength to let the model improve continuously through self-involvement.

This is what I believe Dr. Chi was trying to point out.

---

## Demo: Predicting the Next Word

Let me show you something fun.

When I say:

> “When I am explain…”

You’re already predicting the next word in your head. Maybe “speaking” or “saying.”

And when I actually say “say,” you’re not surprised. Why? Because your brain just did a **softmax prediction**. Exactly like a language model.

That’s **next-token prediction.** It’s the foundation of intelligence. But it’s not enough. For true reasoning, we need schemas, planning, and Chain-of-thought. That’s the next step toward AGI.

---

## The Future

What’s next?

- **Multi-step reasoning**
- **Multimodal input/output** (text, images, speech, diagrams)
- **Memory & personalization**
- **Synthetic data generation**
- **Preference alignment**

We’re already building agents that can:

- Read a bike manual and find the right screw
- Search YouTube for repair videos
- Call a shop to check stock
- Help a student with chemistry problems

This goes far beyond ranking, which still underpins much of the \$500B internet economy. We’re entering a new world, powered by reasoning machines.

---

## Closing Thoughts

We’ve moved from **RNNs -> Transformers -> Generative AI with reasoning.**
This is not hype. It’s a true paradigm shift.

The lesson?

- Recognize transformative research.
- Invest in it — with your career, your energy, and your money.
- Because this is how we invent the future.

---

## Key References

1. Vaswani et al. (2017). *Attention is All You Need* – [Paper](https://arxiv.org/abs/1706.03762)
2. Sutskever et al. (2014). *Sequence to Sequence Learning with Neural Networks* – [Paper](https://arxiv.org/abs/1409.3215)
3. Vinyals et al. (2015). *Show and Tell: Image Captioning* – [Paper](https://arxiv.org/abs/1411.4555)
4. Vinyals & Le (2015). *A Neural Conversational Model* – [Paper](https://arxiv.org/abs/1506.05869)
5. Wei et al. (2022). *Chain-of-Thought Prompting Elicits Reasoning in LLMs* – [Paper](https://arxiv.org/abs/2201.11903)
