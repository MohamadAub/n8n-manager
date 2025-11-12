# 🧠 n8n-manager: Backup & Restore for n8n Docker via GitHub

![Banner](.github/images/Banner.png)

`n8n-manager` is a professional-grade shell-based automation utility built to **simplify, automate, and secure** the backup and restoration of your [n8n](https://n8n.io/) Docker instances via **GitHub integration**. It supports interactive and non-interactive modes, robust error handling, and fully automated CI/CD workflows.

---

## 🚀 Key Features

- **Interactive CLI Interface** – intuitive menus for container & action selection.  
- **Non-Interactive Mode** – full automation via arguments, ideal for cron/CI/CD.  
- **GitHub Backup Integration** – securely sync workflows, credentials, and env vars.  
- **Dated Backups** – maintain version history using timestamped subdirectories.  
- **Selective Restore** – restore `workflows`, `credentials`, or both.  
- **Automatic Rollback** – rollback to last known good state on restore failure.  
- **Dry Run Mode** – simulate operations safely.  
- **Cross-Platform Compatibility** – works with Alpine, Ubuntu, and Debian containers.  
- **Detailed Logging** – multi-level logs with optional file output.  

---

## ⚙️ Requirements

| Component | Required Version | Purpose |
|------------|------------------|----------|
| **Docker** | Latest stable | Container management |
| **Git** | v2.20+ | Versioning & GitHub sync |
| **curl** | Latest stable | API communication |
| **bash** | v4.0+ | Script execution |

---

## 📦 Installation

### Option 1: Quick Install (Recommended)
```bash
curl -sSL -L https://i.n8n.community | sudo bash
```

### Option 2: Manual Installation
```bash
git clone https://github.com/MohamadAub/n8n-manager.git
cd n8n-manager
chmod +x n8n-manager.sh
sudo mv n8n-manager.sh /usr/local/bin/n8n-manager
```

---

## 🧩 Configuration File

Default location:
```
~/.config/n8n-manager/config
```

Example content:
```ini
CONF_GITHUB_TOKEN="ghp_YourGitHubPAT"
CONF_GITHUB_REPO="MohamadAub/n8n-backups"
CONF_GITHUB_BRANCH="main"
CONF_DEFAULT_CONTAINER="n8n"
CONF_DATED_BACKUPS=true
CONF_RESTORE_TYPE="all"
CONF_VERBOSE=false
CONF_LOG_FILE="/var/log/n8n-manager.log"
```

Secure the configuration file:
```bash
chmod 600 ~/.config/n8n-manager/config
```

---

## 💡 Usage Examples

### Interactive Mode
```bash
n8n-manager
```

### Automated Backup
```bash
n8n-manager --action backup   --container my-n8n-container   --token "ghp_YourToken"   --repo "MohamadAub/n8n-backups"   --branch main   --dated
```

### Automated Restore (Workflows Only)
```bash
n8n-manager --action restore   --container my-n8n-container   --token "ghp_YourToken"   --repo "MohamadAub/n8n-backups"   --branch main   --restore-type workflows
```

---

## 🔄 Process Overview

### **Backup Flow**
1. Detect container and validate GitHub access.  
2. Export n8n workflows, credentials, and `.env`.  
3. Commit & push securely to GitHub.  
4. Clean up temporary files.  

### **Restore Flow**
1. Create a pre-restore backup.  
2. Clone from GitHub and validate data.  
3. Import workflows & credentials.  
4. Roll back automatically on failure.  

---

## 🪶 Logging and Debugging

- `--verbose` → detailed output.  
- `--dry-run` → simulate without modifying data.  
- `--trace` → show executed bash commands.  
- `--log-file` → log everything to a file.  

---

## 🧱 CI/CD Integration

n8n-manager can run inside automated pipelines for periodic or event-driven backups.

Example GitHub Actions workflow (`.github/workflows/backup.yml`):
```yaml
name: Automated n8n Backup
on:
  schedule:
    - cron: "0 2 * * *"
jobs:
  backup:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repo
        uses: actions/checkout@v3
      - name: Run Backup
        run: |
          docker ps
          n8n-manager --action backup             --container my-n8n-container             --token ${{ secrets.GH_PAT }}             --repo "MohamadAub/n8n-backups"             --branch main --dated
```

---

## 🔐 Security & Best Practices

- Use **GitHub PAT** with only `repo` scope.  
- Keep configuration file permissions strict (`chmod 600`).  
- Use **private repositories** for sensitive workflow data.  
- Regularly rotate your PAT.  

---

## 🧩 Project Structure

```
n8n-manager/
├── install.sh
├── n8n-manager.sh
├── .gitignore
├── .github/workflows/
├── CHANGELOG.md
└── README.md
```

---

## 🤝 Contributing

Pull requests, issues, and feature suggestions are welcome.  
Visit → [https://github.com/MohamadAub/n8n-manager](https://github.com/MohamadAub/n8n-manager)

---

## 📜 License

Licensed under the **MIT License**.  
© 2025 [Mohamad El Ayoubi](https://github.com/MohamadAub)
