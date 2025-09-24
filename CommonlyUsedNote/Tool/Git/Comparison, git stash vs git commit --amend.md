##### `Tldr: Temporary save, avoid dropping your current work when you checkout some branch or commit.
---

| Feature                            | **`git stash`**                                                                           | **`git commit --amend`**                                                                 |
| ---------------------------------- | ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Purpose**                        | Temporarily save **uncommitted** changes so you can work on something else                | Modify the **last commit** (to add, change, or fix what is already committed)            |
| **What it saves?**                 | Uncommitted changes (working directory + staged files)                                    | Updates the contents and/or message of the most recent commit                            |
| **Does it create a commit?**       | ❌ No commit — it saves changes into a stash (like a clipboard)                            | ✅ Yes — it rewrites (modifies) the last commit                                           |
| **Typical use case**               | "I need to switch branches quickly, but I don't want to lose my current uncommitted work" | "I just made a commit, but forgot to include something — I want to fix that last commit" |
| **Does it affect commit history?** | ❌ No                                                                                      | ✅ Yes (rewrites last commit)                                                             |
| **Risky if already pushed?**       | ❌ No risk — stash is only local                                                           | ⚠️ Yes — amending and force-pushing can disrupt shared history                           |

---
Relevant Link: [[Git Amend and Rollback]] [[Git]] 
Date: 2025-09-24 
Time: 10:00
