# ACM Design Team: Meet the Team

A one-page team wall that we build together during the git workshop.
Open `index.html` in a browser to see it.

## Add yourself

1. **Fork** this repo on GitHub, then clone your fork.
2. Create a branch: `git switch -c add-<your-name>`
3. In `index.html`, copy the member card between the ✂️ comments and paste it above the "Add yourself" card. Fill in your details.
4. Save, then `git add index.html` and `git commit -m "Add <your-name> to the team page"`
5. `git push -u origin add-<your-name>`
6. On GitHub, open a **Pull Request** from your branch into this repo's `main`.

## Git cheat sheet

| Command | What it does |
| --- | --- |
| `git status` | What changed, and what's staged |
| `git diff` | Show changes you haven't staged yet |
| `git add <file>` | Stage a change for the next commit |
| `git commit -m "msg"` | Save staged changes as a snapshot |
| `git push` | Upload your commits to GitHub |
| `git pull` | Download and merge new commits from GitHub |
| `git switch -c <name>` | Create a new branch and switch to it |
| `git switch <name>` | Switch to an existing branch |
| `git merge <branch>` | Bring another branch's commits into the current one |
| `git log --oneline --graph --all` | Draw the commit history |
