#### 🔐 1. **Use SSH Authentication (Best for VPS)**
This is the most secure and convenient method for VPS environments.

##### Steps:
1. **Check if your VPS has an SSH key**:
   ```bash
   ls ~/.ssh/id_rsa.pub
   ```
   If it exists, you're good. If not, generate it:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```
   Press Enter to accept defaults.

2. **Copy the public key**:
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```
   Copy the whole key output.

3. **Add the key to GitHub**:
   - Go to **GitHub → Settings → SSH and GPG Keys**.
   - Click **New SSH key**, give it a name (like "VPS"), and paste the key.

4. **Update your remote URL to use SSH**:
   In your Git repo:
   ```bash
   git remote set-url origin git@github.com:USERNAME/REPO.git
   ```

5. **Now pull normally**:
   ```bash
   git pull
   ```
---
