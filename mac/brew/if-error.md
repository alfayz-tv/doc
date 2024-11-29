To change Homebrew services from running as `root` to running under your user account, you can adjust the configuration for the service in question. Here's how to do it for `nginx` or any other service:

---

### 1. **Stop the Current Service**
Stop the service if it's already running under `root`:
```bash
sudo brew services stop nginx
```

---

### 2. **Edit the Service Configuration**
Homebrew services use `.plist` files to configure how the service runs. Ensure the `nginx` service plist file is in the correct user directory:

- For services running as a user:
  ```
  ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist
  ```

- For services running as `root`:
  ```
  /Library/LaunchDaemons/homebrew.mxcl.nginx.plist
  ```

Move the `plist` file to your user's `LaunchAgents` directory if it's in the `LaunchDaemons` directory:
```bash
sudo mv /Library/LaunchDaemons/homebrew.mxcl.nginx.plist ~/Library/LaunchAgents/
```

Ensure ownership and permissions are set correctly:
```bash
sudo chown $(whoami):staff ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist
chmod 644 ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist
```

---

### 3. **Load the Service as Your User**
Load the service under your user:
```bash
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist
launchctl enable gui/$(id -u)/homebrew.mxcl.nginx
```

Check the status to ensure it’s running under your user:
```bash
launchctl list | grep homebrew.mxcl.nginx
```

---

### 4. **Start the Service**
Start the service using Homebrew, ensuring it uses the user's plist file:
```bash
brew services start nginx
```

This should now run the service under your user account instead of `root`.

---

### 5. **Verify**
To confirm the service is running as your user:
```bash
ps aux | grep nginx
```
You should see processes for `nginx` running under your username.

---

### Notes
- If you encounter issues with permissions, make sure the directories and files Nginx needs to access are readable and writable by your user.
- If you still need to switch between `root` and user modes frequently, you can use the `--all` flag with `brew services` to ensure all services run under the same context.