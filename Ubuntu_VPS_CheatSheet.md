# Ubuntu VPS Development & Linux Commands Guide

*Quick reference guide for development server setup, directory structures, Docker workflows, and Linux command essentials.*

> **Server Overview (ls-infra-dev01):**  
> RAM: **311 GB** (~308 GB free) | Disk: **98 GB** | OS: **Ubuntu Linux** | Primary Dev Path: `/home/tarun/apps/`

---

## 1. Recommended Server Directory Structure

```text
/home/tarun/
├── apps/                 <-- All source code & git repositories live here
│   ├── clarify/          <-- App 1: Source code, .env, docker-compose.yml
│   └── open-seo/         <-- App 2: Source code, .env, docker-compose.yml
│
├── docker/               <-- Infrastructure & shared services
│   ├── compose/          <-- Multi-container services (Reverse proxy, central DBs)
│   └── volumes/          <-- Persistent database storage folders
│
├── scripts/              <-- Maintenance & automated backup scripts
├── logs/                 <-- App and system logs
└── backups/              <-- Database dumps and snapshots
```

---

## 2. Everyday Development Workflow

1. **Navigate to apps:** `cd ~/apps`
2. **Clone project:** `git clone https://github.com/user/project.git`
3. **Enter project:** `cd project`
4. **Setup environment:** `cp .env.example .env && nano .env`
5. **Run with Docker:** `docker compose up -d`
6. **Stop container:** `docker compose down`

---

## 3. File & Directory Management Commands

### A. Navigation & Location
| Goal | Command | Example |
| :--- | :--- | :--- |
| Where am I? | `pwd` | `pwd` |
| Go to directory | `cd <dir>` | `cd apps` |
| Go back one level | `cd ..` | `cd ..` |
| Go to user home (`/home/tarun`) | `cd ~` | `cd ~` |
| List files & hidden files | `ls -la` | `ls -la` |
| View directory tree | `tree -L 2` | `tree -L 2` |

### B. Creating Files & Folders
| Goal | Command | Example |
| :--- | :--- | :--- |
| Create a folder | `mkdir <folder>` | `mkdir clarify` |
| Create nested folders | `mkdir -p <path>` | `mkdir -p apps/clarify/data` |
| Create empty file | `touch <file>` | `touch .env` |
| Create file with text | `echo "text" > <file>` | `echo "PORT=3000" > .env` |
| Create & edit file | `nano <file>` | `nano docker-compose.yml` |

### C. Copying & Moving
| Goal | Command | Example |
| :--- | :--- | :--- |
| Copy a file | `cp <src> <dest>` | `cp .env.example .env` |
| Copy an entire folder | `cp -r <src_dir> <dest_dir>` | `cp -r clarify clarify-backup` |
| Rename a file / folder | `mv <old> <new>` | `mv temp-app clarify` |
| Move file into folder | `mv <file> <folder>/` | `mv .env apps/clarify/` |

### D. Deleting Files & Folders
> ⚠️ **Warning:** Deletions in Linux are permanent (no Recycle Bin). Double-check before pressing Enter.

| Goal | Command | Example |
| :--- | :--- | :--- |
| Delete a single file | `rm <file>` | `rm old.log` |
| Delete an empty folder | `rmdir <folder>` | `rmdir empty-folder` |
| Delete folder and all contents | `rm -rf <folder>` | `rm -rf old-project/` |

### E. Viewing & Searching Files
| Goal | Command | Description |
| :--- | :--- | :--- |
| Print entire file content | `cat <file>` | Prints content to terminal |
| Scroll through long file | `less <file>` | Press `q` to exit |
| Watch logs update live | `tail -f <file>` | Press `Ctrl + C` to stop |
| Find file by name | `find . -name "<name>"` | `find . -name "*.env"` |
| Search text inside files | `grep -rn "<text>" .` | `grep -rn "DB_PORT" .` |

---

## 4. System Installation Commands (Run Once)

```bash
# 1. Update OS and install Git & basic utilities
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl wget unzip htop build-essential tree

# 2. Install Docker & Docker Compose
sudo apt install -y docker.io docker-compose-v2
sudo usermod -aG docker $USER
sudo systemctl enable --now docker
newgrp docker

# 3. Install Node.js LTS via NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install --lts
npm install -g pnpm pm2

# 4. Verify everything
git --version && docker --version && node -v && pnpm -v
```

---

## 5. Essential Keyboard Shortcuts

* `Tab`: Auto-complete file and directory names.
* `Up / Down Arrows`: Browse through previously executed commands.
* `Ctrl + C`: Kill / cancel any stuck or currently running command.
* `Ctrl + L`: Clear the terminal screen.
* **Nano Editor:** Press `Ctrl + O` then `Enter` to save; press `Ctrl + X` to exit.

