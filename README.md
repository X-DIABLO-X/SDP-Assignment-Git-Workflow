# Software Development Practices (SDP) - Assignment 1

**Student / GitHub Profile:** [@X-DIABLO-X](https://github.com/X-DIABLO-X)  
**Repository URL:** [https://github.com/X-DIABLO-X/SDP-Assignment-Git-Workflow](https://github.com/X-DIABLO-X/SDP-Assignment-Git-Workflow)  

---

## Table of Contents
1. [Question 1: Git & GitHub Workflow (10 Marks)](#question-1-git--github-workflow)
   - [Step 1: Local Repository Setup](#step-1-local-repository-setup)
   - [Step 2: Git Workflow Execution](#step-2-git-workflow-execution)
   - [Step 3: .gitignore Implementation](#step-3-gitignore-implementation)
   - [Step 4: Undo Operation using Git](#step-4-undo-operation-using-git)
   - [Step 5: GitHub Integration](#step-5-github-integration)
   - [Step 6: Command Documentation File](#step-6-command-documentation-file)
2. [Question 2: Test Case Design (10 Marks)](#question-2-test-case-design)
   - [Requirements Coverage Analysis](#functional-requirements-coverage-matrix)
   - [Test Case Specification Table](#test-case-specification-table)
   - [Test Design Methodology](#test-design-methodology)

---

## Question 1: Git & GitHub Workflow

### Step 1: Local Repository Setup
- Local workspace initialized without creating GitHub repository first.
- Created `index.html` and `about.html`.
- Initialized local Git repository using `git init -b main`.

### Step 2: Git Workflow Execution
- Recorded initial commit with `index.html` and `about.html`.
- Modified `index.html` and created a second commit on `main`.
- Created branch `feature` using `git branch feature`.
- Switched to `feature` branch using `git checkout feature`.
- Modified `about.html` and committed changes on branch `feature`.
- Switched back to `main` branch with `git checkout main`.
- Merged `feature` into `main` using `git merge --no-ff feature` to explicitly record branch topology.

#### Commit History Graph
```bash
* 4cd8cef Add git_commands.txt with documented commands and explanations
* 4c9c878 Revert "Add temporary feature file for undo demonstration"
* fbb0bde Add temporary feature file for undo demonstration
* 01850fa Configure .gitignore to ignore .log files and temp/ directory
*   f496bf4 Merge branch 'feature' into main
|\  
| * ab576c0 Add Team & Workflow Features section to about.html on feature branch
|/  
* 2d696df Update index.html: Add Key Objectives section
* eac2ca6 Initial commit: Add index.html and about.html
```

### Step 3: .gitignore Implementation
Created `.gitignore` with rules:
```gitignore
# Ignore all log files
*.log

# Ignore temp directory and its contents
temp/
```
Verified that `.log` files and `temp/` folder contents are completely untracked via `git status` and `git check-ignore`.

### Step 4: Undo Operation using Git
1. **Local Undo (`git reset`)**:
   - Committed `test_reset.txt` (`f4db6b7 Commit to be undone via git reset`).
   - Ran `git reset HEAD~1` to move `HEAD` back to previous commit and place file into untracked working directory.
2. **Public History-Preserving Undo (`git revert`)**:
   - Committed `temp_undo_feature.txt` (`fbb0bde Add temporary feature file for undo demonstration`).
   - Executed `git revert HEAD --no-edit` (`4c9c878 Revert "Add temporary feature file for undo demonstration"`).
   - The file was safely removed while maintaining full auditability on GitHub.

### Step 5: GitHub Integration
- Public GitHub repository: [https://github.com/X-DIABLO-X/SDP-Assignment-Git-Workflow](https://github.com/X-DIABLO-X/SDP-Assignment-Git-Workflow)
- Connected local repository via `git remote add origin https://github.com/X-DIABLO-X/SDP-Assignment-Git-Workflow.git`.
- Pushed both `main` and `feature` branches using `git push -u origin main` and `git push origin feature`.

### Step 6: Command Documentation File
All commands with explanations are preserved in [`git_commands.txt`](git_commands.txt).

---

## Question 2: Test Case Design

### Application Scenario
- **System**: Login System
- **Inputs**: Username, Password, `[Login]` button

### Functional Requirements
1. **FR1**: Username must not be empty.
2. **FR2**: Password must be at least 6 characters long.
3. **FR3**: Valid username and password allow a successful login.
4. **FR4**: Invalid credentials should display an error message.
5. **FR5**: After 3 consecutive failed login attempts, the account should be locked.

---

### Test Case Specification Table

| Test Case ID | Test Scenario | Input Data | Expected Output |
| :--- | :--- | :--- | :--- |
| **TC_01** | Valid login with correct username and standard-length password (> 6 characters) | `Username = "user1"`<br>`Password = "pass123"`<br>Click `[Login]` button | Authentication successful. User is redirected to dashboard/home page with welcome message: *"Welcome, user1"*. Consecutive failed login counter is reset to 0. |
| **TC_02** | Valid login with password length equal to the boundary minimum of exactly 6 characters | `Username = "john_doe"`<br>`Password = "Secure"` (length = 6)<br>Click `[Login]` button | Authentication successful. System accepts 6-character boundary password and redirects user to dashboard/home page. |
| **TC_03** | Login attempt with empty username field | `Username = ""` (empty string)<br>`Password = "pass123"`<br>Click `[Login]` button | Login blocked. Inline error message displayed beneath username field: *"Username is required and cannot be empty."* Focus set to username field. No network request sent to authentication server. |
| **TC_04** | Login attempt with empty password field | `Username = "user1"`<br>`Password = ""` (empty string)<br>Click `[Login]` button | Login blocked. Inline error message displayed beneath password field: *"Password is required and cannot be empty."* Focus set to password field. No network request sent to authentication server. |
| **TC_05** | Login attempt with password length strictly less than 6 characters (boundary = 5 characters) | `Username = "user1"`<br>`Password = "12345"` (length = 5)<br>Click `[Login]` button | Login blocked. Form validation error message displayed: *"Password must be at least 6 characters long."* Password field is cleared, and focus is placed on the password input. |
| **TC_06** | Login attempt with both username and password fields left blank | `Username = ""` (empty string)<br>`Password = ""` (empty string)<br>Click `[Login]` button | Login blocked. Validation errors displayed simultaneously for both inputs: *"Username is required and cannot be empty"* and *"Password is required and cannot be empty."* |
| **TC_07** | Login attempt with unregistered/non-existent username | `Username = "ghost_user"`<br>`Password = "ValidPass123"`<br>Click `[Login]` button | Authentication fails. Generic security error message displayed: *"Invalid username or password. Please try again."* User remains on login page. |
| **TC_08** | Login attempt with registered username but incorrect password (Attempt 1 of 3) | `Username = "user1"`<br>`Password = "wrongPass99"`<br>Click `[Login]` button | Authentication fails. Error message displayed: *"Invalid username or password. 2 attempts remaining before account lockout."* System increments failed login attempt counter to 1. |
| **TC_09** | Account lockout triggered after 3 consecutive failed login attempts | **Attempt 1:** `Username = "user1"`, `Password = "bad1"`<br>**Attempt 2:** `Username = "user1"`, `Password = "bad2"`<br>**Attempt 3:** `Username = "user1"`, `Password = "bad3"`<br>Click `[Login]` button | On 3rd failed attempt, authentication fails and account status transitions to 'Locked'. Error message displayed: *"Account locked due to 3 consecutive failed login attempts. Please contact support or reset your password."* |
| **TC_10** | Login attempt with correct valid credentials on an already locked account | `Username = "user1"` (account locked)<br>`Password = "pass123"` (valid password)<br>Click `[Login]` button | Authentication blocked despite correct credentials. Error message displayed: *"Your account is currently locked. Please unlock your account via password reset or contact system administrator."* |
| **TC_11** | Successful login resets the consecutive failed attempts counter | **Attempt 1:** `Username = "user1"`, `Password = "wrong1"` (fails, counter = 1)<br>**Attempt 2:** `Username = "user1"`, `Password = "pass123"` (valid), Click `[Login]` | Login successful on Attempt 2. User redirected to dashboard. Consecutive failed login counter for `user1` resets to 0. |

---

### Functional Requirements Coverage Matrix

| Requirement | Description | Test Case Mapping |
| :--- | :--- | :--- |
| **FR1** | Username must not be empty | TC_03, TC_06 |
| **FR2** | Password must be at least 6 characters long | TC_02 (boundary = 6), TC_04 (empty), TC_05 (boundary = 5), TC_06 (both empty) |
| **FR3** | Valid username and password allow successful login | TC_01, TC_02, TC_11 |
| **FR4** | Invalid credentials display clear error message | TC_07, TC_08 |
| **FR5** | 3 consecutive failed attempts lock account | TC_08, TC_09, TC_10, TC_11 |

---

### Test Design Methodology
- **Equivalence Partitioning (EP)**:
  - *Valid Username Partition*: Registered alphanumeric usernames (`user1`).
  - *Invalid Username Partition*: Empty strings (`""`), unregistered usernames (`ghost_user`).
  - *Valid Password Partition*: Length $\ge 6$ (`pass123`, `Secure`).
  - *Invalid Password Partition*: Empty strings (`""`), length $< 6$ (`12345`), incorrect credentials.
- **Boundary Value Analysis (BVA)**:
  - Minimum boundary $L = 6$ characters (`Secure`) $\rightarrow$ Accepted.
  - Just below boundary $L = 5$ characters (`12345`) $\rightarrow$ Rejected.
- **State Transition Testing**:
  - State 1: Active (Failed Counter = 0)
  - State 2: Active (Failed Counter = 1 after Attempt 1)
  - State 3: Active (Failed Counter = 2 after Attempt 2)
  - State 4: Locked (Triggered when Counter reaches 3)
  - Counter Reset: Transition from State 2/3 back to State 1 on successful authentication.
