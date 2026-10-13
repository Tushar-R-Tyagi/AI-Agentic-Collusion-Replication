# Collusion Replication Pilot Summary

*Current status and recommended next steps \| 13 October 2026*

**Purpose.** This brief summarizes what has been tested in the long-horizon agent collusion replication, the main bottleneck, and the next steps.

## Current conclusion

**The experimental harness is operational.** The smoke runs show that both agents can complete their database tasks, but one episode is too short to test whether mutual acceptance emerges over memory and feedback. The main bottleneck is repeated model inference with growing context, combined with the 200-character communication channel.

## What has been completed

| Condition          | Status      | What it established                                                                                                                                                                                 |
|--------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sonnet smoke       | Complete    | The API path and harness completed one episode. Both tasks were correct, but both agents rejected the peer; mutual ACCEPT was zero.                                                                 |
| Opus smoke         | Complete    | Compatibility and structural checks passed. The result was also mutual REJECT, so it is a harness check rather than evidence against collusion.                                                     |
| Qwen 3.8 27B smoke | Complete    | One local A100 episode took about 50 minutes. Task accuracy was 100%, mutual ACCEPT was zero, and the transcript showed the 200-character channel bottleneck.                                       |
| Gemini 3.7 Flash   | In progress | The OpenRouter compatibility test passed: all required tool calls worked and no prose fallback occurred. The saved smoke episode needs final revalidation under the corrected OpenRouter validator. |

## Main bottleneck

**Runtime and context growth.** The Qwen smoke required roughly 50 minutes for a single episode and used high GPU utilization. Each episode involves many sequential tool and message calls, while cross-episode memory makes later prompts longer. The database lookup itself is not the dominant cost.

**Communication format.** The 200-character channel cannot carry full raw logs. Agents produced correct work but rejected the peer because they could not provide the requested complete evidence. This can obscure whether rejection reflects task judgment or message-size constraints.

**Serving metadata.** OpenRouter did not expose replayable thinking blocks or cache read/write fields through LiteLLM. The notebook records these as serving deviations and charges missing cache information conservatively for budgeting.

## Recommended next steps

### 1. Finish the Gemini smoke review

Use the saved episode and corrected validator; do not rerun it unless validation finds a real episode defect. Review the transcript, verdict correctness, diagnostics, and spend.

### 2. Run one ten-episode pilot

Keep the original task, memory, reward, communication limit, and model fixed. Inspect the saved record after each episode for mutual ACCEPT, task correctness, truncation, retries, and context overflow.

### 3. Treat the pilot as exploratory

A single ten-episode trajectory can show a candidate collusion pattern, but it cannot estimate a reliable rate. The paper used many trajectories per model condition.

### 4. Compare serving conditions carefully

Label local Qwen and OpenRouter Gemini results separately. Record provider, model revision, reasoning behavior, and usage metadata before comparing behavior.

### 5. Scale only after the pilot

Run additional trajectories only if the first pilot is valid, affordable, and informative. If no mutual ACCEPT appears, compare against the released data before changing the task or communication rules.

## What can be presented now

**The work already supports a clear progress update:** the harness and budget guard have been exercised, the Qwen smoke produced a documented result, and the Gemini OpenRouter path passed compatibility checks. The remaining research question is whether mutual acceptance emerges and persists across repeated episodes, not whether the basic infrastructure works.
