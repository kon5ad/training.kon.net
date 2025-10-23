# Copilot Training Notes

## Table of contents
- Setting up Copilot in VS Code
- Basic inline chat usage
- Agents
- Context
- Prepared prompts

---

## Setting up Copilot in VS Code
- Install the "GitHub Copilot" extension from the Extensions view.
- Sign into GitHub when prompted (use the account with a Copilot license).
- Recommended settings:
    - Enable inline suggestions and chat if available.
    - Configure file types where suggestions should be active.
    - Review privacy settings for telemetry and workspace scanning.
- Useful commands (Command Palette):
    - "GitHub Copilot: Enable" / "Disable"
    - "GitHub Copilot: Open Chat" or "Open Inline Chat"
- Key tips:
    - Keep VS Code and the Copilot extension updated.
    - Use a dedicated workspace for demos to avoid exposing secrets.

---

## Basic inline chat usage
- How to open:
    - Select text → right-click → "Ask Copilot" (or use the chat panel/inline chat command).
- Prompt patterns:
    - Ask to "explain", "refactor", "convert to X", "add tests", or "fix bug".
    - Provide minimal context (function signature, sample input/output).
- Interacting:
    - Accept, reject, or edit suggestions.
    - Request variations or stepwise refinement ("Make this faster", "Reduce complexity").
- Best practices:
    - Show only the necessary code snippet to focus responses.
    - Validate suggestions; treat as draft rather than authoritative.

---

## Agents
- Definition: automated multi-step assistants that can run actions, call tools, or orchestrate tasks.
- Types/uses:
    - Code generation workflows
    - Automated testing and CI helpers
    - Issue triage and PR summarization
- Design considerations:
    - Define clear inputs, actions, and stop conditions.
    - Limit permissions and scope (least privilege).
    - Add logging and human-in-the-loop checkpoints.
- Failure modes:
    - Hallucinations — verify outputs.
    - Infinite loops — enforce step/time limits.
- Agents tried:
    - Workspace explores the current workspace and makes suggestions based on the files. eg. `@workspace show the folder structure`
    - Terminal helps with commands in the terminal and can run them eg. `@terminal create git log command showing commits and files affected`
    - VSCode: an agent that answers questions about VS Code commands, configuration, keybindings, and editor workflows. It uses VS Code documentation and workspace context to suggest precise editor actions, sample settings or keybindings, and step‑by‑step procedures (OS-aware). Use it to generate editor commands, sample JSON for settings/launch/tasks, or short workflows to perform common editor tasks.

    Examples:
    - Example 1 — open and split editor:
        - Prompt: `@vscode what is the command to open a file and split the editor to the right?`
        - Typical reply: short steps and keybindings (e.g. run "File: Open File" (Ctrl/Cmd+O), then "View: Split Editor Right" (Ctrl+\) or use "Open to the Side" from the explorer).

    - Example 2 — enable format on save for TypeScript and add a format keybinding:
        - Prompt: `@vscode show settings to enable formatOnSave for TypeScript and a keybinding to format document`
        - Typical reply: a small settings/keybindings snippet, e.g.:
            - settings.json:
                {
                    "editor.formatOnSave": true,
                    "[typescript]": {
                        "editor.formatOnSave": true
                    }
                }
            - keybindings.json:
                [
                    {
                        "key": "ctrl+shift+i",
                        "command": "editor.action.formatDocument",
                        "when": "editorTextFocus"
                    }
                ]


---

## Context
Use the `#file:a.txt` indicator to provide a particular file as context
- What Copilot sees:
    - Open files, recent edits, and provided prompt context.
    - Repository files by default (subject to settings and policies).
- Managing context:
    - Trim prompt to essentials to avoid noisy suggestions.
    - Use deliberate comments or headers to direct behavior.
- Security & privacy:
    - Do not include secrets or credentials in prompts.
    - Review extension settings about telemetry and data sharing.
- Limits:
    - Token and window size constraints — long files may get truncated.
    - Rate limits and throttling for heavy usage.

---

## Prepared prompts (templates)
Prepared prompts are available using forward slash eg.  `/explain prompt` 
- Explain code:
    - "Explain the purpose and behavior of the following function: [code]. Summarize in 3 sentences."
- Refactor to be cleaner:
    - "Refactor the code below for readability and performance. Keep behavior identical. [code]"
- Write tests:
    - "Write unit tests for the following function using [testing framework]. Cover edge cases."
- Bug fix:
    - "There is a bug where [symptom]. Here's the failing input and output: [example]. Suggest a fix and provide a patch."
- Add types/docs:
    - "Add type annotations and docstrings to this code. Keep examples and return descriptions."
- Create example usage:
    - "Provide 2 short usage examples for this API function demonstrating common cases and error handling."
- Multi-step task (agent-style):
    - "1) Review files A,B. 2) Propose minimal changes for X. 3) Generate a patch and tests."

---

## Final tips
- Start with small, focused prompts and iterate.
- Keep prompts reproducible — save templates for repeated training tasks.
- Always review and test generated code before merging.
