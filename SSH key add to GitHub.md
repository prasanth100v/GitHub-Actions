# Create an SSH key and add it to GitHub
 * 🚀 This is the recommended & secure way to authenticate with GitHub without entering username/password everytime

## ✅ Step 1: Check if SSH key already exists
```hcl
ls ~/.ssh
```
🌿 If you see files like `id_rsa` / `id_ed25519`, you may already have a key.

## ✅ Step 2: Generate a new SSH key (Recommended: `ed25519`)
```hcl
ssh-keygen -t ed25519 -C "your_email@example.com"
```

### 🔹 When asked:
```hcl
File location → press Enter  Passphrase → optional (recommended for security)
```
* 👉 This creates:
   * 🌿 Private key: `~/.ssh/id_ed25519` && Public key: `~/.ssh/id_ed25519.pub`

## ✅ Step 3: Start the `SSH agent` & add your `private key`
```hcl
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## ✅ Step 4: Copy the public SSH key
```hcl
cat ~/.ssh/id_ed25519.pub
```
📋 Copy the entire output (starts with `ssh-ed25519`).

## ✅ Step 5: Add SSH key to GitHub
```hcl
1 Go to GitHub → Settings → SSH and GPG keys
2. Click New SSH key
3. Paste the copied key
4. Title example: Ubuntu Laptop / Office Server
5. Click Add SSH key
```

## ✅ Step 6: Test SSH connection
```hcl
ssh -T git@github.com
```

## ✅ Step 7: Use SSH URL in Git
🌿 Check current remote:
```hcl
git remote -v
```
🚀 If HTTPS, change to SSH : 🔹 **Copy ssh from Github** 
```hcl
git remote set-url origin git@github.com:prasanth100v/my-project.git
```
* ⚡ It will switch your remote URL from `HTTPS to SSH `for that `repository`.
