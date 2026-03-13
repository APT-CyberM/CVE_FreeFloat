🧪 Lab Setup for Binary Exploitation Beginners


When you're a beginner it's useful to install and configure every tool by hand to learn how they fit together, but after repeating the same setup many times it's far more efficient to automate it so you don't waste time. I created a set of scripts which you can find in the following repository, they automate many of the configurations I walk through in this documentation.

📑 Table of Contents
Requirements
Download Git
Install Git
Download Windows SDK
Install Windows SDK
Download Java JDK
Install Java JDK
Download Netcat (Nmap)
Install Netcat (Nmap)
Download Python 3 x64
Install Python 3 x64
Create a Python Virtual Environment
Install Python Libraries
Download Python 2 x64
Install Python 2 x64
Add Python 2 x64 to Environment Variables
IDEs
Download Notepad++
Install Notepad++
Download Visual Studio Code
Install Visual Studio Code
Download Pycharm Community
Install Pycharm Community
Dynamic Debuggers
Download Immunity Debugger
Install Immunity Debugger
Add Python 2 x32 to Environment Variables
Download x64 Dbg
Install WinDbg Preview
Disassemblers & Decompilers
Download IDA Free
Install IDA Free
Download Binary Ninja
Install Binary Ninja
Download Ghidra
Download Cutter
Download Radare2
Exploit Development
Download Mona Corelan
Download Mona x64dbg
Configura Mona (Immunity Debugger)
Configura Mona (x64 Dbg)
Configura Mona (WinDbg)
Fuzzing
Download DynamoRIO
Download WinAFL
Build WinAFL
Add WinAFL to System PATH
Binary Diffing
Download Binexport Plugin
Import Binexport Plugin
Download Bindiff
Install Bindiff
Download Diaphora Plugin (IDA Pro)
Add Diaphora Plugin to IDA (IDA Pro)
Vulnerable Applications
Download Vulnserver
Run Vulnserver
📦 Requirements: Download Git
Visit the official Git for Windows download page.
Click on the "Click here to download" link to start downloading the latest version manually.
Save the installer file to your Downloads folder.
📦 Requirements: Requirements: Install Git
Locate the downloaded installer and double-click it.
Follow the setup wizard and use the default options.
Click Install, then check "Launch Git Bash" and click Finish.
📦 Requirements: Download Windows SDK
Visit the official Windows SDK download page.
Click "Download the installer" to get the setup file.
Save it to your Downloads folder.
📦 Requirements: Install Windows SDK
Run the downloaded winsdksetup.exe.
Leave the default installation path and click Install.
When finished, click Close.
📦 Requirements: Download Java JDK
Visit the official Java SE Development Kit (JDK) download page.
Scroll down to the Windows section.
Click "x64 MSI Installer".
Save the installer to your Downloads folder.
📦 Requirements: Install Java JDK
Run the installer.
Leave the default installation path and click Next.
Wait for the setup to complete, then click Close.
📦 Requirements: Download Netcat (Nmap)
Visit the official Nmap download page.
Under "Microsoft Windows binaries", click the link to download the latest stable installer.
Save the installer to your Downloads folder.
📦 Requirements: Install Netcat (Nmap)
Run the Nmap installer.
During setup, leave the default options selected, ensuring that the Ncat component remains checked.
Proceed with the installation and click Next until it finishes.
📦 Requirements: Download Python 3 x64
Go to the official Python downloads page.
Under Latest Python 3 Release, click on "Windows installer (64-bit)".
Save the file to your Downloads folder.
📦 Requirements: Install Python 3 x64
Run the installer.
Check the option "Add Python 3.X to PATH" before continuing.
Click "Install now".
At the end of the setup, click "Disable path length limit".
Once finished, click Close.
📦 Requirements: Create a Python Virtual Environment
Instead of installing Python packages globally (which is now restricted), create a dedicated virtual environment under your user profile.
Open PowerShell or Command Prompt and run the following to create the venv in your user folder:
python -m venv VenvExploiting
📦 Requirements: Install Python Libraries
Activate the environment and install the Python packages you will use frequently:
# Activate the environment
VenvExploiting\Scripts\Activate.ps1

# Install tools
pip install capstone filebytes keystone-engine ropper flawfinder

