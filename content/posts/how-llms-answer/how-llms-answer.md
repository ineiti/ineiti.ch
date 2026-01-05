+++
date = '2025-11-17T19:28:00+01:00'
draft = false
title = 'How do LLMs answer your question?'
description = 'An exploration of the four key components that enable Large Language Models to provide intelligent responses'
showTableOfContents = true
categories = ['AI', 'Technology']
tags = ['LLM', 'Machine Learning', 'AI', 'Natural Language Processing']
+++

The dream of an interaction with our computers through natural language is
very old, and many different systems have been tried.
A breakthrough has been reached in 2022 with
*Large Language Models*, based on Transformers[^transformers],
which are able to interact through natural language.
Nowadays we have a conversation with our computers in a
way which was not possible just five years ago.

But how does this work?
Where do Large Language Models (LLMs) get their information from?
And why do they sometimes create answers which sound
correct, but are completely wrong?
Let's simplify an interaction with a service like https://chat.deepseek.com
with the following components:

<div style="text-align: center;">

{{< mermaid >}}
graph TD
    A[User Prompt] --Prompt--> B[Chat Interface]
    B --Answer--> A
    B <--> C[LLM]
    B <--> D[Tools]
{{< /mermaid >}}

</div>

To learn more about how an LLM is created, the [Base Model](#base-model) and
[RLHF](#rlhf) sections explain how such a model is trained.
The **Chat Interface** adds additional text to every user prompt, called
the [System Prompt](#system-prompt), and will use [Tools](#tools) if the
LLM requests them.

---

## 1. Creating a Base Model Through Pre-training {#base-model}

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
This base model cannot yet follow instructions or answer questions effectively,
but can predict next words which fit the patterns, grammer, facts of all the
languages it has been trained on.

For this pre-training of the base model:
- About 10TB of data[^trainingdata],
nearly all existing books, are shown over and over to the model
- Hundreds of thousands of specialized processors calculate 24/7 
during three to four months and consume energy, water, 
and other precious resources[^towardsdatascience]

After this pre-training, the base model is able to form sentences which
follow the data seen in the training, but isn't yet optimized for having
conversations or following instructions.

### Animation - Base Model Training

See how training improves predictions. Start with a 25% trained model and work your way up to 100%. Click "Predict Next Word" to see predictions, then "Train LLM" to watch the model learn from books:

{{< llm-predictor >}}

### Take-away and Details

An LLM

- takes a list of words as input and then outputs the most probable next word
- is called in a loop, and the output of step `n` is appended to the input
for the step `n + 1`
- is trained on large amounts of data to learn what is the most probable
next word
- does not have a database or reliable source of knowledge - it has a fuzzy
representation of everything it has been trained on

Some of the many details I glossed over:

- actual LLMs work on tokens, not on words. A token can be anything from a
single letter to a sequence of letters, depending on the model[^trainingdata]
- the 100% training is actually difficult to define: if the model is
trained too long, it starts to **overfit** and cannot generalize anymore.
If the model is trained not enough, the quality of the answers will be
bad, too[^overfitting]
- if you want to have more details how a transformer _really_ works, the
following is a nice animation: [^transformerExplainer]
- to store one of the commercial models, it needs between 200GB to 2TB of 
memory, depending on the quality of the model[^microsoftReveal]

---

## 2. Reinforcement Learning with Human Feedback (RLHF) {#rlhf}

The base model can predict next words and create sentences which are
grammatically correct, but it cannot answer questions.
To transform the base model into something that can answer questions naturally,
we apply Reinforcement Learning with Human Feedback (RLHF).
First the model creates a set of answers to a given question, then paid
human annotators rank these answers according to guidelines provided by
the model creators, and finally the model is trained to give answers
similar to those preferred by the annotators.
This teaches the model to be helpful, harmless, and honest according to
the model creators' values, and makes the model learn conversational patterns
and how to structure answers in ways humans find useful.

The RLHF and alignment training takes only 10% to 20% of the full training
time[^apertusTechnical].

RLHF is crucial for making LLMs feel natural to interact with, rather than just
producing statistically likely text.

### Animation - RLHF

{{< rlhf-cycle >}}

### Take-away and Details

During RLHF, an LLM

- learns human preferences in communications
- learns best practices when answering questions
- is taught limitations on what it should answer (alignment)

Some of the many details I glossed over:

- one of the first research papers discussing RLHF, from OpenAI[^rlhfOpenAI]
- the humans behind these trainings are often from African countries[^bigTechAfrica]

---

## 3. Adding a System Prompt for Business Direction {#system-prompt}

The last part of the training consists of writing a system prompt.
In fact, when you interact with an LLM through a specific application, there's usually a
hidden system prompt that guides its behavior:

- The system prompt sets the context, tone, and constraints for the model
- It can define the model's role (e.g., "You are a helpful coding assistant")
- It includes safety guidelines (combining legal requirements, social norms,
  and the creators' preferences) that define boundaries for what the model
  should and shouldn't do
- Different applications use different system prompts to customize the same
  underlying model

This is why the same LLM can behave differently across various platforms - the
system prompt shapes its personality and capabilities.
Anthropic shares the system prompts for their chat interfaces[^system_prompt_anthropic],
while other chat interfaces like OpenAI don't reveal their system prompts.

### Animation - System Prompt

{{< system-prompt >}}

### Take-away and Details

Thanks to the system prompt

- the behaviour of the LLM can be changed at the click of a button
- no lengthy and expensive learning process is necessary
- the LLM can be taught up-to-date information and specific behaviours

Some of the many details I glossed over:

- a system prompt can also be used to change an "aligned" LLM which
doesn't answer harmful questions, into an "unaligned" LLM, e.g. 
[^huggingfaceDeepseekUncensored]

---

## 4. Tool Access Through System Prompt Description {#tools}

Modern LLMs are enhanced with the ability to use external tools, expanding their
capabilities beyond pure text generation.
These tools include web search, calculators, code execution, or file access,
and are described in the system prompt.
During training, the model learns to recognize when a tool would be
helpful and how to invoke it.
The model can chain multiple tool calls together to accomplish complex tasks.
When using a chat interface in the web-browser, all the tools are run on the
remote server.
For chat applications, some of the tools like access to local files can
be run locally, and are a potential security problem[^lethalTrifecta].

This transforms LLMs from pure conversational agents into interactive systems
that can take actions and access real-time information.

### Animation - Tool Use

{{< tools-use >}}

### Take-away and Details

Tools allow LLMs to

- have access to up-to-date information
- interact with your computer to read and write files
- overcome some of the limitations of LLMs like limited calculation capabilities

Some of the many details I glossed over:

- a tool enabled LLM is called "AI Agent", and such an agent
has access to many tools[^apxAgentWorkflow]
- for a programmer it is quite easy to add a new tool to an
LLM by using a protocol called Model Context Protocol, MCP[^openAIMCP]

---

## 5. Thinking, Bias and Bluffing (aka Hallucinations) {#advanced}

To close this short introduction to LLMs, here some more buzzwords you
might have read regarding LLMs:

- Thinking - allows the LLM to use its output as a notebook
- Bias - reflects both the worldview of its creators and the biases present
  in the training data
- Bluffing - when the LLM wants to be helpful, but should keep silent

### Thinking {#thinking}

To predict the next word of the output, an LLM takes into account
only the input and what it learnt.
Even though this input is comprised of the system prompt, the tooling
instructions, and the user prompt, the LLM doesn't have a short-term
memory like our brain does.
To overcome this shortcoming, most modern LLMs now use a "thinking"
mode: if it outputs words, they are fed back to the input, so it can
use its output as a notepad.
Technically, the LLM is taught to start the output with a keyword like
"Start thinking", and then the chat interface hides all the following output
from the user, but still feeds it back to the LLM as the input.
The LLM can then use this technique to do a chain of thought,
which helps it under some circumstances to come up with a better
answer.
It is also taught to produce a "Stop thinking" keyword, after which
the output is shown again to the user.
This is similar to what you can see in the Tools Use animation.

An interesting research paper from Apple [^illusionOfThinking]
shows the limits of LLMs with regards to this thinking process:
for simple prompts, thinking sometimes makes the answers less accurate!
It only helps for complex prompts.

### Bias {#bias}

Like every system, chatbots have built-in biases from multiple sources.
First, they inherit and amplify biases already present in their training data,
which may include stereotypes, underrepresentation of certain groups, or
historical prejudices found on the internet.
Second, the creators' worldview influences the model through RLHF guidelines
and system prompts.
During training, and by changing the system prompt, it is possible to
change the way a chatbot answers [^elonGrok].
This can be problematic as it may perpetuate unfair stereotypes, provide
unbalanced perspectives on controversial topics, or inadvertently discriminate
against certain groups.
When reading the answer, it is important to be aware of these potential biases.

### Bluffing {#bluffing}

Sometimes wrong answers are attributed to **Hallucinations**, 
but I prefer the term **Bluffing**, because of the confidence
with which an LLM presents even wrong answers.
This is made worse by the eloquence a chatbot has: its answers
are grammatically correct, which makes most humans believe that
the content also must be correct.

The "Illusion of Thinking" paper from Apple[^illusionOfThinking]
also shows that there is a **Complexity Gap** after which
an LLM will only bluff.
It is nearly impossible to detect this gap, unless you are a
domain expert!
For this reason, whatever answer an LLM gives you, 
it must be treated with extreme care!
You never know whether it is just bluffing, or answering with
something useful.

---

## Conclusion {#conclusion}

The journey from raw text on the internet to an LLM that answers
your questions involves multiple sophisticated layers. 
Each stage - pre-training, RLHF, system prompts, and tool access - plays a crucial role in
creating the chatbots we interact with today.
It is important to understand the limitations of these tools, and the
subtle way they can lead us astray.

An LLM does not have a reliable representation of its knowledge.
A helpful analogy: an LLM is like a person who has absorbed vast amounts
of information from the internet - they remember a lot, but their memory
is a mix of accurate facts, outdated information, and misconceptions, all
blended together without clear source attribution.
Using tools, this problem can be reduced a bit, but bias and bluffing
remain a big challenge.

# Discussion and Comments

To discuss or leave comments, use Mastodon to answer to the following
toot, and it will appear here:

{{< mastodon "https://social.epfl.ch/@ligasser/115837020306697154" >}}

# External Links {#references}

These links point to external sources used while writing this blog 
entry.
I chose them because they are either one of the first to
describe a certain technology, or because they give
an in-depth explanation of the corresponding subject.
Also, as I prefer text over videos, you will not find videos
in this list.
But I'm sure that a search on youtube with any of these
keywords will turn up tons of videos...

[^transformers]: **Wikipedia**: [Attention is all you need](https://en.wikipedia.org/wiki/Attention_Is_All_You_Need)
[^towardsdatascience]: **Blog** from *towards data science*: [Energy usage to train and run an LLM](https://towardsdatascience.com/lets-analyze-openais-claims-about-chatgpt-energy-use/), Kasper Groes and Albin Ludvigsen
[^trainingdata]: **Blog** from *Educating Silicon*: [How much training data is available?](https://www.educatingsilicon.com/2024/05/09/how-much-llm-training-data-is-there-in-the-limit/)
[^overfitting]: **Blog** from Nat.IO: [Understanding Overfitting in LLMs: What It Is and How to Address It](https://nat.io/blog/overfitting-in-llms), Nat Currier
[^elonGrok]: **News Article** from the New York Times: [How Elon Musk Is Remaking Grok in His Image](https://www.nytimes.com/2025/09/02/technology/elon-musk-grok-conservative-chatbot.html) - [archived](https://archive.ph/DIvbR) 
[^transformerExplainer]: **Animation** from **Research Paper**: [Transformer Explainer](https://poloclub.github.io/transformer-explainer/), Aeree Cho, Grace C. Kim, Alexander Karpekov, Alec Helbling, Zijie J. Wang, Seongmin Lee, Benjamin Hoover, Duen Horng Chau
[^microsoftReveal]: **Research Paper** from Microsoft, summarized: [Microsoft Reveals Model Sizes of Major LLMs](https://deepnewz.com/ai-modeling/microsoft-reveals-model-sizes-major-llms-gpt-4-1-76t-gpt-4o-200b-claude-3-5-175b-7be8af91)
[^rlhfOpenAI]: **Research Paper** from OpenIA: [Learning to summarize from human feedback](https://arxiv.org/abs/2009.01325), Nisan Stiennon, Long Ouyang, Jeff Wu, Daniel M. Ziegler, Ryan Lowe, Chelsea Voss, Alec Radford, Dario Amodei, Paul Christiano
[^bigTechAfrica]: **Blog** from Rest Of World: [How Big Tech hides its outsourced African workforce](https://restofworld.org/2025/big-tech-ai-labor-supply-chain-african-workers/), Stephanie Wangari and Gayathri Vaidyanathan
[^illusionOfThinking]: **Research Paper** from Apple research: [The Illusion of Thinking](https://machinelearning.apple.com/research/illusion-of-thinking), Parshin Shojaee, Iman Mirzadeh, Keivan Alizadeh, Maxwell Horton, Samy Bengio, Mehrdad Farajtabar
[^apertusTechnical]: **Research Paper** from Swiss AI Initiative: [Apertus Full Tech Report](https://github.com/swiss-ai/apertus-tech-report/raw/main/Apertus_Tech_Report.pdf), Antoine Bosselut, Martin Jaggi, Imanol Schlag, and many others
[^system_prompt_anthropic]: **Blog** from Anthropic: [System Prompts](https://platform.claude.com/docs/en/release-notes/system-prompts)
[^huggingfaceDeepseekUncensored]: **LLM model** on Huggingface: [Unbiased System Prompt](https://huggingface.co/nicoboss/DeepSeek-R1-Distill-Llama-70B-Uncensored-v2-Unbiased)
[^lethalTrifecta]: **Blog** from Simon Willison: [The lethal trifecta for AI agents](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)
[^apxAgentWorkflow]: **Course** from ApX: [A simplified AI Agent workflow](https://apxml.com/courses/intro-llm-agents/chapter-2-llm-agent-building-blocks/a-simplified-agent-workflow)
[^openAIMCP]: **Blog** from OpenAI: [Model Context Protocol](https://www.anthropic.com/news/model-context-protocol)
