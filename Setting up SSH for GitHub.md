Setting up SSH for GitHub allows you to push, pull, and clone repositories without needing to input your GitHub password or a Personal Access Token (PAT) every time. This is especially useful for avoiding repeated authentication and for improving security. Here's a detailed step-by-step guide to setting up SSH for GitHub:

---

### Step 1: **Generate SSH Keys on Your Local Machine**

SSH keys are pairs of cryptographic keys used for authenticating with remote services like GitHub.

1. **Open your terminal**.

2. **Generate a new SSH key pair**:
   Run the following command (replace `"your-email@example.com"` with your GitHub email address):

   ```bash
   ssh-keygen -t rsa -b 4096 -C "your-email@example.com"
   ```

   * `-t rsa`: Specifies the type of key (RSA is commonly used).
   * `-b 4096`: Specifies the length of the key (4096 bits is secure).
   * `-C "your-email@example.com"`: This adds a comment to the key, typically your email address, to help identify the key.

3. **Choose the file location**:
   When prompted, press **Enter** to save the key to the default location, which is `~/.ssh/id_rsa`. If you already have an existing key and don’t want to overwrite it, you can specify a different filename (e.g., `~/.ssh/id_rsa_github`).

   ```bash
   Enter file in which to save the key (/home/youruser/.ssh/id_rsa):
   ```

4. **Set a passphrase** (optional):
   You will be prompted to enter a passphrase to encrypt your SSH key. It's optional but recommended for additional security. You can leave it empty if you prefer not to set one.

   ```bash
   Enter passphrase (empty for no passphrase):
   ```

   After this step, you will have two files:

   * `~/.ssh/id_rsa`: The private key (keep this secure).
   * `~/.ssh/id_rsa.pub`: The public key (this will be added to GitHub).

---

### Step 2: **Add Your SSH Key to the SSH Agent**

The **SSH agent** is a background process that manages your SSH keys and keeps them ready for use. You need to add your SSH private key to the agent.

1. **Start the SSH agent**:
   Run the following command to ensure the SSH agent is running:

   ```bash
   eval "$(ssh-agent -s)"
   ```

2. **Add your SSH private key to the agent**:
   Run the following command to add the private key to the agent:

   ```bash
   ssh-add ~/.ssh/id_rsa
   ```

   If you used a different filename for your key, replace `id_rsa` with the correct name (e.g., `id_rsa_github`).

---

### Step 3: **Add Your Public SSH Key to Your GitHub Account**

Now that you have your SSH key pair, you need to add the **public key** (`id_rsa.pub`) to your GitHub account.

1. **Copy the public key to your clipboard**:
   Run the following command to copy the contents of your public key:

   ```bash
   cat ~/.ssh/id_rsa.pub
   ```

   This will display the public key. You can either manually copy it or use `pbcopy` (macOS) or `xclip` (Linux) to copy it directly:

   ```bash
   # For macOS
   pbcopy < ~/.ssh/id_rsa.pub

   # For Linux
   xclip -sel clip < ~/.ssh/id_rsa.pub
   ```

2. **Log in to your GitHub account**.

3. **Navigate to your SSH settings**:

   * Click on your profile picture in the top-right corner.
   * Go to **Settings**.
   * In the left sidebar, click **SSH and GPG keys**.

4. **Add the SSH key**:

   * Click on **New SSH key**.
   * In the "Title" field, give your key a descriptive name (e.g., `My Laptop SSH Key`).
   * Paste your public key into the "Key" field.
   * Click **Add SSH key**.

---

### Step 4: **Test Your SSH Connection to GitHub**

You should test whether your SSH key is correctly set up by running the following command:

```bash
ssh -T git@github.com
```

This command will attempt to connect to GitHub using SSH. If everything is set up correctly, you should see a message like this:

```bash
Hi ezhilbharathi1806! You've successfully authenticated, but GitHub does not provide shell access.
```

If you see a message like this, it means your SSH connection is working.

---

### Step 5: **Change Your Git Remote URL to Use SSH**

Once SSH is set up, you need to change your Git repository’s remote URL to use SSH instead of HTTPS. This way, you'll authenticate with your SSH key rather than using a username and password (or PAT).

1. **Navigate to your repository**:
   Go to your project directory on your local machine.

2. **Update the Git remote URL to use SSH**:
   Run the following command to set the remote URL for your Git repository to use SSH:

   ```bash
   git remote set-url origin git@github.com:ezhilbharathi1806/pico_sdk_workspace.git
   ```

   This changes the remote URL for your repository to the SSH format (`git@github.com:username/repository.git`), replacing the HTTPS format.

3. **Verify the change**:
   You can verify that the remote URL has been updated by running:

   ```bash
   git remote -v
   ```

   This should show the new SSH URL for `origin`:

   ```bash
   origin  git@github.com:ezhilbharathi1806/pico_sdk_workspace.git (fetch)
   origin  git@github.com:ezhilbharathi1806/pico_sdk_workspace.git (push)
   ```

---

### Step 6: **Push to GitHub Using SSH**

Now that your repository is set up to use SSH, you can push your code without needing to enter your username or PAT.

1. **Push your changes**:
   Run the usual `git push` command to push your changes to GitHub:

   ```bash
   git push
   ```

   Since SSH is used for authentication, you won’t be prompted for a username or password (unless your key has a passphrase, in which case you’ll be prompted for it once).

---

### Summary of the SSH Setup Process:

1. **Generate an SSH key** on your local machine using `ssh-keygen`.
2. **Add your SSH private key** to the SSH agent using `ssh-add`.
3. **Add the public key** to your GitHub account via GitHub settings.
4. **Test the SSH connection** to GitHub with `ssh -T git@github.com`.
5. **Change your repository's remote URL** to use SSH instead of HTTPS.
6. **Push to GitHub** using `git push` without needing to input a password or token.

Once you’ve completed these steps, you can push and pull from GitHub using SSH, and you won’t need to enter your username or token anymore.

