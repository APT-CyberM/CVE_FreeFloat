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
<img width="1213" height="523" alt="image" src="https://github.com/user-attachments/assets/c2b06252-5f0e-455d-8440-741170a54b4b" />

3. Save the installer to your **Downloads** folder.

---

## Install Git

1. Locate the downloaded installer.
2. Double-click the file.
<img width="598" height="465" alt="image" src="https://github.com/user-attachments/assets/65be3e6c-4a0a-473b-99d5-ac553d72b52f" />
3. Follow the setup wizard using **default options**.
<img width="599" height="466" alt="image" src="https://github.com/user-attachments/assets/7850c597-f699-4ead-9cb3-7cc43815da0b" />
<img width="600" height="465" alt="image" src="https://github.com/user-attachments/assets/aa93eeea-215d-4df6-9b97-f83de6e7cb03" />
<img width="597" height="467" alt="image" src="https://github.com/user-attachments/assets/03329742-2bb7-4a92-a70e-ebf38f68d9d6" />

4. Click **Install**.
<img width="598" height="465" alt="image" src="https://github.com/user-attachments/assets/3102a20e-c434-4969-be4e-fb3ce238de23" />

5. Check **Launch Git Bash** and click **Finish**.
<img width="598" height="465" alt="image" src="https://github.com/user-attachments/assets/70db1d2d-1ec8-464e-9281-8135e8edaa20" />

---

## Download Java JDK

1. Visit the official **Java SE Development Kit (JDK)** page.
2. Scroll to the **Windows section**.
3. <img width="1353" height="509" alt="image" src="https://github.com/user-attachments/assets/f7598847-54ea-484a-99ab-0f6056c77cd7" />
4. Click **x64 MSI Installer**.
5. Save the installer to your **Downloads** folder.

---

## Install Java JDK

1. Run the installer.
2. Leave the **default installation path**.
<img width="504" height="383" alt="image" src="https://github.com/user-attachments/assets/ff40423a-2d60-4f7a-a5fb-9d7d67c4bdaf" />
3. Click **Next**.
<img width="397" height="152" alt="image" src="https://github.com/user-attachments/assets/7f2025ea-eda9-4643-b8bf-b6b0b1bd7f6c" />
<img width="501" height="381" alt="image" src="https://github.com/user-attachments/assets/4c0b49e3-bf01-4518-9386-b61945c48e90" />
4. Wait until the installation completes.
5. Click **Close**.

---

## Download Netcat (Nmap)

1. Visit the official **Nmap download page**.
2. Under **Microsoft Windows binaries**, download the latest installer.
3. Save it to your **Downloads** folder.
<img width="951" height="300" alt="image" src="https://github.com/user-attachments/assets/a2bb8a02-477a-450e-b88c-7072eaf1b70f" />


---

## Install Netcat (Nmap)

1. Run the **Nmap installer**.
2. Leave the default options selected.
<img width="501" height="386" alt="image" src="https://github.com/user-attachments/assets/08947201-6055-42c1-9b03-1643ae8865ee" />

3. Ensure **Ncat** remains checked.
<img width="500" height="392" alt="image" src="https://github.com/user-attachments/assets/fe674328-b954-4df6-982c-1ed25d36b7f5" />

4. Continue clicking **Next** until installation finishes.
<img width="499" height="391" alt="image" src="https://github.com/user-attachments/assets/cfb48a42-458c-4b9c-b299-d1b9b52f5372" />

---

## Download Python 3 x64

1. Go to the official **Python downloads page**.
2. Under **Latest Python 3 Release**, download **Windows installer (64-bit)**.
3. Save it to your **Downloads** folder.
<img width="1491" height="603" alt="image" src="https://github.com/user-attachments/assets/0a523d6f-cb64-40fa-9563-0b9fe2425e32" />
<img width="246" height="29" alt="image" src="https://github.com/user-attachments/assets/4d7c12d0-68da-4a02-89b8-61f5757a4c0e" />



---

## Install Python 3 x64

1. Run the installer.
2. Check **Add Python to PATH**.
<img width="653" height="416" alt="image" src="https://github.com/user-attachments/assets/a5db5aa4-0412-4627-b3c0-391ba388090c" />

