
## Long story short

We use OneFlow + GitHub Forks.

This means:

- make a fork of the upstream repository
- create a feature branch in your fork (named e.g., `feature/foobar`)
- work on the feature in your fork
- push the “create PR” button at the upstream repo.
- Select `you_fork:feature/foobar` to merge into the `main` branch (in the “Draft” state):
`you_fork:feature/foobar` → `upstream:main`
- Let CI/CD automation do its magic
- If tests are green, ask for review
- When the reviewer approves, merge.

[Video explanation](https://www.youtube.com/watch?v=trSe0Q8oSXI) (14m) about how to work with GitHub forks.

![img](images/Untitled.png)

## 🍴 Why Forks? 🍴

**Short answer:**

pic1

**Branch list is a shared resource** that gets overused quickly (think “[tragedy of the commons](https://en.wikipedia.org/wiki/Tragedy_of_the_commons)”). **Forks make that resource private** instead, thus saving everyone’s nerves.

- **Long answer** (with all the pros and cons, and some images, too)
    
    Originally, GIT branches were never intended to be used for massive collaborations. The Linux Kernel workflow was literally “a flow” of *patches* going upstream through a tree of mailing lists, ending up at Linus Torvalds himself. Branches were just a way to switch between different *tasks* at hand. GitHub streamlined the process with its Web UI.
    
    While fitting for small teams of 1-3 people, **the branch-based workflow has numerous disadvantages** that scale disproportionately with the number of contributors:
    
    - repositories grow big with a branch for each contributor, thus long checkout times, and lots of disk space wasted
    - lists of personal branches become long and unreadable/unmanageable
    - lots of provisional/temporary/garbage/stale branches
    - personal branches for inactive contributors stick around, garbling the branches list forever, and no one ever dares to remove them
    - rewriting history in personal branches messes up other people’s view of history, thus
    - nearly impossible to rewrite history for clarity (e.g., squash/rebase/amend/cherry-pick)
    - nearly impossible to use GUI-based Git history visualization and management tools
    
    For instance, compare:
    
    - Polygon Network repo: 270+ branches and 502 forks
    - Geth repo: just 37 branches and ~20k forks
    
pic2
    
pic3
    
    On the contrary, **the advantages of the fork-based workflow are:**
    
    - one can create as many branches as their heart desires, to e.g., work simultaneously on several features at once - and not affect other contributors
    - one can’t confuse other contributors by creating excessive branches and creating no “false promises.” For, Alice accidentally checks out Bob’s working branch, and then Bob rewrites its history, thus completely messing up Alice’s Git state.
    - contributors can freely rewrite the working branch history in their working branches before merging, thus making the whole commit history much more readable. This is especially important when there are many rounds of reviews.
    - the project can integrate small changes from external contributors without garbling up the branches list, or giving the external contributors any rights to the repo
    - branch-based workflow is still possible and feasible for “collective effort” and specialized experimental branches
    

## OneFlow model

OneFlow is a simplified version of the GitFlow/GitHubFlow branching model. Here it is:

pic4

The principles are:

- everyone works on their feature branches on their personal (`origin`) forks of the upstream repository (`upstream`)
- every feature gets a a new branch named `feature/<feature_name>`, the same goes for `bugfix` , `refactor` , etc.
- feature branches are merged by creating a Pull Request onto the `main` branch, and only after appeasing the CI/CD (green tests) and peer reviewers 🧔
- all the feature branches are merged directly into the upstream’s `main` branch
- as soon as the feature branch is merged, it is deleted immediately (to avoid garbage)
- a release starts with a tag naming the release version, e.g., `v0.1.0`
- if there are to be any hotfixes on some release branch, we create the branch for the release series, e.g., `release-v0.1`, and add hotfixes/changes there. We also tag the exact hotfix releases (e.g. `v0.1.1`).

## Branching / Pull Request ~~commandments~~ rules

- Thou shalt not breakth `main` for other developers
- Thou shalt not try to merge multiple unrelated features in a single PR
- You break it, you fix it
- If several people need to work on a single feature, create a `collective_effort/<name>` branch in the upstream repo for it
- If you are the only developer working on a project - merge the stuff directly yourself, no review needed
- If the project is still in prototyping stage: merge directly, no need for review
- Updating just the docs: no need for review
- “[**Optimistic merging**](https://dmerej.info/blog/post/optimistic-merging/)”: the best skill that every developer should learn for collaboration is to *accept other’s coding style.* I.e., if a proposed change solves a problem and fits the dev guidelines, merge it immediately, and don’t waste others’ time on “stylistic improvements” and other nitpicks.
