### Version Control

```ad-info
**Definition 2.1**: Version Control keeps track of changes of files over time, allowing you to roll back to previous versions.
```

Software to handle this are known as "version control systems" (VCS)

#### Two Paradigms
- Centralized (CVCS)
	- **Central server** keeps track of all the changes and history
	- Each developer has local copies of files they need, but need to **check** in with the server to do any versioning

- Decentralized (DVCS)
	- Each developer has a local copy of the entire codebase and its history
	- Developers can perform **versioning locally** without needing to contact a server
	- Server **optional**
	- Examples: Git, Mercurial

Version is important in that
- Checkpointing your work
- Keeping **multiple parallel** versions of your work
	- Keeping multiple versions of in-progress work
	- Coordinate with people working on the same file
	- **NEVER** have to email/send codes

---

### Git Overview
- Repository: a **directory** of stuff that Git is versioning.
	- `.git` is the directory that holds all this metadata
- Commit: a **checkpoint** for the files in the repository
	- Given a hash for identification
- History is a **linked list** of **commits** pointing to their parent
	![[Pasted image 20230119010110.png|500]]


#### Initialize with `git init`
This initializes a Git repository inside the current directory.
- Creates the `.git` directory
- There will be an initial **branch** that you will be on with a default name
	- Currently is `master`, but subject to change
	- Can specify the `-b` and `--initial-branch` arguments to change what the initial branch is

```ad-warning
Initializing Git repos inside of Git repos might not work the way you expect them to.
- These sub-repos, or "sub-modules" follow their own versioning
```

---

#### File States
- **Unmodified**: Nothing has happened to this file; no changes as of the current commit
- **Modified**: This file **differs** from the version as of the **current commit**. Can be `git add` ed to be **Staged**
- **Staged**: This file differs, and is set to be in the **next commit**
- **Untracked**: This file does **NOT** **exist** as of the current commit
	- Similar to `modified`, but differs by **existing** while shouldn't
	
		![[Pasted image 20230119010724.png|500]]

#### Areas
- **Working Directory**: the directory as your filesystem sees it, a mess of files which may or may not be changed, or may be even untracked
- **Staging Area**/Index: list of files whose snapshots will be part of the **next commit**
- **Repository**: What commits Git now has saved
	![[Pasted image 20230119011006.png|500]]

```ad-example
**Scenario 1**: Untracked file
1. An untracked file exists
2. You decide to start versioning it, so you `git add` it, making it **Staged** and putting it into the **Index**
3. You commit the file in the **Index**, landing it in **Repository**

**Scenario 2**: Modified file
1. You make some changes, so now the file is Modified
	- To change it back to the old **committed version**, do `git checkout`
2. You `git add` it, making it **Staged** and putting it into the **Index**
	- To unstage files, do `git reset` and the files will be back to **modified**
3. You commit the file's snapshot, getting that snapshot into the Repository
```

---

### Git Commands
#### Initialization
1. Initialize the repository
	- `git init`
2. Add the initial files you want to track to the **Index**
	- `git add`
3. Commit those initial files to the **Repository**
	- `git commit`

#### Modification
- Want to make some **modified** file **unmodified** again
	- `git checkout <filename>`
	- `git restore`
- Take files **out** of the **Index**
	- `git reset <filename>`
- After `git commit`, want to modify the commit message or add **NEW** files
	- `git commit --amend`

#### Commits
`git commit -m <message>` is a quick and dirty way to make a commit
- Not ideal for collaboration
- `git commit` will open the **configured editor** and allow you to more easily fully fill out a commit message

##### Style
- Title
	- Limit to 50 characters
	- Capitalize the first letter
	- Imperative
	- Summarize the commit
- Body
	- Limit to 72 characters per line
	- Explain **what** changed and **why**, **NOT** how
	- References to bug/issue number

---

#### Branching

```ad-info
**Definition 2.2**: A branch in Git is a pointer to a commit.
- Super lightweight
- `HEAD` is a pointer pointing to the **current commit** that's being looked at.
	- `HEAD` will follow along with the branch you are on
```

#####  Why Branching?
- Make a **backup** of branch: easy to refer to a particular commit
- Manage a **feature** ("topic"/"feature" branches)
- Have a separate line of development (e.g. taking two different approaches)
- Represent release schedules (e.g. a **development** branch and a **release** branch)

---

##### Branching Commands

- The default branch is `master`.
	- Typically used for production/release
- `git branch` lists your **local** branches
- `git branch <branch-name>` creates a **NEW** branch
	- `<branch-name>` will point to wherever `HEAD` is pointing to
- `git checkout <branch-name>` checks out the branch, making the `HEAD` point to where `<branch-name>` is pointing to
	- `git switch` switches to a branch
	- `git checkout -b <branch-name>` **creates** and **checks out** the branch
- `git merge <branch-name>` will try to move the **current branch** to where `<branch-name>` is. This is called **fast-forwarding**
	- If the branches **diverged** (`<branch-name>` and the current branch both got **new commits** before **merging**), a special "merge commit" will be produced; more on this later

