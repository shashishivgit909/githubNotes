# 🔁 Git Reflog – Beginner Friendly Guide

## 🧠 What is `git reflog`?

`git reflog` is like a **security camera for Git**. It keeps a record of everything you’ve done in your Git repository:

- Commits
- Branch switches (checkouts)
- Resets
- Merges
- Rebases

Even if you accidentally **delete a commit** or **reset a branch**, `reflog` still remembers!

---

## 🧪 Why is this Useful?

Sometimes you:

- Run `git reset --hard` and lose commits 😱
- Delete a branch by mistake
- Can’t find something with `git log`

You can **recover lost work** using `git reflog`.

---

## 🛠 How to Use

```bash
git reflog
```

This shows output like:

```
a3c6e5d HEAD@{0}: commit: fixed login bug
2b1c22a HEAD@{1}: checkout: switched to main
b4f22e7 HEAD@{2}: commit: added auth middleware
```

- `HEAD@{0}` → Current state
- `HEAD@{1}` → One step before
- `HEAD@{2}` → Two steps before
- Each entry shows the action you did (commit, checkout, reset, etc.)

---

## 🔙 How to Go Back to an Earlier State

1. Run:

   ```bash
   git reflog
   ```

2. Find the previous point you want to go back to (e.g., `HEAD@{2}`)

3. Run:

   ```bash
   git checkout HEAD@{2}
   ```

   Or to **restore everything** to that point:

   ```bash
   git reset --hard HEAD@{2}
   ```

---

## 💡 Common Use Cases

### ✅ Recover a Deleted Commit

```bash
git reset --hard HEAD~1  # Oops! Deleted a commit

git reflog               # Find where HEAD was
git reset --hard HEAD@{1}  # Recover it!
```

---

### ✅ Restore Deleted Branch Work

```bash
git reflog               # Find commit ID
git checkout <commit-id> # Restore lost work
```

---

## 🔍 `git log` vs `git reflog`

| Command      | Shows                                     |
|--------------|-------------------------------------------|
| `git log`    | Commits that are still part of a branch   |
| `git reflog` | **All changes**, even deleted or reset    |

---

## ⏳ How Long Does Reflog Last?

- Git stores reflog data for **90 days** by default.
- This gives you plenty of time to **recover lost work**.

You can configure it using:

```bash
git config --global gc.reflogExpire 30.days
```

---

## 🧽 How to Clean Reflog (Advanced – ⚠️ Dangerous)

```bash
git reflog expire --expire=now --all
git gc --prune=now --aggressive
```

⚠️ This will **permanently delete** reflog history. Use with caution.

---

## 🧠 Summary Table

| Command                            | What It Does                              |
|------------------------------------|--------------------------------------------|
| `git reflog`                       | Show all recent Git actions                |
| `HEAD@{n}`                         | Go back n steps in history                 |
| `git checkout HEAD@{2}`            | Go to the state 2 steps ago                |
| `git reset --hard HEAD@{3}`        | Reset project to that exact past state     |
| `git reflog show <branch-name>`    | Show history for a specific branch         |

---

## 📦 Pro Tip

You can use `git reflog` to **undo nearly anything** — even if it's not in `git log`. It's a lifesaver!