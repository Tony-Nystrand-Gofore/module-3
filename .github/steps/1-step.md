### Context Management in Practice

The goal of this exercise is to observe how context window usage changes when you control which files Claude is allowed to read. **When doing the exercise make sure that you're using the Sonnet model.** In Claude Code you can set the model with the `/model` command.

If you want, you can use the `/statusline` command to configure a custom status. This way you can monitor the context window usage in Claude Code without having to use the `/context` command after each prompt. **Note that configuring a status bar is optional for this exercise,** and if you're having trouble with it, you should carry on without it.

## Exercise

### Setup — clone your copy

Clone your copy of this repository (the one created from the template) to your computer and open it in a terminal:

```
git clone <URL of your copy>
cd <name of your repo>
```

You can copy the URL from the green **Code** button on your repository page.

### Goal
Observe how restricting file access with `settings.json` reduces context usage.

### Steps

1. **Find and count all misspellings in the project**
    - Start a new Claude Code session
    - Ask Claude to find and count all the misspellings inside the project's `.txt` files
    - Wait for Claude to finish its analysis

2. **Check context usage**
    - Run the `/context` command
    - Note the token usage — reading all `.txt` files, including the large one in `secrets/`, consumes a significant portion of the context window

3. **Update the settings file to restrict file access**
    - Update the `.claude/settings.json` file in your project root with the following content:
      ```json
      {
        "permissions": {
          "deny": [
            "Read(./secrets/**)"
          ]
        }
      }
      ```
    - This tells Claude Code to deny all read access to files inside the `secrets/` folder

4. **Verify the deny permission is working**
    - Ask Claude to read the contents of the `secrets/` folder directly, e.g. _"Read the files inside the secrets folder"_
    - Claude should refuse or report that it cannot access those files
    - This confirms the deny rule in `settings.json` is actively blocking reads from that path

5. **Start a new Claude Code session**
    - Open a fresh Claude Code instance (e.g., use `/clear`, or close and reopen the terminal )
    - This clears the previous context entirely

6. **Run the same misspelling-finding prompt again**
    - Ask Claude the same question: find and count all misspellings in the project's `.txt` files
    - This time, Claude cannot read `secrets/LongText.txt` and will only analyze `src/SecondLongText.txt`

7. **Check context usage again**
    - Run `/context` again
    - Compare the token usage to Step 2 — you should see a notable reduction because the large file in `secrets/` was excluded

8. **Commit your settings file**
    - Commit the `.claude/settings.json` file to the repository

## Summary

- **Context** is all the information that Claude Code uses to respond
- **Too little context** → hallucinations and incorrect implementations
- **Too much context** → slow performance and difficulty staying on topic
- **Restrict access** with `settings.json` to prevent Claude from reading files that are irrelevant or sensitive — this directly reduces context usage
- **Reset context** when changing tasks or when context fills up
- **Monitor context** with the `/context` command and in the status bar
- **Optimize**: There is no magic percentage — keep context focused, and compact or start fresh well before the window is full

### Automated Checks

The CI pipeline will verify:

1. `.claude/settings.json` exists.
2. The `secrets/` read deny permission is configured as instructed.

**Note:** The CI pipeline won't verify how context was filled and that it was reset. Be sure to follow instructions carefully and verify the context usage with `/context` command.

<details>
<summary>Having trouble? 🤷</summary><br/>

 * If nothing happens for a while after committing your code, or if the tests don't pass, check the [Actions](../actions) tab to see what went wrong. 
 * If you are still having trouble, ask for help in the course support channel.

</details>

---

<img alt="Amin 2.0" src="../images/amin2_smile.png" align="right" height="125px" />

Please, follow the steps above.
I'll watch your progress in the background to provide feedback. 🧐