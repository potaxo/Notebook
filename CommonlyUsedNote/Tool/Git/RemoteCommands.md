##### `Tldr: 
   ```bash
   ssh-keygen -a 100 -t ed25519 -f ~/.ssh/filename -C "Leave comments you want"
   ```
   
Config:   [[#**Using `~/.ssh/config`**]]
   
---
### situation:

GitHub **no longer allows password authentication for Git operations**. They only support either:

1. **SSH keys**
2. **HTTPS with a Personal Access Token (PAT)**
---
#### Option 1: Use SSH (recommended)

1. Generate an SSH key if you don’t have one:

   ```bash
   ssh-keygen -a 100 -t ed25519 -f ~/.ssh/filename -C "Leave comments you want"
   ```
##### `-a 100`

This sets the number of **KDF (Key Derivation Function) rounds** for protecting the private key with your passphrase.

* Think of it as how many times the program scrambles your passphrase before using it.
* The higher the number, the harder it is for an attacker to brute-force your passphrase if they steal your private key file.
* `100` is stronger than the default (usually 16), but not so high that it makes your logins annoyingly slow. Modern recommendations often use `100` or even `200`.
##### -c
Leaving meaningful comment, like `-c "work-server"`.

##### ed25519
[[#What is **ed25519**?]]

---

2. Start the ssh-agent and add the key:
- Or you can just use a more convenient way [[#**Using `~/.ssh/config`**]]

   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ```

3. Copy the public key:

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

   Add it to GitHub → Settings → SSH and GPG keys → "New SSH key".

4. Change your repo remote to SSH:

   ```bash
   git remote set-url origin git@github.com:potaxo/dotfiles.git
   ```

5. Now try:

   ```bash
   git push -u origin main
   ```

---

#### Option 2: Use HTTPS with Personal Access Token (PAT)

1. Go to GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic).
2. Generate a new token with `repo` permission.
3. When you push, use:

   * Username: your GitHub username (`potaxo`)
   * Password: your token (paste it instead of your real password)

If you don’t want to re-enter it every time:

```bash
git config --global credential.helper store
```

---
### 1. `eval "$(ssh-agent -s)"`

* `ssh-agent` is a background program that holds your **private SSH keys** in memory.
  This way, you don’t have to type your passphrase every single time you use the key.
* `-s` tells it to output shell commands that set up environment variables (like `SSH_AUTH_SOCK` and `SSH_AGENT_PID`) so your current shell knows how to talk to the agent.
* `eval` executes those shell commands in your current session.

So, this line **starts an ssh-agent process and connects your shell to it.**

---

### 2. `ssh-add ~/.ssh/id_ed25519`

* `ssh-add` is used to add a private key to the running `ssh-agent`.
* `~/.ssh/id_ed25519` is the default private key file you generated with `ssh-keygen`.

When you run this:

* If your private key has a passphrase, it will ask you for it once.
* After that, the agent keeps the decrypted key in memory, and you don’t have to retype the passphrase every time you push/pull to GitHub.

---
### What is **ed25519**?

* **ed25519** is a modern, secure default — best choice for GitHub today.
* You *can* use RSA or another type, but unless you need compatibility with some ancient system, stick with ed25519.

---
### Ways to identify your SSH keys

1. **By filename**

   * When generating keys, you can give them descriptive names:

   ```bash
     ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_github -C "your_github_email@example.com"
     ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_uni -C "student@university.edu"
     ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_server -C "server login"
     ```
   * This gives you separate files like:

     * `id_ed25519_github` / `id_ed25519_github.pub`
     * `id_ed25519_uni` / `id_ed25519_uni.pub`
     * `id_ed25519_server` / `id_ed25519_server.pub`

2. **By public key comment**

   * The `-C "comment"` you add during `ssh-keygen` is stored at the end of the `.pub` file. Example:

     ```
     ssh-ed25519 AAAAC3Nz...longkey... your_github_email@example.com
     ```
   * You can open `~/.ssh/id_ed25519_github.pub` and instantly see which key belongs to which service.

3. **Using `ssh-add -l`**

   * If you’ve added keys to the agent:

     ```bash
     ssh-add -l
     ```

     Output shows fingerprints and filenames:

     ```
     256 SHA256:abc123... /home/potaxo/.ssh/id_ed25519_github (ED25519)
     256 SHA256:def456... /home/potaxo/.ssh/id_ed25519_uni (ED25519)
     ```

### **Using `~/.ssh/config`**

   This file lets you map a key to a specific host. Example:

   ```ssh
   Host github.com
       User git
       HostName github.com
       IdentityFile ~/.ssh/id_ed25519_github
       AddKeysToAgent yes

   Host uni-server
       User student
       HostName ssh.university.edu
       IdentityFile ~/.ssh/id_ed25519_uni

   Host my-vps
       User root
       HostName 203.0.113.42
       IdentityFile ~/.ssh/id_ed25519_server
   ```

   Now you don’t need to remember which key goes where — SSH handles it for you.
   Running `ssh github.com` will automatically use the GitHub key.

---
### In short

* **Filename convention** = most straightforward way to distinguish.
* **Comments** inside `.pub` files = quick human-readable ID.
* **ssh-agent listing** = shows loaded keys.
* **ssh config** = the ultimate way to automate which key is used for which service.

---
Relevant Link: [[Git]] [[SSH]] 
Date: 2025-09-23 
Time: 20:37
