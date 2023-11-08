_New to working with a team on git and github? Start here_

TL;DR Don't commit to `main`, always use a branch, then create a pull request

### GUI vs Command Line

Although it's tempting to use the GUI on github.com or on the github desktop app, the sooner you start using the command line for all git commands, the sooner you will be comfortable with it. Believe it or not, you actually have more control when using the command line and are able to do more advanced git commands. So it might be intimidating at first, but in the long run it will be faster and easier.

### Suggested Tools

- Github desktop (or similar app for read-only visualization)
- iTerm (for command line git operations)

### Local vs Remote

- Local is your computer. You will use the command line to navigate between folders and projects, and run git commands.
- Remote is github.com. Everyone on your team has access to all code and branches there.
- To make sure that your local is connected to the remote, run `git remote -v` in the command line, and it should show something similar to

```
origin	https://github.com/jwicksnin/docs.git (fetch)
origin	https://github.com/jwicksnin/docs.git (push)
```

### Branches

- Your repo has one branch automatically: `main`
- Never commit to `main` - always use a branch
- You can see which branches are available locally by running `git branch`
- In order to create a new branch, run `git checkout -b your-new-branch-name-here`
- Your branch name should have no spaces and use `-` or `_` or camelCase
- Make all of your commits on your branch (not `main`), and when your feature is done, you can create a PR. See the `Pull Requests` section below.

### Rebase and Merge

#### Why to use Rebase locally

- Cleaner and understandable git history
- Easier to reason about when merging PRs
- To learn more about rebasing and merging, see [this article](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

#### Workflow

1. On your local branch, not `main`, make your commits for your feature
1. If you need the most recent changes on `main` (for example, your teammate merged some code that your branch depends on, or you are ready to create a PR), run the following

   ```command
   git checkout main
   git pull origin main
   ```

   Now your local `main` is up to date with the remote

   ```command
   git checkout your-local-branch
   git rebase main
   ```

   This "replays" your branch's commits on top of what is in `main`

1. If you run into merge conflicts, don't panic. This is normal and usually easier to resolve than it seems. And VS Code makes it easy to choose which code to keep during a merge conflict. You can search your codebase for `>>>>>` and it will find all the merge conflicts. VS Code will label each conflict and you can remove the change you don't want.
1. Once all the merge conflicts are resolved and there are no more `>>>>>` in your codebase, you can run the following
   ```command
   git add .
   git rebase --continue
   ```
   This updates your commit with the changes from main. You might have multiple merge conflicts when rebasing onto `main`, and that is ok. Each one is associated with a different commit that you are replaying on top of `main`.
1. One you have replayed all your commits on top of `main`, you can run `git log` and you will see all your commits, followed by all the commits on `main`.
1. Now you have the most recent changes in `main` on your local branch and can continue working or create a PR when you're ready.

### Pull Requests (PRs)

1. See `Branches` and `Rebase and Merge` above
1. Once your local branch has all of your feature changes and you have rebased onto `main`, you are ready to create a PR
1. In your local branch, run `git push origin your-new-branch` which pushes your local branch to the remote.
1. Go to github.com and select Pull Requests. You can select `your-new-branch` to be merged into `main`. Create the PR and assign any of your teammates to review it.
1. Once your teammate has reviewed and approved your PR, go to your PR on github.com and merge your branch into `main`. If it objects that your banch is X commits behind `main`, you will need to follow the process under `Rebase and Merge` above to get all the changes in `main` into your branch, then push to the remote again.

1. Once your PR is merged, update your local repo by running
   ```
   git checkout main
   git pull origin main
   ```
   Your local `main` branch is always a copy of what is on the remote. If there are any issues when pulling it down, you should check if you have any local changes on `main` and remove them.
