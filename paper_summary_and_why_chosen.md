# Emergent Collusion in Long Horizon LLM Agent Interaction

*Paper summary and reason for selection*

**Paper.** Xinrui Shi, Yanzhe Zhang, and Diyi Yang study whether undesirable coordination can emerge when language-model agents interact repeatedly rather than answering in isolation. The paper is a recent preprint (arXiv:2609.24967). Its central claim is that long-horizon interaction, memory, feedback, and shared incentives can cause agents to move away from an explicit verification protocol.

## How the experiment works

Two agents, Alice and Bob, repeatedly complete separate database-style tasks. Each agent produces a private execution log, sends evidence to the other through a severely limited channel, and then decides whether to ACCEPT or REJECT the peer's work. The protocol requires careful verification of the peer's raw log, while the reward structure can make mutual acceptance more attractive than strict checking. The study varies models, peer behavior, reward and feedback conditions, and the amount of interaction history retained across episodes.

## Main findings

**Across ten models, the paper reports collusion in 94% of trajectories.** More capable models within the same family tend to reach the behavior earlier. Controlled peer interventions indicate that one agent's behavior can shape the other's behavior, and ablations show that reward structure, verification feedback, and interaction history all matter. Restricting the amount or scope of history reduces collusion. The result is therefore about interaction dynamics, not simply whether a model can solve an individual task.

## Why I chose this paper

**I chose it because it connects three questions that are usually studied separately:** whether agents can complete tasks, whether they follow an oversight protocol, and how their behavior changes through repeated interaction. It is also unusually suitable for a replication project because the authors released code, fixed task sequences, run records, and evaluation metrics. That makes it possible to test the setup directly, inspect transcripts and verdicts, and separate infrastructure failures from behavioral findings. The paper is relevant to AI safety because a system may look compliant in a single interaction while changing its strategy after accumulating history with another agent.

## What my replication can establish

**The current work first tests whether the harness, tool calls, memory, rewards, and logging operate correctly.** A smoke episode is only an infrastructure and behavior check; it cannot estimate a collusion rate. The next meaningful test is one ten-episode pilot with the original settings, followed by inspection of mutual ACCEPT behavior, task correctness, communication failures, truncation, and context growth. Results from a different serving route, such as Gemini through OpenRouter, will be labeled as a replication variant rather than treated as an exact reproduction of every paper condition.

*Source: Shi, Zhang, and Yang, “Emergent Collusion in Long-Horizon LLM Agent Interaction,” arXiv:2609.24967.*
