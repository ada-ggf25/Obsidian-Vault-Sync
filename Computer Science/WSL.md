#computer_science #os 

​The Windows Subsystem for Linux (WSL) enables you to run a full Linux environment directly on your Windows PC, allowing you to develop and run Linux-based applications without the need for a separate virtual machine or dual-boot setup. Here's how you can set it up:​[Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/install?utm_source=chatgpt.com)

---

### 🛠️ Step-by-Step: Setting Up WSL on Windows

#### 1. **Install WSL**

On Windows 10 (version 2004 and higher) or Windows 11, you can install WSL by following these steps:​[Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/install?utm_source=chatgpt.com)

1. **Open PowerShell as Administrator:**
    
    - Click on the Start menu, type "PowerShell", right-click on "Windows PowerShell", and select "Run as administrator".​
        
2. **Install WSL:**
    
    - In the PowerShell window, enter the following command:​
        
        powershell
        
        CopyEdit
        
        `wsl --install`
        
    - This command will enable the necessary features, install the latest Linux kernel, set WSL 2 as the default version, and install the Ubuntu distribution by default.​[Microsoft Learn+1Microsoft Learn+1](https://learn.microsoft.com/en-us/windows/wsl/setup/environment?utm_source=chatgpt.com)
        
3. **Restart Your Computer:**
    
    - After the installation completes, you'll be prompted to restart your machine.​[Microsoft Learn+1Visual Studio Code+1](https://learn.microsoft.com/en-us/windows/wsl/setup/environment?utm_source=chatgpt.com)
        

For more detailed instructions, you can refer to the official Microsoft documentation: [Install WSL](https://learn.microsoft.com/en-us/windows/wsl/install).​[Microsoft Learn+1Microsoft Learn+1](https://learn.microsoft.com/en-us/windows/wsl/install?utm_source=chatgpt.com)

#### 2. **Set Up Your Linux User Account**

After restarting, launch the installed Linux distribution (e.g., Ubuntu) from the Start menu.​

- You'll be prompted to create a new UNIX username and password. This account is separate from your Windows user account and will be your default user for the Linux environment.​[Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/setup/environment?utm_source=chatgpt.com)
    

#### 3. **Update and Upgrade Packages**

It's a good practice to update your package lists and upgrade installed packages:​

bash

CopyEdit

`sudo apt update && sudo apt upgrade`

#### 4. **Install Development Tools**

Depending on your development needs, you can install various tools. For example, to install Python and pip:​

bash

CopyEdit

`sudo apt install python3 python3-pip`

For Node.js development, you can use Node Version Manager (nvm):​

bash

CopyEdit

`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh | bash`

After installing nvm, restart your terminal and install Node.js:​

bash

CopyEdit

`nvm install node`

#### 5. **Integrate with Visual Studio Code**

To enhance your development experience, you can use Visual Studio Code with the WSL extension:​

1. **Install Visual Studio Code:**
    
    - Download and install VS Code from the [official website](https://code.visualstudio.com/).​
        
2. **Install the WSL Extension:**
    
    - In VS Code, go to the Extensions view by clicking on the Extensions icon in the Activity Bar on the side of the window.​
        
    - Search for "WSL" and install the "Remote - WSL" extension.​
        
3. **Open a WSL Project in VS Code:**
    
    - In your WSL terminal, navigate to your project directory and type:​[Reddit+3Visual Studio Code+3Microsoft Learn+3](https://code.visualstudio.com/docs/remote/wsl-tutorial?utm_source=chatgpt.com)
        
        bash
        
        CopyEdit
        
        `code .`
        
    - This will open the current directory in VS Code, running in the context of your WSL environment.​
        

For a comprehensive guide, refer to the [Remote development in WSL tutorial](https://code.visualstudio.com/docs/remote/wsl-tutorial).​[Visual Studio Code](https://code.visualstudio.com/docs/remote/wsl-tutorial?utm_source=chatgpt.com)

---

### 📺 Helpful Video Tutorials

For visual guidance, you might find these tutorials useful:

- [Running Linux on Windows with WSL 2](https://www.youtube.com/watch?v=qPMsV1DSGJY)
    
- [How to use Linux on Windows! - WSL Installation steps](https://www.youtube.com/watch?v=Tlx7qZCPIsA)
    

---

If you need assistance with setting up specific development environments or tools within WSL, feel free to ask!


Customizing your Windows Subsystem for Linux (WSL) terminal can significantly enhance your development experience. Here's a comprehensive guide to help you transform your WSL terminal into a more efficient and visually appealing environment:[Medium](https://medium.com/%40junseopark/a-detailed-wsl-customization-guide-2ab19e5ca4db?utm_source=chatgpt.com)

---

## 1. Install and Configure Zsh with Oh My Zsh

Zsh is an advanced shell that offers improved features over the default Bash shell. To set it up:

1. **Install Zsh**:
    
    ```bash
    bash
    CopyEdit
    sudo apt install zsh
    
    ```
    
2. **Set Zsh as the Default Shell**:
    
    ```bash
    bash
    CopyEdit
    chsh -s $(which zsh)
    
    ```
    
3. **Install Oh My Zsh**, a framework for managing Zsh configuration:
    
    ```bash
    bash
    CopyEdit
    sh -c "$(curl -fsSL <https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh>)"
    
    ```
    

This setup provides a robust foundation for further customization. [DEV Community](https://dev.to/shettykaran21/customize-windows-terminal-with-wsl2-od9?utm_source=chatgpt.com)

---

## 2. Enhance Visuals with Powerline or Nerd Fonts

To ensure that your terminal displays icons and glyphs correctly:

1. **Install Powerline Fonts**:
    
    ```bash
    bash
    CopyEdit
    sudo apt install fonts-powerline
    
    ```
    
2. **Alternatively, Install Nerd Fonts**:
    
    - Download a font like [FiraCode Nerd Font](https://www.nerdfonts.com/font-downloads).
    - Install the font on Windows by opening the `.ttf` file and clicking "Install".
3. **Configure Windows Terminal to Use the Installed Font**:
    
    - Open Windows Terminal.
    - Navigate to Settings (`Ctrl + ,`).
    - Under your WSL profile, set `"fontFace"` to the name of the installed font, e.g., `"FiraCode Nerd Font"`.[DEV Community](https://dev.to/shettykaran21/customize-windows-terminal-with-wsl2-od9?utm_source=chatgpt.com)[DEV Community](https://dev.to/rishabk7/beautify-your-terminal-wsl2-5fe2?utm_source=chatgpt.com)

This ensures that your terminal displays all symbols and icons correctly.

---

## 3. Customize the Prompt with Powerlevel10k

Powerlevel10k is a fast and highly customizable Zsh theme:

1. **Clone the Powerlevel10k Repository**:
    
    ```bash
    bash
    CopyEdit
    git clone --depth=1 <https://github.com/romkatv/powerlevel10k.git> \\
    ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
    
    ```
    
2. **Set the Theme in `.zshrc`**:
    
    - Open the `.zshrc` file:
        
        ```bash
        bash
        CopyEdit
        nano ~/.zshrc
        
        ```
        
    - Set the theme by modifying the line:
        
        ```bash
        bash
        CopyEdit
        ZSH_THEME="powerlevel10k/powerlevel10k"
        
        ```
        
    - Save and exit the editor.[Medium+2DEV Community+2DEV Community+2](https://dev.to/shettykaran21/customize-windows-terminal-with-wsl2-od9?utm_source=chatgpt.com)
        
3. **Apply the Changes**:
    
    ```bash
    bash
    CopyEdit
    source ~/.zshrc
    
    ```
    

Upon restarting the terminal, you'll be guided through the Powerlevel10k configuration wizard to customize your prompt.

---

## 4. Configure Windows Terminal Settings

To further personalize your terminal:

1. **Set WSL as the Default Profile**:
    
    - Open Windows Terminal Settings (`Ctrl + ,`).
    - Locate the GUID of your WSL profile and set it as the `"defaultProfile"`.
2. **Enable Acrylic Background**:
    
    - Within your WSL profile settings, add:
        
        ```json
        json
        CopyEdit
        "useAcrylic": true,
        "acrylicOpacity": 0.85
        
        ```
        
3. **Customize Color Scheme and Appearance**:
    
    - Adjust the `"colorScheme"` to your preference.
    - Modify `"cursorShape"`, `"backgroundImage"`, and other settings as desired.

These adjustments enhance the visual appeal and usability of your terminal. [DEV Community](https://dev.to/shettykaran21/customize-windows-terminal-with-wsl2-od9?utm_source=chatgpt.com)

---

## 5. Add Useful Zsh Plugins

Enhance functionality with plugins:[DEV Community+1DEV Community+1](https://dev.to/shettykaran21/customize-windows-terminal-with-wsl2-od9?utm_source=chatgpt.com)

1. **Install `zsh-syntax-highlighting`**:
    
    ```bash
    bash
    CopyEdit
    git clone <https://github.com/zsh-users/zsh-syntax-highlighting.git> \\
    ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
    
    ```
    
2. **Install `zsh-autosuggestions`**:
    
    ```bash
    bash
    CopyEdit
    git clone <https://github.com/zsh-users/zsh-autosuggestions> \\
    ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
    
    ```
    
3. **Enable Plugins in `.zshrc`**:
    
    - Open `.zshrc`:
        
        ```bash
        bash
        CopyEdit
        nano ~/.zshrc
        
        ```
        
    - Modify the `plugins` array:
        
        ```bash
        bash
        CopyEdit
        plugins=(git zsh-syntax-highlighting zsh-autosuggestions)
        
        ```
        
    - Save and exit.[DEV Community+1DEV Community+1](https://dev.to/rishabk7/beautify-your-terminal-wsl2-5fe2?utm_source=chatgpt.com)[DEV Community](https://dev.to/shettykaran21/customize-windows-terminal-with-wsl2-od9?utm_source=chatgpt.com)
        
4. **Apply the Changes**:
    
    ```bash
    bash
    CopyEdit
    source ~/.zshrc
    
    ```
    

These plugins provide real-time syntax highlighting and command suggestions, improving your command-line efficiency.

---

## 6. Customize the Prompt Manually

If you prefer to tailor your prompt without using themes:

1. **Edit `.bashrc` or `.zshrc`**:
    
    - Open the file:
        
        ```bash
        bash
        CopyEdit
        nano ~/.bashrc  # or ~/.zshrc for Zsh
        
        ```
        
    - Modify the `PS1` variable to customize the prompt. For example:
        
        ```bash
        bash
        CopyEdit
        PS1='\\[\\e[32m\\]\\u@\\h:\\[\\e[34m\\]\\w\\[\\e[0m\\]\\$ '
        
        ```
        
    - Save and exit.
        
2. **Apply the Changes**:
    
    ```bash
    bash
    CopyEdit
    source ~/.bashrc  # or source ~/.zshrc
    
    ```
    

This method allows for granular control over your prompt's appearance.

---

By following these steps, you can transform your WSL terminal into a powerful and aesthetically pleasing development environment. If you need assistance with specific configurations or encounter any issues, feel free to ask!