# Git Rebase & Merge Lab (Java Edition)

Welcome to the Git Rebase and Merge practical session! We will simulate a real-world software development lifecycle by working on a `Calculator.java` application.

## 0. Prerequisite: Enter Your Project Directory
Open your terminal inside this project folder (`java-calculator-demo`).

## 1. Initialize Git and Make Initial Commit
First, let's turn this folder into a Git repository and commit our base Java file.
```bash
git init
git add .
git commit -m "Initial commit: Add base Calculator App"
```

## 2. Create Feature Branches
Let's simulate a team working on different features simultaneously. We will create 3 feature branches from our base code.
```bash
# Branch 1: Addition Feature
git branch feature-add

# Branch 2: Subtraction Feature 
git branch feature-subtract

# Branch 3: Multiplication Feature
git branch feature-multiply
```

---

## 3. Work on Feature 1 and MERGE It (The Standard Way)

Switch to our first branch adding the addition feature.
```bash
git checkout feature-add
```

**Task:** Open `src/com/demo/Calculator.java` and add an `add` method below `main`:
```java
    public static int add(int a, int b) {
        return a + b;
    }
```

Commit your changes:
```bash
git add src/com/demo/Calculator.java
git commit -m "Add addition method to Calculator"
```

**Merge it to main:**
```bash
git checkout main
git merge feature-add
```
*Since `main` hasn't changed since `feature-add` was created, this will be a "fast-forward" merge.*

---

## 4. Work on Feature 2 and REBASE It (Keeping a Clean History)

While `main` now has the `add` method, our `feature-subtract` branch does not know about it yet. Let's work on it.
```bash
git checkout feature-subtract
```

**Task:** Open `src/com/demo/Calculator.java` and add a `subtract` method. Notice the `add` method is missing here because we branched off before it was merged!
```java
    public static int subtract(int a, int b) {
        return a - b;
    }
```

Commit your changes:
```bash
git add src/com/demo/Calculator.java
git commit -m "Add subtract method to Calculator"
```

In a real project, before merging a feature, it's a good idea to pull the latest `main` changes into your feature branch so you resolve any conflicts locally and keep history linear. We do this via **rebase**.

```bash
# Rebase feature-subtract onto main
git rebase main
```
*Git will temporarily remove your subtract commit, apply the commits from `main` (the add feature), and then put your subtract commit back on top.*

Now merge it into main:
```bash
git checkout main
git merge feature-subtract
```
*Because we rebased first, this also becomes a clean fast-forward merge!*

---

## 5. Work on Feature 3 and Handle a MERGE CONFLICT

Conflicts happen when two branches modify the exact same lines of code. Let's force a conflict.

```bash
git checkout feature-multiply
```

**Task:** Open `src/com/demo/Calculator.java`. Because we branched off the original commit, neither `add` nor `subtract` methods are here. Change the main method's print statement to:
```java
        System.out.println("Welcome to the Advanced Java Calculator!");
```
And add the `multiply` method:
```java
    public static int multiply(int a, int b) {
        return a * b;
    }
```

Commit the changes:
```bash
git add src/com/demo/Calculator.java
git commit -m "Update greeting and add multiply method"
```

Now, try to rebase or merge it with main. Let's try rebasing:
```bash
git rebase main
```

**BOOM! Conflict.** Git will pause and say there's a conflict because `Calculator.java` was modified differently in `main` and `feature-multiply`.

**How to resolve:**
1. Open `src/com/demo/Calculator.java` in VS Code.
2. You will see markers like `<<<<<<< HEAD`, `=======`, and `>>>>>>>`.
3. Choose "Accept Both Changes" or manually edit the file so it has the new println greeting AND all 3 methods (`add`, `subtract`, `multiply`).
4. Save the file.

Finish the rebase:
```bash
git add src/com/demo/Calculator.java
git rebase --continue
```

Finally, merge into main:
```bash
git checkout main
git merge feature-multiply
```

**Congratulations!** You have practically applied Git branching, merging, rebasing, and conflict resolution!