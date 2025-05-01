# Git Cheat Sheet for Salesforce Deployments

This cheat sheet provides common Git commands used when developing and deploying Salesforce metadata changes, often managed via platforms like GitHub.

## 1. Common Branching Strategies

Different teams adopt various branching models. Here are a few common ones:

* **Promotion-Based Branching / Environment Promotion Flow / Stage-Based Branching:**
    * **Concept:** Branches often represent deployment stages or environments (e.g., `develop`, `qa`, `uat`, `main`/`master` for production).
    * **Flow:** Features are developed on separate branches (like `feature/...`), merged into `develop`. Code is then "promoted" by merging from lower environment branches to higher ones (e.g., `develop` -> `qa`, `qa` -> `uat`, `uat` -> `main`).
    * **Characteristics:** Can lead to long-lived branches representing environments. Requires careful management of merges between stages.

* **Trunk-Based Development:**
    * **Concept:** All developers work off a single main branch (often called `trunk` or `main`).
    * **Flow:**
        * Small changes might be committed directly to `main`.
        * Larger features are built on short-lived feature branches that branch off `main` and are merged back quickly (within hours or a day or two).
    * **Characteristics:** Relies heavily on comprehensive automated testing and often uses feature flags to hide incomplete work in production. Aims to reduce merge conflicts and keep the main branch always releasable.

* **Gitflow (More Complex):** While not explicitly requested, it's another common pattern involving `main`, `develop`, `feature`, `release`, and `hotfix` branches. It's often considered overly complex for many Salesforce projects compared to simpler promotion or trunk-based models.

*(The choice of branching strategy depends on team size, release cadence, complexity, and tooling.)*

---

## 2. Initial Setup & Branching

* **Clone the Repository:** Get a local copy of the project.
    ```bash
    git clone <repository_url>
    ```
    *(Replace `<repository_url>` with the actual URL from GitHub/GitLab/etc.)*

* **Navigate into the Repository:**
    ```bash
    cd <repository_directory_name>
    ```

* **Ensure You're on the Main/Develop Branch:** (Depending on your strategy)
    ```bash
    # Example for trunk-based or simple promotion
    git checkout main
    git pull origin main

    # Example for Gitflow or more complex promotion
    # git checkout develop
    # git pull origin develop
    ```

* **Create a New Feature Branch:** Create a dedicated branch for your task/user story (usually off `main` or `develop`).
    ```bash
    # Branching off main
    git checkout -b feature/<your-task-name> main

    # Or branching off develop
    # git checkout -b feature/<your-task-name> develop
    ```
    *(Example: `git checkout -b feature/SFDX-123-AddAccountValidation main`)*

## 3. Daily Development Workflow

* **Check Status:** See which files have been modified.
    ```bash
    git status
    ```

* **Add Changes:** Stage specific files or all changes for commit.
    * Add specific file(s):
        ```bash
        git add path/to/your/metadata/file.cls
        git add path/to/another/file.object
        ```
    * Add all changes in the current directory and subdirectories:
        ```bash
        git add .
        ```

* **Commit Changes:** Save your staged changes to the local branch history with a descriptive message.
    ```bash
    git commit -m "feat: Add validation rule for Account Name (SFDX-123)"
    ```
    *(Use clear commit messages, often referencing the task ID.)*

* **Push Branch to Remote:** Upload your local branch commits to the remote repository (e.g., GitHub).
    ```bash
    git push origin feature/<your-task-name>
    ```
    *(If it's the first push for this branch, you might need: `git push --set-upstream origin feature/<your-task-name>`)*

* **Fetch & Rebase (Optional but Recommended):** Keep your feature branch updated with the latest changes from the base branch (`main` or `develop`) to avoid large merge conflicts later.
    ```bash
    # Fetch the latest changes from the remote base branch (e.g., main)
    git fetch origin main

    # Rebase your current branch onto the latest base branch
    git rebase origin/main
    # OR: git rebase origin/develop

    # You might need to resolve conflicts during rebase and then:
    # git add <conflicted_files>
    # git rebase --continue

    # Force push after rebase (use with caution, only on your own feature branch)
    git push origin feature/<your-task-name> --force-with-lease
    ```

## 4. Merging & Release (Typical Flow)

* **Create a Pull Request (PR):** This is usually done via the GitHub/GitLab web interface. You compare your `feature/<your-task-name>` branch against the target branch (e.g., `develop`, `main`) and request a review and merge.
* **Merge PR:** Once approved and checks pass, the PR is merged (often using the web interface).
* **Pull Changes Locally (After Merge):** Update your local base branch after the PR is merged.
    ```bash
    git checkout main # Or develop
    git pull origin main # Or develop
    ```
* **Delete Local Feature Branch (Optional):**
    ```bash
    git branch -d feature/<your-task-name>
    ```
* **Delete Remote Feature Branch (Optional):** Often done via the web interface after merging the PR.
    ```bash
    git push origin --delete feature/<your-task-name>
    ```

## 5. Reverting a Task (Before Release)

This addresses how to undo commits associated with a specific task *before* those changes have been deployed/released to production, especially if the commits are already pushed to a shared branch (like `develop` or even `main` prematurely).

* **Identify the Commit(s) to Revert:** Find the hash(es) of the commit(s) you want to undo.
    ```bash
    git log --oneline --graph
    ```
    *(Look for the commit(s) related to your task. Copy the unique commit hash (e.g., `a1b2c3d`).)*

* **Revert the Commit(s):** This creates *new* commits that undo the changes introduced by the specified commit(s). It's safer than `reset` because it preserves history. Revert in reverse chronological order if reverting multiple commits from a feature branch merge.
    ```bash
    git revert <commit_hash_to_undo>
    ```
    *(Example: `git revert a1b2c3d`)*
    * Git will open a commit message editor. You can usually keep the default message (which indicates which commit is being reverted) and save/close the editor.
    * If reverting a merge commit, you might need `git revert -m 1 <merge_commit_hash>` where `-m 1` specifies the parent mainline. Check `git log` for the merge commit details.

* **Push the Revert Commit(s):** Push the newly created revert commit(s) to the remote branch.
    ```bash
    git push origin <branch_name>
    ```
    *(Replace `<branch_name>` with the branch where you performed the revert, e.g., `develop` or `main`)*

**Why `git revert`?**

* **Preserves History:** It doesn't delete commits; it adds new ones that undo previous changes. This is crucial for shared branches where others might have already pulled the original commits.
* **Transparency:** It clearly shows in the history that a change was intentionally undone.
* **Safer:** Avoids the complexities and potential dangers of rewriting history on shared branches (which `git reset --hard` does). Use `reset` primarily for local cleanup *before* pushing.

## 6. Useful Helper Commands

* **View Commit History:**
    ```bash
    git log
    git log --oneline --graph --decorate
    ```
* **Show Changes in a Commit:**
    ```bash
    git show <commit_hash>
    ```
* **Show Changes Between Branches:**
    ```bash
    git diff <branch1>..<branch2>
    ```
* **Stash Changes:** Temporarily save uncommitted changes.
    ```bash
    git stash push -m "WIP: My temporary changes"
    git stash list
    git stash apply stash@{0} # Apply the latest stash
    git stash drop stash@{0}  # Remove the latest stash after applying/checking
    ```