3. Click **Install Now**.
<img width="654" height="413" alt="image" src="https://github.com/user-attachments/assets/7c4e4005-59fb-4047-aaaf-3bf1fb6ba494" />

4. Click **Close**.
   
---

## Download Python 2 x64

1. Go to the **Python 2.7.11 release page**.
2. Scroll to **Files**.
3. Download **Windows x86-64 MSI installer**.
<img width="1290" height="831" alt="image" src="https://github.com/user-attachments/assets/6b8d1c57-8c56-461a-9788-6c604d4f2558" />

4. Save to **Downloads**.

---

## Install Python 2 x64

1. Run the installer.
<img width="316" height="272" alt="image" src="https://github.com/user-attachments/assets/dffc00f9-d16a-49e5-9c7a-c8721f34e316" />

2. Set installation directory to:
  
```
C:\Python27x64
```
 
<img width="295" height="251" alt="image" src="https://github.com/user-attachments/assets/969f8018-b90c-4cd2-924c-c3dc3e61a88f" />
3. Leave other options as default.
<img width="225" height="195" alt="image" src="https://github.com/user-attachments/assets/48730866-5484-4d8b-aff1-ca910744cb4c" />
<img width="256" height="220" alt="image" src="https://github.com/user-attachments/assets/4883efe9-6700-4b25-b2b7-2aa6800932d9" />

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
<img width="1488" height="556" alt="image" src="https://github.com/user-attachments/assets/4dc78267-892e-44bc-98d7-cab0eb68601d" />


---

## Install Notepad++

1. Run the installer.
2. Select language.
<img width="501" height="386" alt="image" src="https://github.com/user-attachments/assets/7d92aaad-71e1-4ce6-b4b6-52840a4c2ff5" />
<img width="500" height="391" alt="image" src="https://github.com/user-attachments/assets/8fba3365-2c48-428c-98b1-30829b703fa5" />

3. Follow the default installation steps.
<img width="500" height="389" alt="image" src="https://github.com/user-attachments/assets/c556e955-4291-4f37-9923-7bc2f45bbfa2" />
<img width="499" height="389" alt="image" src="https://github.com/user-attachments/assets/ba454510-8ad3-4f88-a860-e98cbb090865" />

---

## Download Visual Studio Code

1. Visit the **Visual Studio Code** page.
2. Download **System Installer (64-bit)**.
 <img width="1255" height="615" alt="image" src="https://github.com/user-attachments/assets/2b468f5e-f1d0-47b9-9154-d20bede8a266" />


---

## Install Visual Studio Code

1. Run the installer.
2. Accept license.
<img width="597" height="468" alt="image" src="https://github.com/user-attachments/assets/15c32ac2-e5f2-47db-9871-1162a4a74a6a" />

4. Enable these options:

* Add **Open with Code** to context menu
* Register Code as editor
* Add to **PATH**
* <img width="600" height="472" alt="image" src="https://github.com/user-attachments/assets/a91fb306-5efa-4b99-a895-230a7bc3d6a7" />


4. Click **Install → Finish**.
<img width="601" height="466" alt="image" src="https://github.com/user-attachments/assets/fbb5a42f-4cf4-4c93-b6cb-4afe7ad5cd9f" />

---

## Download PyCharm Community

1. Visit **JetBrains PyCharm** website.
2. Download **PyCharm Community for Windows**.

<img width="1370" height="567" alt="image" src="https://github.com/user-attachments/assets/9eb98fbe-6e13-4441-a542-59d7d482827d" />

---

## Install PyCharm Community

1. Run installer.
<img width="503" height="391" alt="image" src="https://github.com/user-attachments/assets/77c28f08-181e-479f-91ff-1a728a78e68c" />

2. Keep default path.
<img width="496" height="388" alt="image" src="https://github.com/user-attachments/assets/e7983eda-819b-4609-90ce-747feb413b14" />
<img width="497" height="388" alt="image" src="https://github.com/user-attachments/assets/200798c3-528f-49c4-884e-54a49e6c0e8e" />


3. Enable:

* **Add Open Folder as Project**
* **Add bin folder to PATH**
<img width="498" height="391" alt="image" src="https://github.com/user-attachments/assets/2d317730-242d-47be-82b1-b59b5ef94307" />

