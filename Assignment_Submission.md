# SOFTWARE DEVELOPMENT PRACTICES (SDP)
## ASSIGNMENT SUBMISSION DOCUMENT

---

### Student & Submission Details
- **Student GitHub Profile**: [@X-DIABLO-X](https://github.com/X-DIABLO-X)
- **Public GitHub Repository URL**: [https://github.com/X-DIABLO-X/SDP-Assignment-Git-Workflow](https://github.com/X-DIABLO-X/SDP-Assignment-Git-Workflow)
- **Primary Branch**: `main`
- **Secondary Feature Branch**: `feature`

---

# QUESTION 1: GIT & GITHUB WORKFLOW (10 Marks)

---

### Step 1: Local Repository Setup (2 Marks)

#### 1. Project Folder Setup
A clean local directory was designated at `D:\BITS\SGA\SDP` on the local machine. The repository was created entirely locally first without initializing on GitHub.

#### 2. HTML Project Files Created
Two essential HTML files were authored:
- `index.html`: Contains semantic HTML5 boilerplate with project title, navigation header, and introduction.
- `about.html`: Contains project about information and team navigation structure.

#### 3. Git Repository Initialization
The local repository was initialized using the default branch name `main`:
```bash
git init -b main
```

**Terminal Output:**
```text
Initialized empty Git repository in D:/BITS/SGA/SDP/.git/
```

**Repository State Check:**
```bash
git status
```
```text
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	about.html
	index.html

nothing added to commit but untracked files present (use "git add" to track)
```

---

### Step 2: Git Workflow Execution (4 Marks)

#### 1. Initial Commit
The initial state of `index.html` and `about.html` was staged and committed:
```bash
git add index.html about.html
git commit -m "Initial commit: Add index.html and about.html"
```
**Terminal Output:**
```text
[main (root-commit) eac2ca6] Initial commit: Add index.html and about.html
 2 files changed, 52 insertions(+)
 create mode 100644 about.html
 create mode 100644 index.html
```

#### 2. Modify File and Create Second Commit
`index.html` was modified to append a **Key Objectives** section.
```bash
git diff index.html
git add index.html
git commit -m "Update index.html: Add Key Objectives section"
```
**Terminal Output:**
```text
[main 2d696df] Update index.html: Add Key Objectives section
 1 file changed, 9 insertions(+)
```

#### 3. Create and Switch to `feature` Branch
A new branch named `feature` was created and checked out:
```bash
git branch feature
git checkout feature
```
**Terminal Output:**
```text
Switched to branch 'feature'
```

#### 4. Make Changes and Commit to the `feature` Branch
`about.html` was updated on the `feature` branch to add a **Team & Workflow Features** section.
```bash
git add about.html
git commit -m "Add Team & Workflow Features section to about.html on feature branch"
```
**Terminal Output:**
```text
[feature ab576c0] Add Team & Workflow Features section to about.html on feature branch
 1 file changed, 9 insertions(+)
```

#### 5. Switch Back to `main` Branch
```bash
git checkout main
```
**Terminal Output:**
```text
Switched to branch 'main'
```

#### 6. Merge the `feature` Branch into `main`
To explicitly preserve the branch topology and activity in history (avoiding a fast-forward collapse), the merge was executed with `--no-ff`:
```bash
git merge --no-ff feature -m "Merge branch 'feature' into main"
```
**Terminal Output:**
```text
Merge made by the 'ort' strategy.
 about.html | 9 +++++++++
 1 file changed, 9 insertions(+)
```

#### 7. Display Commit History Showing Branch Activity
```bash
git log --graph --oneline --all
```
**Terminal Output:**
```text
*   f496bf4 Merge branch 'feature' into main
|\  
| * ab576c0 Add Team & Workflow Features section to about.html on feature branch
|/  
* 2d696df Update index.html: Add Key Objectives section
* eac2ca6 Initial commit: Add index.html and about.html
```

---

### Step 3: .gitignore Implementation (1 Mark)

#### 1. `.gitignore` Configuration
A `.gitignore` file was created in the repository root to ignore all `.log` files and the `temp/` directory:
```gitignore
# Ignore all log files
*.log

# Ignore temp directory and its contents
temp/
```

#### 2. Verification of `.gitignore`
To test and prove the ignoring behavior, `app.log` and a subfolder `temp/cache.txt` were created:
```bash
New-Item -ItemType Directory -Force -Path "temp"
Set-Content -Path "temp\cache.txt" -Value "temporary file"
Set-Content -Path "app.log" -Value "error log entry"
git status
```
**Terminal Output:**
```text
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore

nothing added to commit but untracked files present (use "git add" to track)
```
*Note: Neither `app.log` nor `temp/` appears under untracked files.*

Confirmation via `git check-ignore`:
```bash
git check-ignore app.log temp/cache.txt
```
**Output:**
```text
app.log
temp/cache.txt
```

#### 3. Committing `.gitignore`
```bash
git add .gitignore
git commit -m "Configure .gitignore to ignore .log files and temp/ directory"
```
**Terminal Output:**
```text
[main 01850fa] Configure .gitignore to ignore .log files and temp/ directory
 1 file changed, 5 insertions(+)
 create mode 100644 .gitignore
```

---

### Step 4: Undo Operation using Git (1 Mark)

Both standard Git undo techniques were demonstrated and verified:

#### Technique A: Local History Undo using `git reset HEAD~1`
1. Created and committed a test file `test_reset.txt`:
   ```bash
   git add test_reset.txt
   git commit -m "Commit to be undone via git reset"
   ```
   **Output:** `[main f4db6b7] Commit to be undone via git reset`
2. Executed undo:
   ```bash
   git reset HEAD~1
   ```
3. Checked repository state:
   ```bash
   git log -n 2 --oneline
   git status
   ```
   **Effect on State:**
   - The commit `f4db6b7` was removed from the commit history.
   - `HEAD` was reset back to `01850fa`.
   - `test_reset.txt` was preserved in the working directory as an untracked file, demonstrating that the commit was undone without data destruction.

#### Technique B: Shared/Remote History Undo using `git revert HEAD`
In team environments and on public remote repositories (like GitHub), `git revert` is the recommended operation because it preserves an auditable, non-destructive history.
1. Created and committed `temp_undo_feature.txt`:
   ```bash
   git add temp_undo_feature.txt
   git commit -m "Add temporary feature file for undo demonstration"
   ```
   **Output:** `[main fbb0bde] Add temporary feature file for undo demonstration`
2. Executed undo via revert:
   ```bash
   git revert HEAD --no-edit
   ```
   **Terminal Output:**
   ```text
   [main 4c9c878] Revert "Add temporary feature file for undo demonstration"
    1 file changed, 1 deletion(-)
    delete mode 100644 temp_undo_feature.txt
   ```
3. **Effect on State:**
   - Git automatically deleted `temp_undo_feature.txt` from the working tree.
   - A new commit `4c9c878` was recorded, explicitly documenting the undo operation in the permanent GitHub commit history.

---

### Step 5: GitHub Integration (1 Mark)

#### 1. Public GitHub Repository Creation
A public repository was created under GitHub user `X-DIABLO-X`:
- **Repository Name**: `SDP-Assignment-Git-Workflow`
- **Visibility**: Public
- **URL**: [https://github.com/X-DIABLO-X/SDP-Assignment-Git-Workflow](https://github.com/X-DIABLO-X/SDP-Assignment-Git-Workflow)

#### 2. Linking Local Repository to GitHub Remote
```bash
git remote add origin https://github.com/X-DIABLO-X/SDP-Assignment-Git-Workflow.git
git remote -v
```
**Terminal Output:**
```text
origin	https://github.com/X-DIABLO-X/SDP-Assignment-Git-Workflow.git (fetch)
origin	https://github.com/X-DIABLO-X/SDP-Assignment-Git-Workflow.git (push)
```

#### 3. Pushing Branches to GitHub
```bash
git push -u origin main
git push origin feature
```

#### 4. GitHub Remote Commit Verification
All commits, branch forks, merges, and revert operations are fully verified on GitHub:
```json
[
  {"sha": "4cd8cef", "message": "Add git_commands.txt with documented commands and explanations"},
  {"sha": "4c9c878", "message": "Revert \"Add temporary feature file for undo demonstration\""},
  {"sha": "fbb0bde", "message": "Add temporary feature file for undo demonstration"},
  {"sha": "01850fa", "message": "Configure .gitignore to ignore .log files and temp/ directory"},
  {"sha": "f496bf4", "message": "Merge branch 'feature' into main"},
  {"sha": "ab576c0", "message": "Add Team & Workflow Features section to about.html on feature branch"},
  {"sha": "2d696df", "message": "Update index.html: Add Key Objectives section"},
  {"sha": "eac2ca6", "message": "Initial commit: Add index.html and about.html"}
]
```

---

### Step 6: Command Documentation File (`git_commands.txt`) (1 Mark)

The file `git_commands.txt` was created in the repository root and committed.

#### Complete Content of `git_commands.txt`:
```text
================================================================================
GIT COMMANDS DOCUMENTATION - SDP ASSIGNMENT
Student / GitHub User: X-DIABLO-X
================================================================================

1. git init -b main
   Initializes a new Git repository locally in the current directory with the default branch set to 'main'.

2. git config --global user.name "<username>"
   Configures the global Git username associated with all future commits made on this system.

3. git config --global user.email "<email>"
   Configures the global Git user email associated with all future commits made on this system.

4. git status
   Displays the working tree status, showing tracked, untracked, modified, and staged files.

5. git add <file>
   Stages a specific file from the working directory into the Git staging area for the next commit.

6. git add .
   Stages all new, modified, and deleted files across the entire workspace into the staging area.

7. git commit -m "<message>"
   Captures a permanent snapshot of all staged changes in the repository history with a descriptive message.

8. git diff
   Inspects line-by-line differences between modified files in the working directory and the staging area.

9. git branch feature
   Creates a new branch named 'feature' pointing to the current commit without switching to it.

10. git checkout feature
    Switches the active working directory and HEAD pointer to the 'feature' branch.

11. git checkout main
    Switches the active working directory and HEAD pointer back to the primary 'main' branch.

12. git merge --no-ff feature -m "<message>"
    Merges the 'feature' branch into 'main', using --no-ff to create a dedicated merge commit preserving history.

13. git log --graph --oneline --all
    Displays an ASCII graph of all commits, branch forks, and merges in a compact single-line format.

14. git check-ignore -v <path>
    Verifies which rule inside the .gitignore file is causing a specific file or folder to be ignored.

15. git reset HEAD~1
    Undoes the most recent commit locally by rewinding HEAD by one commit, leaving file changes unstaged.

16. git revert HEAD --no-edit
    Undoes the previous commit by automatically creating a new inverse commit, preserving shared repository history.

17. git remote add origin <url>
    Binds the local repository to a remote repository hosted on GitHub under the alias 'origin'.

18. git remote -v
    Displays the URLs and aliases of all configured remote repositories for both fetch and push operations.

19. git push -u origin main
    Pushes local 'main' branch commits to GitHub and establishes upstream tracking for subsequent pulls/pushes.

20. git push origin feature
    Pushes the local 'feature' branch to GitHub so branch development history is fully visible on the remote.

================================================================================
```

---
---

# QUESTION 2: TEST CASE DESIGN (10 Marks)

---

### Application Scenario
Testing a **Login System** containing:
- **Username** (Text Input)
- **Password** (Masked Text Input)
- **Login** (Action Button)

### Functional Requirements (FR) Under Test
- **FR1**: Username must not be empty.
- **FR2**: Password must be at least 6 characters long.
- **FR3**: Valid username and password allow a successful login.
- **FR4**: Invalid credentials should display an error message.
- **FR5**: After 3 consecutive failed login attempts, the account should be locked.

---

### Step 1: Comprehensive Test Case Table

| Test Case ID | Test Scenario | Input Data | Expected Output |
| :--- | :--- | :--- | :--- |
| **TC_01** | Valid login with correct credentials and password length exceeding minimum (> 6 characters) | `Username = "user1"`<br>`Password = "pass123"` (length = 7)<br>Click `[Login]` button | Authentication successful. User is redirected to dashboard/home page with welcome message: *"Welcome, user1"*. Consecutive failed login counter is reset to 0. |
| **TC_02** | Valid login with boundary password length of exactly 6 characters | `Username = "john_doe"`<br>`Password = "Secure"` (length = 6)<br>Click `[Login]` button | Authentication successful. System accepts exactly 6 characters as valid password length and redirects user to dashboard. |
| **TC_03** | Login attempt with empty username field (Edge Case) | `Username = ""` (empty string)<br>`Password = "pass123"`<br>Click `[Login]` button | Login blocked. Form validation error message displayed beneath username field: *"Username is required and cannot be empty."* Focus placed on username input. No authentication request dispatched. |
| **TC_04** | Login attempt with empty password field (Edge Case) | `Username = "user1"`<br>`Password = ""` (empty string)<br>Click `[Login]` button | Login blocked. Form validation error message displayed beneath password field: *"Password is required and cannot be empty."* Focus placed on password input. No authentication request dispatched. |
| **TC_05** | Login attempt with password length strictly less than 6 characters (boundary = 5 characters) | `Username = "user1"`<br>`Password = "12345"` (length = 5)<br>Click `[Login]` button | Login blocked. Client-side validation error message displayed: *"Password must be at least 6 characters long."* Password input is cleared and highlighted in red. |
| **TC_06** | Login attempt with both username and password fields empty (Edge Case) | `Username = ""` (empty string)<br>`Password = ""` (empty string)<br>Click `[Login]` button | Login blocked. Dual validation error messages displayed simultaneously: *"Username is required and cannot be empty"* and *"Password is required and cannot be empty."* |
| **TC_07** | Login attempt with unregistered/invalid username and valid password format | `Username = "non_existent_user"`<br>`Password = "ValidPass789"`<br>Click `[Login]` button | Authentication fails. Generic error message displayed: *"Invalid username or password. Please verify your credentials and try again."* User remains on login screen. |
| **TC_08** | Login attempt with valid username but incorrect password (Attempt 1 of 3) | `Username = "user1"`<br>`Password = "wrongPassword1"`<br>Click `[Login]` button | Authentication fails. Error message displayed: *"Invalid username or password. 2 attempts remaining before account lockout."* Failed login attempt counter increments from 0 to 1. |
| **TC_09** | Account lockout triggered upon reaching 3 consecutive failed login attempts | **Attempt 1:** `Username = "user1"`, `Password = "wrong1"` (Counter = 1)<br>**Attempt 2:** `Username = "user1"`, `Password = "wrong2"` (Counter = 2)<br>**Attempt 3:** `Username = "user1"`, `Password = "wrong3"` (Counter = 3)<br>Click `[Login]` button | On 3rd failed attempt, authentication fails and account status transitions to 'Locked'. Error message displayed: *"Account locked due to 3 consecutive failed login attempts. Please contact administrator or reset your password."* |
| **TC_10** | Login attempt with valid credentials on an already locked account | `Username = "user1"` (account locked)<br>`Password = "pass123"` (correct credentials)<br>Click `[Login]` button | Authentication rejected despite correct credentials. Error message displayed: *"Your account is currently locked. Please contact support or utilize account recovery to unlock."* |
| **TC_11** | Reset of consecutive failed login attempt counter upon successful login | **Attempt 1:** `Username = "user1"`, `Password = "wrong1"` (fails, Counter = 1)<br>**Attempt 2:** `Username = "user1"`, `Password = "pass123"` (correct), Click `[Login]` | Login successful on Attempt 2. User redirected to dashboard. Consecutive failed login attempt counter for `user1` is reset from 1 to 0. |

---

### Step 2: Requirements Traceability & Coverage Matrix

| Functional Requirement | Specification Description | Primary Test Cases | Coverage Status |
| :--- | :--- | :--- | :--- |
| **FR1** | Username must not be empty | **TC_03, TC_06** | 100% Covered (Empty, Both Empty) |
| **FR2** | Password must be at least 6 characters long | **TC_02, TC_04, TC_05, TC_06** | 100% Covered (Boundary 6, Boundary 5, Empty) |
| **FR3** | Valid username and password allow successful login | **TC_01, TC_02, TC_11** | 100% Covered (Standard, Boundary, Counter Reset) |
| **FR4** | Invalid credentials display error message | **TC_07, TC_08** | 100% Covered (Unregistered user, Wrong password) |
| **FR5** | 3 consecutive failed attempts lock account | **TC_08, TC_09, TC_10, TC_11** | 100% Covered (Threshold, Lockout, Locked state, Reset) |

---

### Step 3: Test Case Design Techniques Used

1. **Boundary Value Analysis (BVA)**
   - *Target Attribute*: Password length requirement ($\ge 6$ characters).
   - *Boundary Values Tested*:
     - $L = 5$ characters (`12345`) $\rightarrow$ Rejected (**TC_05**).
     - $L = 6$ characters (`Secure`) $\rightarrow$ Accepted (**TC_02**).

2. **Equivalence Partitioning (EP)**
   - *Username Partitions*:
     - Valid Partition: Non-empty registered username (`user1`, `john_doe`).
     - Invalid Partition 1: Empty string (`""`).
     - Invalid Partition 2: Non-existent / unregistered username (`non_existent_user`).
   - *Password Partitions*:
     - Valid Partition: Correct password with length $\ge 6$.
     - Invalid Partition 1: Empty string (`""`).
     - Invalid Partition 2: Length $< 6$ characters.
     - Invalid Partition 3: Incorrect password with valid length.

3. **State Transition Testing (Account Lockout)**
   - **State 0 (Active, Failed Attempts = 0)**: Initial active state.
   - **State 1 (Active, Failed Attempts = 1)**: First invalid attempt (**TC_08**).
   - **State 2 (Active, Failed Attempts = 2)**: Second consecutive invalid attempt.
   - **State 3 (Locked)**: Third consecutive invalid attempt transitions account to locked state (**TC_09**).
   - **State 4 (Post-Lockout Block)**: Correct credentials cannot bypass locked state (**TC_10**).
   - **State Reset**: A successful login at State 1 or 2 returns the system to State 0 (**TC_11**).
