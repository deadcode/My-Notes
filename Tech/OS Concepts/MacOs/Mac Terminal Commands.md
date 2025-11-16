# Application Permissions
* Give permissions to application and remove OS qurantine
```Shell
sudo find <App Folder> -type f -exec xattr -d com.apple.quarantine "{}" \;
```
# Disable SIP (System Integrity Protection)

**1.** Put the Mac into Recovery Mode (hold down command+R during startup).
**2.** Go to the Utilities menu and open Terminal and type: **csrutil disable**. This will disable SIP (System Integrity Protection).
```shell
csrutil disable
```

**3.** Reboot into the OS.