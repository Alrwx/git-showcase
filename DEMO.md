# Git Workshop: Presenter Script

Two presenters:

- **Lead (L)** owns the main GitHub repo.
- **Co-lead (C)** plays a contributor who doesn't have write access, so they work from a **fork**.

Replace `<LEAD>` and `<COLEAD>` with your GitHub usernames. Rough timing is about 45 minutes.

> Keep `index.html` open in a browser on the projector and refresh after every step.
> Each change shows up on the page, so the audience can see what git is doing.

---

## Before the workshop (both of you)

- [ ] Git is installed: `git --version`
- [ ] Your identity is set:
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "you@example.com"
  ```
- [ ] You're signed in to GitHub in the browser, and you've pushed at least once from this machine so the credential prompt doesn't happen live.
- [ ] **L:** Create an **empty** public repo on GitHub named `git-showcase`. Don't add a README, .gitignore, or license.
- [ ] **L:** Make sure this folder is **not** a git repo yet (there should be no `.git` folder). You'll run `git init` live.

---

## Act 1: add, commit, push (L, about 10 min)

**Explain first:** git has three places a change can be.

```
 Working directory  --git add-->  Staging area  --git commit-->  Local repo  --git push-->  GitHub
   (your files)                  ("the cart")                    (history)                 (shared)
```

**L runs:**

```bash
git init -b main
git status                      # everything is "untracked"
git add index.html              # stage one file and show that status changed
git status
git add .                       # stage the rest
git commit -m "Initial team page"
git log --oneline
```

**Talking point:** a commit is a snapshot with a message, an author, and a unique ID. So far it only exists on this laptop.

**L connects to GitHub and pushes:**

```bash
git remote add origin https://github.com/alrwx/git-showcase.git
git push -u origin main
```

Refresh the GitHub page so everyone can see the files.

**Make one more change to show the loop:** in `index.html`, edit the tagline, save, and refresh the browser.

```bash
git status
git diff                        # red is the old line, green is the new one
git add index.html
git commit -m "Update tagline"
git push
```

**Talking point:** this is the everyday loop: **edit, add, commit, push**.

---

## Act 2: Branches (L, about 8 min)

**Explain first:** a branch is a separate line of work. `main` stays safe while you experiment.

```bash
git switch -c add-lead-card
```

Copy the member card in `index.html` (between the ✂️ comments), paste it above "Add yourself", and fill in **L's** details. Refresh the browser.

```bash
git add index.html
git commit -m "Add lead's card"
git log --oneline --graph --all
```

**Show the key moment:**

```bash
git switch main                 # refresh the browser: the card is GONE
git switch add-lead-card        # refresh the browser: the card is BACK
```

**Talking point:** the files on disk change to match the branch you're on. Nothing is lost.

**Merge the branch into main and push:**

```bash
git switch main
git merge add-lead-card         # a "fast-forward", because main hadn't moved
git push
git log --oneline --graph --all
```

---

## Act 3: Forks (C, about 7 min)

**Explain first:** a **fork** is your own copy of someone else's repo on GitHub.
You fork when you don't have permission to push to the original, as with open source or another team's project.

| | Branch | Fork |
| --- | --- | --- |
| Where it lives | Inside the same repo | A separate copy of the repo under your account |
| Who can push | People with write access | Anyone, to their own fork |
| Typical use | Your own team | Outside contributors, open source |

**C, on GitHub:** open `github.com/<LEAD>/git-showcase` and click **Fork**.

**C, in the terminal:** use a different folder from L's.

```bash
git clone https://github.com/<COLEAD>/git-showcase.git
cd git-showcase
git remote add upstream https://github.com/<LEAD>/git-showcase.git
git remote -v
```

**Talking point:** `origin` is **my fork**, where I can push. `upstream` is **the original repo**, which I can only read from.

```bash
git switch -c add-colead-card
```

C adds their own card (below L's card, above "Add yourself").

**Also, as part of the conflict setup:** in `styles.css`, C changes the accent color to green:

```css
--accent: #00b894;
```

Refresh to show a green page, then commit and push to the fork:

```bash
git add .
git commit -m "Add co-lead's card and make accent green"
git push -u origin add-colead-card
```

**Don't open the PR yet.**

---

## Act 4: Merge conflicts (L sets it up, C resolves, about 10 min)

**Explain first:** git merges changes automatically **unless two people edited the same lines differently**.
When that happens it stops and asks a human to decide. That's a merge conflict. It isn't an error, and nothing is broken.

**Meanwhile, L changes the same line on `main`:** in `styles.css`, set

```css
--accent: #e84393;
```

```bash
git add styles.css
git commit -m "Make accent pink"
git push
```

Now `main` says pink and C's branch says green.

**C opens the PR** (Act 5 step 1). GitHub will say **"This branch has conflicts that must be resolved."** Point this out to the room.

**C resolves it locally:**

```bash
git fetch upstream
git merge upstream/main
# CONFLICT (content): Merge conflict in styles.css
git status                      # shows "both modified: styles.css"
```

Open `styles.css` in VS Code and show the markers:

```
<<<<<<< HEAD
  --accent: #00b894;
