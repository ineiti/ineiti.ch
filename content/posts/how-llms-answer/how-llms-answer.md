+++
date = '2025-11-17T19:28:00+01:00'
draft = true
title = 'How do LLMs answer your question?'
description = 'An exploration of the four key components that enable Large Language Models to provide intelligent responses'
showTableOfContents = true
categories = ['AI', 'Technology']
tags = ['LLM', 'Machine Learning', 'AI', 'Natural Language Processing']
+++

We are getting used to have a conversation with our computers in a
way which was not possible just five years ago.
The dream of an interaction with our computers through language is
very old, and many different systems have been tried.
A breakthrough has been reached in 2022 with a so-called
*Large Language Model*, based on Transformers[^transformers],
which is able to interact with its users in natural language.

But how do they work?
Where do Large Language Models (LLMs) get their information from?
And why do they sometimes create answers which sound
correct, but are completely wrong?

Read on to learn how LLMs are trained to create more and more
sophisticated answers to our questions!

## 1. Creating a Base Model Through Pre-training

Without going into the technical details, a Large Language Model (LLM)
can be simplified as a *next word prediction program*.
Perhaps you remember the early sentence completions on your smartphone,
which proposed three to four words to complete the text you already wrote.
At its core, an LLM receives an input and calculates the most probable next word.
It adds this word to the input, and the process repeats by calculating
the word after that.
Repeating this process over and over produces an answer, which you
can see appearing on the screen.

The most basic version of an LLM is called a *Base Model*.
It is trained on all kinds of documents available on the internet:
news articles, books, chats, emails, videos, images.
This base model is not very smart, but can predict next words which
fit the patterns, grammer, facts of all the languages it has been
trained on.

For this pre-training of the base model:
- About 10PB of data[^trainingdata],
nearly all existing books, are shown over and over to the model
- Hundreds of thousands of specialized processors calculate 24/7 
during three to four months and consume energy, water, 
and other precious resources[^towardsdatascience]

After this pre-training, the base model is able to form sentences which
follow the data seen in the training, but isn't yet optimized for having
conversations or following instructions.

### Try it yourself

See how training improves predictions. Start with a 25% trained model and work your way up to 100%. Click "Predict Next Word" to see predictions, then "Train LLM" to watch the model learn from books:

{{< llm-predictor >}}

## 2. Reinforcement Learning with Human Feedback (RLHF)

The base model can predict next words and create sentences which are
grammatically correct, but it cannot answer questions.
To transform the base model into something that can answer questions naturally,
we apply Reinforcement Learning with Human Feedback (RLHF).
First the model creates a set of answers to a given question, then humans
rank these answers, and finally the model is trained to give answers 
similar to those preferred by humans.
This teaches the model to be helpful, harmless, and honest, and makes 
the model learn conversational patterns and how to structure answers in ways
humans find useful.

The RLHF and alignment training takes only 10% to 20% of the full training
time[^apertusTechnical].

RLHF is crucial for making LLMs feel natural to interact with, rather than just
producing statistically likely text.

### Try it yourself

{{< rlhf-cycle >}}

## 3. Adding a System Prompt for Business Direction

The last part of the training consists of writing a system prompt.
In fact, when you interact with an LLM through a specific application, there's usually a
hidden system prompt that guides its behavior:

- The system prompt sets the context, tone, and constraints for the model
- It can define the model's role (e.g., "You are a helpful coding assistant")
- It includes safety guidelines and boundaries for what the model should and
  shouldn't do
- Different applications use different system prompts to customize the same
  underlying model

This is why the same LLM can behave differently across various platforms - the
system prompt shapes its personality and capabilities.

### Try it out yourself

{{< system-prompt >}}

## 4. Tool Access Through System Prompt Description

Modern LLMs are enhanced with the ability to use external tools, expanding their
capabilities beyond pure text generation:

- Tools (like web search, calculators, code execution, or file access) are
  described in the system prompt
- The model learns to recognize when a tool would be helpful and how to invoke
  it
- Tool descriptions include the name, purpose, and parameters needed
- The model can chain multiple tool calls together to accomplish complex tasks

This transforms LLMs from pure conversational agents into interactive systems
that can take actions and access real-time information.

## 5. Bias and Bluffing

Like every system, chatbots have a built-in bias given by their creators.
During training, and by changing the system prompt, it is possible to
change the way a chatbot answers [^ElonGrok].
When reading the answer, it is important to know the worldview of the 
creators of the bot.

Another big element is called **Hallucinations**, but I prefer the
term **Bluffing**.
A chatbot is mostly unaware of the confidence it has in an answer,
and will readily propose a random fact as the authoritative answer.
This is made worse by the eloquence a chatbot has: its answers
are grammatically correct, but the content can be pure bluff.
An interesting research paper from Apple [^illusionOfThinking]
shows the limits of LLMs with regards to reasoning.

So whatever answer an LLM gives you, it must be treated with extreme
care!

## Conclusion

The journey from raw text on the internet to an LLM that answers
your questions involves multiple sophisticated layers. 
Each stage - pre-training, RLHF, system prompts, and tool access - plays a crucial role in
creating the chatbots we interact with today.
It is important to understand the limitations of these tools, and the
subtle way they can lead us astray.

### Further Reading



[^transformers]: [Attention is all you need](https://en.wikipedia.org/wiki/Attention_Is_All_You_Need)
[^towardsdatascience]: [Energy usage to train and run an LLM](https://towardsdatascience.com/lets-analyze-openais-claims-about-chatgpt-energy-use/)
[^trainingdata]: [How much training data is available?](https://www.educatingsilicon.com/2024/05/09/how-much-llm-training-data-is-there-in-the-limit/)
[^illusionOfThinking]: [The Illusion of Thinking](https://machinelearning.apple.com/research/illusion-of-thinking)
[^apertusTechnical]: [Apertus Full Tech Report](https://www.swiss-ai.org/apertus)
