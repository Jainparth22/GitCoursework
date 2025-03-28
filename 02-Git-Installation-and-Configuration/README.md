# Module 02: Git Installation & Configuration

> **Level**: 🟢 Beginner | **Time**: 2 hours | **Prerequisites**: [Module 01](../01-Introduction-to-Version-Control/README.md)

---

## 📋 Learning Objectives

- Install Git on Windows, macOS, and Linux
- Understand the Git configuration hierarchy
- Set up essential configuration for development
- Configure SSH keys for GitHub authentication
- Set up GPG signing for verified commits

---

## 1. Installing Git

### How the Installation Process Works

```mermaid
flowchart TD
    A[Start] --> B{Which OS?}
    B -->|Windows| C[Download Git for Windows]
    B -->|macOS| D[Xcode CLI Tools or Homebrew]
    B -->|Linux| E[Package Manager]
    
    C --> C1[Includes Git Bash, GUI, Shell Integration]
    D --> D1[Includes Git + developer tools]
    E --> E1[Lightweight Git install]
    
    C1 --> F[Verify: git --version]
    D1 --> F
    E1 --> F
    F --> G{Version shown?}
    G -->|Yes| H[✅ Git Installed]
    G -->|No| I[❌ Check PATH / Reinstall]
    
    style H fill:#51cf66
    style I fill:#ff6b6b
```

### Windows

**Option A: Git for Windows (Recommended)**