=======
  --accent: #e84393;
>>>>>>> upstream/main
```

**Talking point:** the part above `=======` is **mine**, the part below is **theirs**. VS Code offers *Accept Current / Accept Incoming / Accept Both*, or you can type something new.
This is a good moment to ask the design team which color should win.

After choosing, make sure all the marker lines are gone:

```bash
git add styles.css
git commit                      # git pre-fills a merge message, so save and close
git push
```

Refresh the PR. The conflict warning is gone and GitHub shows **"able to merge"**.

> **Alternative:** for small conflicts, GitHub's **Resolve conflicts** button on the PR lets you do the same thing in the browser.

---

## Act 5: Pull request (C opens it, L reviews and merges, about 8 min)

**Explain first:** a pull request says "here are my changes, please review them and pull them in." It's where code review happens.

1. **C:** On the fork, click **Compare & pull request**. Check that:
   - base repository is `<LEAD>/git-showcase` and base is `main`
   - head repository is `<COLEAD>/git-showcase` and compare is `add-colead-card`

   Write a title and description, then click **Create pull request**.
2. **L:** Open the PR and walk through the tabs:
   - **Conversation**: discussion
   - **Commits**: what's included
   - **Files changed**: the diff. Click a line and leave a comment, e.g. "Love the card!"
3. **L:** Click **Review changes**, choose **Approve**, and submit.
4. **L:** Click **Merge pull request**.
5. **L:** Pull the result locally and refresh the browser. Both cards are there, in the agreed color.
   ```bash
   git pull
   git log --oneline --graph --all
   ```
6. **C:** Sync the fork, either with the **Sync fork** button on GitHub or with:
   ```bash
   git switch main
   git pull upstream main
   git push
   ```

**Wrap-up:** everyone in the room forks the repo and opens a PR adding their own card, following the README.
If several people add cards in the same spot, they'll hit real merge conflicts, which is good practice.

---

## If something goes wrong live

| Problem | Fix |
| --- | --- |
| Staged the wrong file | `git restore --staged <file>` |
| Want to discard edits to a file | `git restore <file>` |
| Typo in the last commit message (not pushed yet) | `git commit --amend -m "new message"` |
| Push rejected ("fetch first") | `git pull`, then `git push` |
| A merge got messy and you want to start over | `git merge --abort` |
| Not sure what state you're in | `git status`. It usually tells you what to do next. |

## Resetting for a rehearsal

- **L:** Delete the `.git` folder in this project, revert `index.html` and `styles.css` to the original versions, and delete and recreate the GitHub repo.
- **C:** Delete the fork on GitHub (Settings → Danger Zone) and delete your local clone.
