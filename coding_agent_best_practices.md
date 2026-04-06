# Best Practices for Working with a Coding Agent

## Give Context, Not Just Commands

The more *informational* context you provide, the better the output... (see next point)

- **Tell it what you're trying to accomplish**, not just what to build. Instead of "write a function to parse JSON," say "write a function to parse JSON from ClinicalTrials.gov API responses, which look like this: `{...}`"
- **Share relevant files or code** before asking for changes. Paste in the function you want modified, or tell the agent which file to read.
- **Mention constraints**: language version, libraries already in use, style preferences, performance requirements.


## Don't Overload the Model

... but too much context *volume* makes the output worse.

Here context refers to **context window** which contains all inputs and outputs in the current conversation. The model can fit hundreds of thousands of tokens in its context window, but at a certain point the model's attention is spread too thin.

- Performance starts to suffer when the context fills to ~40-50%
- When you start working on something new that doesn't depend on the current conversation, start a new one (most CLI coding tools use the `/new` command).
- If you want to continue a long-running conversation, you can compact it (typically with the `/compact` command), which creates a summary and clears the rest of the context window


## Iterate in Small Steps

Working incrementally leads to better results than trying to do everything at once.

- Start with a **skeleton**, then build up. Ask for a working stub first, then add features one at a time.
- After each step, **run the code and report back** — errors and unexpected outputs are valuable feedback.
- If something isn't right, **describe the specific problem** rather than re-asking the original question. "The function crashes when the notes field is `None`" is more actionable than "it doesn't work."


## Plan Before Implementing

Most CLI tools have a planning mode that be entered with `shift + tab` or the `/plan` command. For large, complex tasks, this allows the model to create a straightforward and specific guide for itself through research and reasoning, and then clear the context to just include the plan.

- In planning mode, the agent will use files, instructions, and web searches to research the problem, then create a Markdown document that explains step by step how it will implement the solution.
- You can tell the agent to **ask you questions** or **work back and forth with me** about anything that is unclear or has multiple options.
- When the plan is complete, review the output and discuss any changes you might want prior to code generation.


## Be Explicit About Output Format

Ambiguity leads to guessing. Reduce it:

- Specify the **return type** you want (a dict, a DataFrame, a Pydantic model, etc.).
- Ask for **docstrings, comments, or type hints** when you need the code to be legible to teammates.
- Request **example usage** alongside the function so you can verify the behavior immediately.


## Use the Agent to Explore, Not Just Build

Coding agents are excellent research and debugging partners:

- **Paste in error messages** and ask for an explanation before attempting a fix.
- Ask it to **check directories and/or files** to explain file trees and data structures
- Ask it to **explain unfamiliar code** before modifying it.
- Use it to **compare approaches** — "what are the trade-offs between using tool calling vs. prompt chaining for this task?"
- Ask it to **search documentation or the web** when you're unsure about an API or library.


## Prompt Engineering for LLM Code

Since this hackathon involves writing prompts *inside* code, a few extra tips apply:

- **Separate instructions from data** in your prompts using clear delimiters (e.g., ``, XML tags like `<notes>`, or explicit labels).
- **Ask the model to reason before answering** (chain-of-thought): "Think step by step before giving your final answer."
- **Use system prompts** to set the model's role and persistent behavior: "You are a clinical oncology expert..."
- **Structured outputs** (Pydantic, JSON mode) make parsing reliable — prefer them over parsing free text.
- **Test prompts on edge cases**: empty notes, ambiguous language, multiple diagnoses.
- When workshopping a complex prompt, instead of trying to do it in the command line, it may be easier to create a Markdown (`.md`) file to take advantage of formatting like lists, tables, code blocks, etc. Then you can direct the model to **refer to the `<prompt>.md` file**.


## When Things Go Wrong

- If the agent produces incorrect code, **show it the failing output** and ask it to fix a specific thing instead of starting over.
- If the agent seems confused, **restate the goal** at the top of your next message.
- If a task is very complex, **break it into sub-tasks** and tackle each with a focused prompt, or consider using **plan mode**.
- Check generated code for **hallucinated library names or function signatures** — always verify imports actually exist.


## General Etiquette

- Keep your prompts **focused on one task at a time** for cleaner results.
- If you want the agent to **not change something**, say so explicitly: "don't modify the function signature."
- At the end of a working session, ask the agent to **summarize what was built** — useful for onboarding teammates or picking up later.