4. Install and finish.
<img width="498" height="390" alt="image" src="https://github.com/user-attachments/assets/538f1820-7101-4bd9-9cb8-40629b073742" />

---

# 🐞 Debugging Tools

## Download Immunity Debugger

1. Visit the official **Immunity Debugger** download page.
2. Fill any required fields.
3. Download the installer.

<img width="569" height="228" alt="image" src="https://github.com/user-attachments/assets/0daffac8-b108-4e73-8737-0bdeb58085fc" />

---

## Install Immunity Debugger

1. Run the installer.
2. If prompted, allow installation of **Python 2.7.1**.
3. Accept the license agreement.
4. Complete installation.

<img width="307" height="213" alt="image" src="https://github.com/user-attachments/assets/3cb362cd-2bd4-4c9d-be6e-23c4de324399" />
<img width="299" height="204" alt="image" src="https://github.com/user-attachments/assets/52cc8dc6-fee1-4712-8a07-569cd53c83e5" />
<img width="299" height="206" alt="image" src="https://github.com/user-attachments/assets/a123b902-bebb-48c7-8376-1941cc6acd5e" />




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

   <img width="505" height="380" alt="image" src="https://github.com/user-attachments/assets/0624dbbb-8d0b-427e-abba-026d7507dc33" />
   <img width="499" height="376" alt="image" src="https://github.com/user-attachments/assets/24225a19-1ef1-4b44-9701-c3cbe54d0cf1" />
   <img width="502" height="378" alt="image" src="https://github.com/user-attachments/assets/974f8bb0-1791-4311-8865-91820eeca12a" />
   <img width="302" height="333" alt="image" src="https://github.com/user-attachments/assets/674dca96-35ff-407b-a499-06b51485fd8f" />
   <img width="315" height="293" alt="image" src="https://github.com/user-attachments/assets/7f11e56e-b7ef-43af-81b0-aa5638866b54" />


---

## Download Ghidra

1. Visit the official **Ghidra page**.
<img width="643" height="379" alt="image" src="https://github.com/user-attachments/assets/40c9cb03-5149-4891-9b11-11e04956a62f" />
2. Download the latest release.

<img width="643" height="346" alt="image" src="https://github.com/user-attachments/assets/d6fa8cea-8af1-449e-85c7-6ac33a34432c" />

3. Extract the archive.
<img width="622" height="78" alt="image" src="https://github.com/user-attachments/assets/1e0429be-8db5-4b0d-a7ab-175cdf8da6fd" />

---

# ⚡ Exploit Development

## Download Mona Corelan

Clone the Corelan repository:

```bash
cd Downloads
git clone https://github.com/corelan/mona
```
<img width="643" height="379" alt="image" src="https://github.com/user-attachments/assets/75ec32ca-32c6-4993-ba2f-ada2301f8cd1" />
<img width="535" height="281" alt="image" src="https://github.com/user-attachments/assets/94f70600-f1ab-4a6d-a5ae-da28f49bbf2f" />

---

---

## Configure Mona (Immunity Debugger)

Copy `mona.py` to the **PyCommands** directory.

```bash
cd C:\Users\YOURUSER\Downloads\mona\
copy mona.py "C:\Program Files (x86)\Immunity Inc\Immunity Debugger\PyCommands\"
```
<img width="527" height="374" alt="image" src="https://github.com/user-attachments/assets/1ce87425-ce0a-460d-bccb-b2982fa567fa" />

```

Open **Immunity Debugger as Administrator**.

Configure the working directory:

```
!mona help
!mona config -set workingfolder C:\mona\%p

<img width="333" height="382" alt="image" src="https://github.com/user-attachments/assets/67828c7f-54f1-4b6e-b041-798b27ed19ca" />

```

---
```

# 🧪 Vulnerable Applications

## Download FreeFloat ftp server

Download **FreeFloat ftp server**
```
<img width="643" height="202" alt="image" src="https://github.com/user-attachments/assets/87984bb9-7611-46d4-88de-56256f0022e2" />

---
```

## Run FreeFloat ftp server

<img width="208" height="113" alt="image" src="https://github.com/user-attachments/assets/79dec3c6-5afe-40c3-a903-cfddaf404e75" />


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
