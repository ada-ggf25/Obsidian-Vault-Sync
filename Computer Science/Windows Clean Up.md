#computer_science #os #guide 

To enhance and clean your old Windows 10 PC using built-in command-line tools, follow these steps. These commands can help free up disk space, repair system files, and optimize performance.

---

### 🧹 1. Clean Up Temporary and System Files

**Disk Cleanup Utility:**

- **Basic Cleanup:**
    
    ```
    cmd
    CopyEdit
    cleanmgr
    
    ```
    

This opens the Disk Cleanup tool, allowing you to select and delete unnecessary files.

- **Advanced Cleanup with Preset Options:**
    
    ```
    cmd
    CopyEdit
    cleanmgr /sageset:1
    cleanmgr /sagerun:1
    
    ```
    

The first command lets you choose cleanup options to save under the preset '1'. The second command runs the cleanup using those saved settings.

**Manual Deletion of Temporary Files:**

```
cmd
CopyEdit
del /q/f/s %TEMP%\\*
del /q/f/s C:\\Windows\\Temp\\*

```

These commands delete files from temporary directories. Some files in use may not be deleted.

---

### 🛠️ 2. Repair System Files

**System File Checker (SFC):**

```
cmd
CopyEdit
sfc /scannow

```

Scans and repairs corrupted system files. This process may take some time.

**Deployment Imaging Service and Management Tool (DISM):**

```
cmd
CopyEdit
DISM /Online /Cleanup-Image /RestoreHealth

```

Repairs the system image, which can help resolve issues that SFC cannot fix.

---

### 🧰 3. Optimize Storage and Performance

**Component Store Cleanup:**

```
cmd
CopyEdit
Dism.exe /Online /Cleanup-Image /StartComponentCleanup /ResetBase

```

Reduces the size of the WinSxS folder by removing superseded versions of system files. [TenForums](https://www.tenforums.com/performance-maintenance/196167-list-useful-commands.html?utm_source=chatgpt.com)

**Defragment Hard Drive (For HDDs Only):**

```
cmd
CopyEdit
defrag C: /O

```

Optimizes and defragments the specified drive. Not recommended for SSDs.

**Adjust Virtual Memory (Pagefile) Settings:**

```
cmd
CopyEdit
wmic computersystem where name="%computername%" set AutomaticManagedPagefile=False
wmic pagefileset where name="C:\\\\pagefile.sys" set InitialSize=4096,MaximumSize=8192

```

Sets custom initial and maximum sizes for the pagefile. Adjust values based on your system's RAM. [Reddit](https://www.reddit.com/r/pchelp/comments/xlc2gb/speed_up_your_pc_free/?utm_source=chatgpt.com)

---

### 🚀 4. Manage Startup Programs

**Disable Unnecessary Startup Programs:**

- Open Task Manager (`Ctrl + Shift + Esc`).
- Navigate to the 'Startup' tab.
- Right-click and disable programs that you don't need to start automatically. [Reddit](https://www.reddit.com/r/pchelp/comments/xlc2gb/speed_up_your_pc_free/?utm_source=chatgpt.com)[Reddit+2Microsoft Learn+2outbyte.com+2](https://learn.microsoft.com/en-us/answers/questions/1338760/how-to-clean-up-computer-to-run-faster-windows-10?utm_source=chatgpt.com)

---

### 🔍 5. Additional Tips

- **Check Disk for Errors:**
    
    ```
    cmd
    CopyEdit
    chkdsk /f
    
    ```
    

Checks the disk for errors and fixes them. You may need to restart your computer.

- **Run Windows Assessment Tool:**
    
    ```
    cmd
    CopyEdit
    winsat formal
    
    ```
    

Assesses the performance of various system components and provides a performance report. [Microsoft Learn](https://learn.microsoft.com/en-us/answers/questions/1338760/how-to-clean-up-computer-to-run-faster-windows-10?utm_source=chatgpt.com)

- **Uninstall Unused Programs:**
    
    ```
    cmd
    CopyEdit
    wmic product get name
    wmic product where name="Program Name" call uninstall
    
    ```
    

Lists installed programs and allows you to uninstall them via Command Prompt. Replace "Program Name" with the actual program name. [outbyte.com](https://outbyte.com/blog/how-to-speed-up-windows-10/?utm_source=chatgpt.com)

---

**Note:** Always back up important data before performing system operations. Some commands may require administrative privileges.

For a visual guide on cleaning your computer using Command Prompt, you might find this video helpful:

Feel free to ask if you need further assistance or explanations on any of these steps!