# Leave environment
deactivate
📦 Requirements: Download Python 2 x64
Go to the Python 2.7.11 release page.
Scroll down to Files, and download "Windows x86-64 MSI installer".
Save it to your Downloads folder.
📦 Requirements: Install Python 2 x64
Run the installer.
When asked for the installation directory, manually set it to "C:\Python27x64"
Leave all other options as default and finish the installation.
📦 Requirements: Add Python 2 x64 to Environment Variables
Press Win + R, type sysdm.cpl, and press Enter.
Go to Advanced -> Environment Variables.
Under User variables, find and select Path, then click New.
Add these two new entries "C:\Python27x64" and "C:\Python27x64\Scripts"
Click OK to save and close all dialogs.
📝 IDEs: Download Notepad++
Visit the official Notepad++ download page.
Click on the latest release.
Under "Installer", click the link for 64-bit version.
Save the installer to your Downloads folder.
📝 IDEs: Install Notepad++
Run the installer.
Select your preferred language and click OK.
Follow the default installation steps.
📝 IDEs: Download Visual Studio Code
Visit the official Visual Studio Code page
Under Windows, click System Installer (64-bit).
Save the installer file to your Downloads folder.
📝 IDEs: Install Visual Studio Code
Run the installer.
Accept the license agreement and click Next.
Keep the default installation path and click Next.
In the Additional Tasks screen, make sure to check "Add Open with Code action to Windows Explorer file context menu", "Add Open with Code action to directory context menu", "Register Code as an editor for supported file types", and "Add to PATH (requires shell restart)"
Click Install, then Finish to launch VS Code.
📝 IDEs: Download PyCharm Community
Visit the official JetBrains PyCharm download page.
Click Download for Windows.
Save the installer file to your Downloads folder.
📝 IDEs: Install PyCharm Community
Run the installer.
Keep the default installation path and click Next.
In the Installation Options screen, check the following options "Add Open Folder as Project", and "Add bin folder to the PATH"
Click Next, then Install, and Finish.
🐛 Dynamic Debuggers: Download Immunity Debugger
Visit the official Immunity Debugger download page.
Click on the download link to get the latest version of Immunity Debugger.
Put any random data and then click download.
🐛 Dynamic Debuggers: Install Immunity Debugger
Locate the downloaded setup file (ImmunityDebugger_VERSION_setup.exe). Double-click the setup file to start the installation process.
Follow the on-screen instructions to complete the installation. Here are the typical steps:
Python: If you don't have Python 2.7.1 it will show you a popup to install it, Just click yes.
License Agreement: Read and accept the license agreement, then click "Next".
Choose Installation Location: Select the directory where you want to install Immunity Debugger or leave it at the default location, then click "Install".
Installation Complete: Click "Finish" to complete the installation.
🐛 Dynamic Debuggers: Add Python 2 x32 to Environment Variables
Open the Start menu, search for "Environment Variables", and select "Edit the system environment variables."
In the System Properties window, click on the "Environment Variables" button.
In the Environment Variables window, find the "Path" variable under "User variables," and click "Edit".
Click "New" and add the path to the directory where you installed Python (C:\Python27).
Click "OK" to close all windows.
🐛 Dynamic Debuggers: Download x64dbg
Visit the official x64dbg download page.
Click on the download button (you're going to be redirected to sourceforge) and download the latest snapshot.
Save the file in a directory, such as Downloads.
Extract the .zip archive to x64dbg.
🐛 Dynamic Debuggers: Install WinDbg Preview
Open the Microsoft Store.
In the search bar, type "WinDbg".
Select WinDbg Preview (Microsoft) from the results and click Install.
🕵️‍♂️ Disassemblers & Decompilers: Download IDA Free
Visit the official IDA Free download page.
Click on the download link to get the latest version of IDA Free.
Select the operating system and then click download.
🕵️‍♂️ Disassemblers & Decompilers: Install IDA Free
Locate the downloaded setup file.
Double-click the setup file to start the installation process.
Follow the on-screen instructions to complete the installation. Here are the typical steps:
Start: Click "Next" to start the installation process.
License Agreement: Read and accept the license agreement, then click "Next".
Choose Installation Location: Select the directory where you want to install IDA Free or leave it at the default location, then click "Next", and "Next" again.
Installation Complete: Click "Finish" to complete the installation.
🕵️‍♂️ Disassemblers & Decompilers: Download Binary Ninja
Visit the official Binary Ninja download page.
Go to the Free Edition section and click Download.
Save the installer file to your Downloads folder.
🕵️‍♂️ Disassemblers & Decompilers: Install Binary Ninja
Locate the downloaded setup file.
Run the installer.
Choose your preferred installation directory and click Next.
Follow the on-screen instructions to complete the setup, then click Finish.
🕵️‍♂️ Disassemblers & Decompilers: Download Ghidra
Visit the official Ghidra download page.
Click on the download link to get the latest multi-platform release.
Extract the .zip archive to Ghidra.
🕵️‍♂️ Disassemblers & Decompilers: Download Cutter
Visit the official Cutter download page.
Click on the "Download" button to automatically start downloading the latest Windows ZIP package.
Save the ZIP file to your Downloads folder.
Extract the .zip archive to a directory of your choice.
🕵️‍♂️ Disassemblers & Decompilers: Download Radare2
Visit the official Radare2 GitHub Releases page.
Under the latest release, scroll down to the Assets section.
Click on the Windows x64 blob to start the manual download.
Save the file to your Downloads folder.
Extract it to a directory of your choice.
Press Win + R, type sysdm.cpl, and press Enter.
Go to Advanced -> Environment Variables.
Under System variables, find and select Path, then click New.
Add the path to the folder.
⚡ Exploit Development: Download Mona Corelan
Visit the official Mona download page.
Clone Corelan repository for mona.py
Open CMD
cd Downloads
git clone https://github.com/corelan/mona
⚡ Exploit Development: Download Mona x64dbg
Visit the official Mona for x64dbg repository.
Clone repository
Open CMD
cd Downloads
https://github.com/x64dbg/mona
⚡ Exploit Development: Configure Mona (Immunity Debugger)
Place the mona.py (Corelan) file at the "PyCommands" location.
Open CMD as Administrator
cd C:\Users\YOURUSER\Downloads\mona\
copy mona.py "C:\Program Files (x86)\Immunity Inc\Immunity Debugger\PyCommands\"
Open Immunity Debugger as Administrator
Configure a working folder running mona commands in Immunity Debugger at the bottom side input box.
!mona help
!mona config -set workingfolder C:\mona\%p
