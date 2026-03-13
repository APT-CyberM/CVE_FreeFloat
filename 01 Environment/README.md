# 🔬 Lab Setup for Binary Exploitation Beginners

Setting up a **Binary Exploitation lab** for the first time can be an excellent learning experience. Installing every tool manually helps you understand how they interact and what dependencies they require.

However, after repeating the same setup several times, it becomes far more efficient to automate the process. For that reason, I created a set of scripts that automate many of the configurations described in this documentation.

> These scripts can be found in the repository associated with this guide.

---

# 📚 Table of Contents

* [Requirements](#requirements)
* [Development Environments](#development-environments)
* [Debugging Tools](#debugging-tools)
* [Disassemblers & Decompilers](#disassemblers--decompilers)
* [Exploit Development](#exploit-development)
* [Fuzzing](#fuzzing)
* [Binary Diffing](#binary-diffing)
* [Vulnerable Applications](#vulnerable-applications)

---

# 📥 Requirements

## Download Git

1. Visit the official **Git for Windows** download page.
2. Click **"Click here to download"** to download the latest version.
3. Save the installer to your **Downloads** folder.

---

## Install Git

1. Locate the downloaded installer.
2. Double-click the file.
3. Follow the setup wizard using **default options**.
4. Click **Install**.
5. Check **Launch Git Bash** and click **Finish**.

---

## Download Windows SDK

1. Visit the official **Windows SDK** download page.
2. Click **Download the installer**.
3. Save it to your **Downloads** folder.

---

## Install Windows SDK

1. Run `winsdksetup.exe`.
2. Leave the **default installation path**.
3. Click **Install**.
4. When finished click **Close**.

---

## Download Java JDK

1. Visit the official **Java SE Development Kit (JDK)** page.
2. Scroll to the **Windows section**.
3. Click **x64 MSI Installer**.
4. Save the installer to your **Downloads** folder.

---

## Install Java JDK

1. Run the installer.
2. Leave the **default installation path**.
3. Click **Next**.
4. Wait until the installation completes.
5. Click **Close**.

---

## Download Netcat (Nmap)

1. Visit the official **Nmap download page**.
2. Under **Microsoft Windows binaries**, download the latest installer.
3. Save it to your **Downloads** folder.

---

## Install Netcat (Nmap)

1. Run the **Nmap installer**.
2. Leave the default options selected.
3. Ensure **Ncat** remains checked.
4. Continue clicking **Next** until installation finishes.

---

## Download Python 3 x64

1. Go to the official **Python downloads page**.
2. Under **Latest Python 3 Release**, download **Windows installer (64-bit)**.
3. Save it to your **Downloads** folder.

---

## Install Python 3 x64

1. Run the installer.
2. Check **Add Python to PATH**.
3. Click **Install Now**.
4. At the end select **Disable path length limit**.
5. Click **Close**.

---

## Create a Python Virtual Environment

Instead of installing Python packages globally, create a dedicated virtual environment.

Open **PowerShell or CMD**:

```bash
python -m venv VenvExploiting
```

---

## Install Python Libraries

Activate the environment and install commonly used libraries.

```bash
# Activate environment
VenvExploiting\Scripts\Activate.ps1

# Install tools
pip install capstone filebytes keystone-engine ropper flawfinder

# Leave environment
deactivate
```

---

## Download Python 2 x64

1. Go to the **Python 2.7.11 release page**.
2. Scroll to **Files**.
3. Download **Windows x86-64 MSI installer**.
4. Save to **Downloads**.

---

## Install Python 2 x64

1. Run the installer.
2. Set installation directory to:

```
C:\Python27x64
```

3. Leave other options as default.

---

## Add Python 2 x64 to Environment Variables

1. Press **Win + R**
2. Run:

```
sysdm.cpl
```

3. Navigate to:

```
Advanced → Environment Variables
```

4. Under **User variables → Path → New**

Add:

```
C:\Python27x64
C:\Python27x64\Scripts
```

---

# 💻 Development Environments

## Download Notepad++

1. Visit the official **Notepad++ website**.
2. Download the latest **64-bit installer**.

---

## Install Notepad++

1. Run the installer.
2. Select language.
3. Follow the default installation steps.

---

## Download Visual Studio Code

1. Visit the **Visual Studio Code** page.
2. Download **System Installer (64-bit)**.

---

## Install Visual Studio Code

1. Run the installer.
2. Accept license.
3. Enable these options:

* Add **Open with Code** to context menu
* Register Code as editor
* Add to **PATH**

4. Click **Install → Finish**.

---

## Download PyCharm Community

1. Visit **JetBrains PyCharm** website.
2. Download **PyCharm Community for Windows**.

---

## Install PyCharm Community

1. Run installer.
2. Keep default path.
3. Enable:

* **Add Open Folder as Project**
* **Add bin folder to PATH**

4. Install and finish.

---

# 🐞 Debugging Tools

## Download Immunity Debugger

1. Visit the official **Immunity Debugger** download page.
2. Fill any required fields.
3. Download the installer.

---

## Install Immunity Debugger

1. Run the installer.
2. If prompted, allow installation of **Python 2.7.1**.
3. Accept the license agreement.
4. Complete installation.

---

## Add Python 2 x32 to Environment Variables

1. Search **Environment Variables**.
2. Open **Edit system environment variables**.
3. Under **User variables → Path → Edit → New**

Add:

```
C:\Python27
```

---

## Download x64dbg

1. Visit the **x64dbg download page**.
2. Download the latest snapshot from **SourceForge**.
3. Extract the ZIP.

---

## Install WinDbg Preview

1. Open **Microsoft Store**.
2. Search for **WinDbg Preview**.
3. Click **Install**.

---

# 🔍 Disassemblers & Decompilers

## Download IDA Free

1. Visit the **IDA Free download page**.
2. Select Windows version.
3. Download installer.

---

## Install IDA Free

1. Run installer.
2. Accept license.
3. Keep default installation path.
4. Click **Finish**.

---

## Download Binary Ninja

1. Visit **Binary Ninja website**.
2. Go to **Free Edition**.
3. Download installer.

---

## Install Binary Ninja

1. Run installer.
2. Choose installation directory.
3. Finish installation.

---

## Download Ghidra

1. Visit the official **Ghidra page**.
2. Download the latest release.
3. Extract the archive.

---

## Download Cutter

1. Visit **Cutter official website**.
2. Download the Windows ZIP package.
3. Extract it.

---

## Download Radare2

1. Visit the **Radare2 GitHub Releases** page.
2. Download the **Windows x64 blob**.
3. Extract files.

Add Radare2 to PATH:

```
sysdm.cpl → Environment Variables → Path → New
```

Add the folder path.

---

# ⚡ Exploit Development

## Download Mona Corelan

Clone the Corelan repository:

```bash
cd Downloads
git clone https://github.com/corelan/mona
```

---

## Download Mona x64dbg

Clone the x64dbg mona repository:

```bash
cd Downloads
git clone https://github.com/x64dbg/mona
```

---

## Configure Mona (Immunity Debugger)

Copy `mona.py` to the **PyCommands** directory.

```bash
cd C:\Users\YOURUSER\Downloads\mona\
copy mona.py "C:\Program Files (x86)\Immunity Inc\Immunity Debugger\PyCommands\"
```

Open **Immunity Debugger as Administrator**.

Configure the working directory:

```
!mona help
!mona config -set workingfolder C:\mona\%p
```

---

# 🧪 Vulnerable Applications

## Download Vulnserver

Download **Vulnserver** from its official repository.

---

## Run Vulnserver

Extract and run:

```bash
vulnserver.exe
```

Default port:

```
9999
```

This service is commonly used to practice **buffer overflow exploitation and fuzzing techniques**.

---

# 🎯 Conclusion

This setup provides a **complete Windows environment for binary exploitation**, including:

* Debuggers
* Disassemblers
* Exploit development tools
* Fuzzing frameworks
* Vulnerable targets

Once configured, you will have a powerful lab environment for practicing:

* **Buffer Overflows**
* **Shellcode Development**
* **Reverse Engineering**
* **Exploit Development**
* **Binary Diffing**

---

⭐ If this guide helped you, consider **starring the repository**.
