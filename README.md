# 📦 Post-Installation Guide — Deepin OS 25

This guide aims to prepare the **Deepin OS 25** environment for general use and development. It includes installation of essential tools like Docker, multiple versions of Java, Node.js, editors, fonts, firewall, Flatpak, and more.

> ⚠️ **Run each command carefully.** For `sudo` commands, you will need to provide your password.

---

## 🔁 Update the system

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🐳 Install Docker (no need to use `sudo`)

```bash
# Create directory for the key if it doesn't exist
sudo install -m 0755 -d /etc/apt/keyrings

# Download and save Docker's GPG key
curl -fsSL https://download.docker.com/linux/debian/gpg |   sudo gpg --dearmor -o /etc/apt/keyrings/docker.asc

# Add official Docker repository for Debian Bookworm
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc]   https://download.docker.com/linux/debian   $(. /etc/os-release && echo \"bookworm\") stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update repositories
sudo apt update

# Install Docker and plugins
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Add current user to the docker group
# Replace $USER with your username if needed
sudo usermod -aG docker $USER

# Update Docker socket permissions (optional, immediate effect)
sudo chmod 666 /var/run/docker.sock
```

> ✅ **Important**: After this step, **log out and log in again**, or run:
```bash
newgrp docker
```
> This applies the new group membership so you can run `docker` without `sudo`.

> Verify with:
```bash
docker run hello-world
```

---

## ☕ Install Java (OpenJDK 11 through 21)

```bash
sudo apt install -y openjdk-11-jdk openjdk-17-jdk openjdk-21-jdk

# Check installed versions
update-java-alternatives --list

### 1. Set Oracle JDK as Default (optional)

$ sudo update-alternatives --config java

### 2. Edit Environment

$ sudo nano /etc/environment

# Add the following line (example for Java 17)
JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64"

### Save (Ctrl+O, Enter, Ctrl+X).

### 3. Reload Environment

$ source /etc/environment

### 4. Verify

$ echo $JAVA_HOME

### 5. Testing 

$ javac --version
```

---

## 🟢 Install Node.js (latest version)

```bash
# Download and install nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# in lieu of restarting the shell
\. "$HOME/.nvm/nvm.sh"

# Download and install Node.js:
nvm install 22

# Verify the Node.js version:
node -v # Should print "v22.17.0".
nvm current # Should print "v22.17.0".

# Verify npm version:
npm -v # Should print "10.9.2".

```

---

## 🛠️ Install text editors (vim, neovim, nano)

```bash
sudo apt install -y vim neovim nano
```

---

## 🔥 Configure firewall (ufw)

```bash
# Install
sudo apt install -y ufw

# Enable and allow basic connections
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh

# Enable firewall
sudo ufw enable

# Check status
sudo ufw status verbose
```

---

## 📟 Install Neofetch

```bash
sudo apt install -y neofetch

# Run
neofetch
```

---

## 🧬 Install Git

```bash
sudo apt install -y git

# Initial configuration
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

---

## 🔤 Install Microsoft fonts

```bash
sudo apt install -y ttf-mscorefonts-installer
```

> Accept Microsoft terms during installation.

---

## 🧩 Useful extra packages

```bash
sudo apt install -y   build-essential   curl   wget   unzip   zip   net-tools   gnupg2   software-properties-common   htop   gparted   lsb-release
```

---

## 📦 Install Flatpak and configure Flathub

```bash
# Install flatpak
sudo apt install -y flatpak

# Add Flathub repository (main source of flatpak apps)
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# Restart system or logout/login to apply flatpak changes
```

### Suggested Flatpak applications

```bash
# Install common Flatpak apps (examples)
flatpak install flathub com.visualstudio.code          # VS Code
flatpak install flathub org.gimp.GIMP                   # GIMP - image editor
flatpak install flathub org.videolan.VLC                 # VLC - media player
flatpak install flathub com.spotify.Client               # Spotify
flatpak install flathub org.mozilla.firefox               # Firefox updated via flatpak
flatpak install flathub com.slack.Slack                   # Slack - communication
flatpak install flathub org.telegram.desktop              # Telegram
```

---

## ✔️ Finalization

After completing all steps:

- Restart your system if prompted.
- Verify Docker and Java are working correctly:
```bash
docker run hello-world
java -version
```
- Ensure `neofetch`, `vim`, `git`, `node`, and `flatpak` are accessible:
```bash
neofetch
vim --version
git --version
node -v
flatpak --version
```

---

### ✅ System ready for general use and development!