1. Download from [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Run the installer with these recommended settings:

```mermaid
flowchart TD
    A[Run Installer] --> B["Component Selection<br/>✅ Git Bash Here<br/>✅ Git GUI Here<br/>✅ Git LFS"]
    B --> C["PATH Selection<br/>✅ Git from command line<br/>AND 3rd-party software"]
    C --> D["SSH Selection<br/>✅ Use bundled OpenSSH"]
    D --> E["HTTPS Backend<br/>✅ Use OpenSSL library"]
    E --> F["Line Endings<br/>✅ Checkout Windows-style,<br/>commit Unix-style"]
    F --> G["Terminal Emulator<br/>✅ Use MinTTY"]
    G --> H["Default Branch<br/>✅ Override: main"]
    H --> I[Install Complete]
    
    style I fill:#51cf66
```

3. Verify:
```bash
git --version
# Output: git version 2.x.x.windows.x
```

**Option B: winget (Windows 11)**

```bash
winget install --id Git.Git -e --source winget
```

### macOS

**Option A: Xcode Command Line Tools**

```bash
xcode-select --install
git --version
```

**Option B: Homebrew**

```bash
brew install git
git --version
```

### Linux

**Ubuntu / Debian:**
```bash
sudo apt update && sudo apt install git
```

**Fedora:**
```bash
sudo dnf install git
```

**Arch Linux:**
```bash
sudo pacman -S git
```

---

## 2. Git Configuration System — How It Works Internally

Git stores configuration at **three levels**, each overriding the previous:

```mermaid
graph TB
    subgraph "Configuration Hierarchy (highest priority on top)"
        LOCAL["🟢 LOCAL<br/>.git/config<br/>This repository only"]
        GLOBAL["🟡 GLOBAL<br/>~/.gitconfig<br/>Your user account"]
        SYSTEM["🔴 SYSTEM<br/>/etc/gitconfig<br/>All users on machine"]
    end
    
    LOCAL -->|overrides| GLOBAL
    GLOBAL -->|overrides| SYSTEM
    
    style LOCAL fill:#51cf66,stroke:#333
    style GLOBAL fill:#ffd43b,stroke:#333
    style SYSTEM fill:#ff6b6b,stroke:#333
```

### How Git Resolves Configuration

```mermaid
sequenceDiagram
    participant Git as Git Command
    participant L as .git/config (Local)
    participant G as ~/.gitconfig (Global)
    participant S as /etc/gitconfig (System)
    
    Git->>L: Check local config first
    alt Found in local
        L-->>Git: Return local value ✅
    else Not found
        Git->>G: Check global config
        alt Found in global
            G-->>Git: Return global value ✅
        else Not found
            Git->>S: Check system config
            alt Found in system
                S-->>Git: Return system value ✅
            else Not found
                S-->>Git: Use default value
            end
        end
    end
```

### Viewing Configuration

```bash
# View all config with source locations
git config --list --show-origin

# View specific level
git config --local --list
git config --global --list
git config --system --list

# View a specific key
git config user.name
git config user.email
```

### How Config Files Look Internally

```ini
# ~/.gitconfig (Global)
[user]
    name = Parth
    email = parth@example.com
[core]
    editor = code --wait
    autocrlf = true
[init]
    defaultBranch = main
[alias]
    lg = log --oneline --graph --all
```

---

## 3. Essential Configuration — First-Time Setup

```mermaid
flowchart TD
    A["First Git Install"] --> B["Set Identity"]
    B --> C["git config --global user.name"]
    B --> D["git config --global user.email"]
    
    C --> E["Set Editor"]
    D --> E
    E --> F["git config --global core.editor"]
    
    F --> G["Set Default Branch"]
    G --> H["git config --global init.defaultBranch main"]
    
    H --> I["Set Line Endings"]
    I --> J{OS?}
    J -->|Windows| K["autocrlf = true"]
    J -->|macOS/Linux| L["autocrlf = input"]
    
    K --> M["Ready to Use Git ✅"]
    L --> M
    
    style M fill:#51cf66
```

### Identity (Required)

```bash
# Your name (shown in commits)
git config --global user.name "Your Full Name"

# Your email (must match GitHub account for linking)
git config --global user.email "your.email@example.com"
```

### Default Editor

```bash
# VS Code
git config --global core.editor "code --wait"

# Vim
git config --global core.editor "vim"

# Nano
git config --global core.editor "nano"

# Notepad++ (Windows)
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession"
```

### Default Branch Name

```bash
git config --global init.defaultBranch main
```

### Color Output

```bash
git config --global color.ui auto
```

---

## 4. Line Endings — How They Work

Different OS use different characters for line breaks. This causes problems in cross-platform teams.

```mermaid
flowchart LR
    subgraph "Operating Systems"
        W["Windows<br/>CRLF: \\r\\n<br/>(2 bytes)"]
        M["macOS / Linux<br/>LF: \\n<br/>(1 byte)"]
    end
    
    subgraph "Git Repository"
        R["Always stores LF \\n<br/>(normalized)"]
    end
    
    W -->|"commit: CRLF → LF"| R
    R -->|"checkout: LF → CRLF"| W
    M -->|"commit: LF → LF"| R
    R -->|"checkout: LF → LF"| M
    
    style R fill:#51cf66
    style W fill:#74c0fc
    style M fill:#74c0fc
```

### Configuration

```bash
# Windows: Convert CRLF→LF on commit, LF→CRLF on checkout
git config --global core.autocrlf true

# macOS/Linux: Convert CRLF→LF on commit, no conversion on checkout
git config --global core.autocrlf input
```

### How `core.autocrlf` Works Internally

```mermaid
flowchart TD
    subgraph "core.autocrlf = true (Windows)"
        W1["Working Directory<br/>CRLF (\\r\\n)"]
        W2["Repository<br/>LF (\\n)"]
        W1 -->|"git add<br/>CRLF → LF"| W2
        W2 -->|"git checkout<br/>LF → CRLF"| W1
    end
    
    subgraph "core.autocrlf = input (macOS/Linux)"
        L1["Working Directory<br/>LF (\\n)"]
        L2["Repository<br/>LF (\\n)"]
        L1 -->|"git add<br/>CRLF → LF (if found)"| L2
        L2 -->|"git checkout<br/>No conversion"| L1
    end
    
    subgraph "core.autocrlf = false"
        F1["Working Directory<br/>Whatever"]
        F2["Repository<br/>Whatever"]
        F1 -->|"No conversion"| F2
        F2 -->|"No conversion"| F1
    end
```

---

## 5. SSH Key Setup for GitHub

SSH keys provide **secure, password-less authentication** with GitHub.

### How SSH Authentication Works

```mermaid
sequenceDiagram
    participant You as Your Computer
    participant GH as GitHub Server
    
    Note over You: Has Private Key 🔑<br/>(~/.ssh/id_ed25519)
    Note over GH: Has Public Key 🔓<br/>(in your account settings)
    
    You->>GH: 1. Initiate connection (git push)
    GH->>You: 2. Send challenge (random data)
    You->>You: 3. Sign challenge with Private Key 🔑
    You->>GH: 4. Send signed response
    GH->>GH: 5. Verify with Public Key 🔓
    GH-->>You: 6. ✅ Authentication successful
    
    Note over You,GH: Private key NEVER leaves your machine
```

### Generate SSH Key

```bash
# Generate Ed25519 key (recommended)
ssh-keygen -t ed25519 -C "your.email@example.com"

# When prompted:
#   File: Press Enter (default location ~/.ssh/id_ed25519)
#   Passphrase: Enter a strong passphrase (recommended)
```

### Key Files Explained

```mermaid
graph LR
    subgraph "~/.ssh/ directory"
        PRIV["id_ed25519<br/>🔑 PRIVATE KEY<br/>(NEVER share this!)"]
        PUB["id_ed25519.pub<br/>🔓 PUBLIC KEY<br/>(Add to GitHub)"]
    end
    
    PUB -->|"Copy to"| GH["GitHub Settings →<br/>SSH and GPG Keys →<br/>New SSH Key"]
    
    style PRIV fill:#ff6b6b,stroke:#333
    style PUB fill:#51cf66,stroke:#333
    style GH fill:#74c0fc,stroke:#333
```

### Add Key to SSH Agent

```bash
# Start ssh-agent
eval "$(ssh-agent -s)"

# Add your key
ssh-add ~/.ssh/id_ed25519
```

### Add Public Key to GitHub

```bash
# Copy public key to clipboard
# Windows:
clip < ~/.ssh/id_ed25519.pub

# macOS:
pbcopy < ~/.ssh/id_ed25519.pub

# Linux:
cat ~/.ssh/id_ed25519.pub
# Then copy the output manually
```

Then: **GitHub → Settings → SSH and GPG Keys → New SSH Key → Paste**

### Test Connection

```bash
ssh -T git@github.com
# Output: Hi username! You've successfully authenticated
```

---

## 6. Credential Helper

For HTTPS connections (alternative to SSH):

```bash
# Cache credentials in memory (15 min default)
git config --global credential.helper cache

# Cache for 1 hour
git config --global credential.helper 'cache --timeout=3600'

# Windows: Use Windows Credential Manager
git config --global credential.helper wincred

# macOS: Use Keychain
git config --global credential.helper osxkeychain
```

---

## 7. GPG Commit Signing

Prove that commits are genuinely from you (shows "Verified" badge on GitHub):

```mermaid
flowchart LR
    C["Your Commit"] -->|"Sign with GPG Key"| SC["Signed Commit"]
    SC -->|"Push to GitHub"| GH["GitHub verifies<br/>signature"]
    GH --> V["✅ Verified Badge"]
    
    style V fill:#51cf66
```

```bash
# Generate GPG key
gpg --full-generate-key
# Choose: RSA and RSA, 4096 bits, your GitHub email

# List keys
gpg --list-secret-keys --keyid-format=long

# Configure Git to use GPG key
git config --global user.signingkey YOUR_KEY_ID
git config --global commit.gpgsign true

# Export public key (add to GitHub)
gpg --armor --export YOUR_KEY_ID
```

---

## 8. Recommended Aliases

```bash
git config --global alias.s "status -s"
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.co "checkout"
git config --global alias.br "branch"
git config --global alias.cm "commit -m"
git config --global alias.last "log -1 HEAD --stat"
git config --global alias.unstage "restore --staged"
git config --global alias.amend "commit --amend --no-edit"
```

### How Aliases Work

```mermaid
flowchart LR
    A["git lg"] -->|"Git reads alias"| B["git log --oneline<br/>--graph --all --decorate"]
    C["git s"] -->|"Git reads alias"| D["git status -s"]
    
    style A fill:#74c0fc
    style C fill:#74c0fc
    style B fill:#51cf66
    style D fill:#51cf66
```

---

## 9. Complete First-Time Setup Script

```bash
#!/bin/bash
# first-time-git-setup.sh

# Identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Editor
git config --global core.editor "code --wait"

# Default branch
git config --global init.defaultBranch main

# Line endings (change based on OS)
git config --global core.autocrlf true    # Windows
# git config --global core.autocrlf input  # macOS/Linux

# Colors
git config --global color.ui auto

# Merge conflict style
git config --global merge.conflictstyle diff3

# Pull strategy
git config --global pull.rebase true

# Aliases
git config --global alias.s "status -s"
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.co "checkout"
git config --global alias.br "branch"
git config --global alias.cm "commit -m"
git config --global alias.last "log -1 HEAD --stat"
git config --global alias.unstage "restore --staged"

echo "✅ Git configured successfully!"
git config --list
```

---

## 🏋️ Exercises

1. Install Git and verify with `git --version`
2. Run the first-time setup: name, email, editor, default branch
3. Generate an SSH key and add it to your GitHub account
4. Test SSH connection: `ssh -T git@github.com`
5. Create 3 custom aliases and test them
6. Run `git config --list --show-origin` to see all your settings

---

## 🔑 Key Takeaways

1. Git config has 3 levels: **system → global → local** (local wins)
2. Always set `user.name` and `user.email` before your first commit
3. SSH keys are more secure and convenient than HTTPS passwords
4. `core.autocrlf` prevents line ending issues in cross-platform teams
5. Aliases save time — `git lg` is better than typing the full log command
6. GPG signing adds a "Verified" badge to your commits on GitHub

---

**[← Module 01](../01-Introduction-to-Version-Control/README.md)** | **[Module 03 →](../03-Your-First-Repository/README.md)**
