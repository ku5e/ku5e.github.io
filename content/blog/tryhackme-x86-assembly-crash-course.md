---
title: 'TryHackMe: x86 Assembly Crash Course'
date: 2026-04-13T19:42:00
draft: false
tags:
  - tryhackme
  - walkthrough
  - cybersecurity
  - blog
  - ku5e
description: x86 Assembly, Opcodes, MOV/LEA/NOP, Arithmetic Instructions, Logical Instructions, Flags, Conditionals, Branching, Stack Operations, Function Calls
cover:
  image: /images/assemblycrashcourse.png
  alt: Terminal screen displaying x86 assembly opcodes alongside their mnemonics, with one instruction highlighted as the current execution point.
  caption: ''
  relative: false
---

**Author:** Mario Martinez Jr. (ku5e / Gary7) | TryHackMe USA Rank #76 | Top 1%

**Difficulty:** Easy

**Topics:** x86 Assembly, Opcodes, MOV/LEA/NOP, Arithmetic Instructions, Logical Instructions, Flags, Conditionals, Branching, Stack Operations, Function Calls

**Link:** [x86 Assembly Crash Course on TryHackMe](https://tryhackme.com/room/x86assemblycrashcourse)

---

Answers are redacted within the narrative to allow you to complete the tasks on your own, but a full table of answers is available at the end of this walkthrough.

Assembly is the lowest level of human-readable language and the highest level a compiled binary can be reliably decompiled to. When you open a malware sample in Ghidra or x64dbg, you are reading assembly. There is no layer above it. This room covers the instructions you will see on every analysis: MOV, LEA, NOP, ADD, SUB, XOR, CMP, TEST, JMP, PUSH, POP, and CALL. Complete the x86 Architecture Overview room first if you have not already.

---

## Task 1: Introduction

No questions. The prerequisite is the x86 Architecture Overview room. If you skipped it, go back. This room assumes you already know what EAX, ESP, and EIP do.

---

## Task 2: Opcodes and Operands

Every instruction you write in assembly maps to a sequence of bytes the CPU actually executes. Those bytes are opcodes and operands. The disassembler reads the opcodes and translates them into the human-readable mnemonics you see in a tool like Ghidra.

The room's example is clean:

```
040000:    b8 5f 00 00 00    mov eax, 0x5f
```

The address is `040000`. The opcode is `b8`, which the CPU reads as "move an immediate value into EAX." The bytes `5f 00 00 00` are the value `0x5f` written in little-endian format.

Three operand types matter: immediate operands (fixed constants like `0x5f`), registers (like `eax`), and memory operands (denoted by square brackets, like `[eax]`, where the value in the register is treated as a memory address).

### Technical Deep Dive: Null Bytes and Why XOR Exists in Shellcode

The little-endian representation of `0x5f` in the example above is `5f 00 00 00`. Those three zero bytes are null bytes. In normal programs, null bytes are harmless. In shellcode, they are fatal.

Shellcode is often injected through a string-handling vulnerability. Functions like `strcpy` treat `0x00` as the end of a string. If your shellcode contains a null byte, the copy stops at that byte and your payload is truncated. The rest of your code never makes it into memory.

This is why `MOV EAX, 0` almost never appears in shellcode even though it is the obvious way to zero a register. That instruction encodes as `b8 00 00 00 00`, which is four null bytes. The shellcode dies before it runs.

`XOR EAX, EAX` produces the same result, zero in EAX, and encodes as `31 c0`. No null bytes. This is why you see XOR used to zero registers in almost every piece of shellcode ever written, and why seeing `MOV EAX, 0` in a shellcode context is actually suspicious. Someone who knows what they are doing does not write it that way.

**Answers:**

- What are the hex codes that denote assembly operations called? **[REDACTED]**
- Which type of operand is denoted by square brackets? **[REDACTED]**

---

## Task 3: General Instructions

**MOV** copies a value from source to destination. The source can be an immediate value, a register, or a memory location. The destination receives the copy. The original source is unchanged. Syntax: `mov destination, source`.

Memory addressing with MOV uses square brackets. `mov eax, [0x5fc53e]` reads the value at that memory address and puts it in EAX. `mov eax, [ebx]` reads from the address stored in EBX. `mov eax, [ebp+4]` reads from the address EBP plus 4, which is how function arguments are accessed off the stack.

**LEA** (Load Effective Address) looks like MOV but behaves differently. Where MOV with brackets reads the value at a memory address, LEA loads the address itself. `lea eax, [ebp+4]` puts the value of EBP plus 4 into EAX. It does not read memory. It calculates and stores an address.

**NOP** does nothing. It exchanges EAX with itself. Execution moves to the next instruction unchanged. Opcode `0x90`.

**Shift instructions** (SHR, SHL) move bits left or right. Bits shifted out go into the Carry Flag. Empty positions fill with zero. Shift left by one is equivalent to multiplying by two. Shift right by one is dividing by two. Compilers use shift instructions instead of MUL and DIV when the multiplier is a power of two because it is faster.

**Rotate instructions** (ROR, ROL) move bits around the register in a circle. Bits that fall off one end come back on the other. No bits are lost.

### Technical Deep Dive: The LEA Arithmetic Trap

LEA catches analysts constantly. The instruction is designed to calculate memory addresses, but compilers use it for arithmetic because it can combine a multiply and an add in a single operation, which is faster than doing them separately.

An analyst stepping through code might see:

```
lea eax, [ebx + ecx*4 + 10]
```

The brackets make it look like a memory dereference. The analyst starts hunting for an array or a structure at that address. Twenty minutes later, nothing turns up. That is because there is no memory access happening. The compiler needed to calculate `x = y + (z * 4) + 10` and LEA was the most efficient way to do it. The result lands in EAX and the code moves on.

The rule: when you see LEA, the brackets are not a promise that memory is being read. They are notation for an address calculation. Check what happens to the destination register next. If the code immediately uses EAX as a pointer, it is a real address. If it treats EAX as a value, the compiler was doing math.

### Technical Deep Dive: The NOP Sled in Shellcode

The NOP sled is not a textbook curiosity. It is a practical solution to a real problem in exploitation.

When you overflow a buffer and overwrite the return address, you need to know exactly where your shellcode lands in memory. In practice, addresses shift. Stack alignment changes between runs. ASLR randomizes bases. If you aim directly at the first byte of your shellcode and miss by four bytes, your payload does not execute.

The NOP sled solves this by padding the shellcode with a large block of NOP instructions before the actual payload. In a hex dump, it is unmistakable: a wall of `90 90 90 90 90 90` stretching across dozens or hundreds of bytes, then the real code begins. The attacker points the return address anywhere inside that sled. The CPU executes NOPs one by one, does nothing, moves to the next, and eventually slides down into the shellcode. As long as you land somewhere on the sled, the payload runs.

You see this most in older exploits and brute-force shellcode. Modern mitigations like ASLR make the sled harder to aim at in the first place, which is why current exploitation techniques rely more on information leaks to find exact addresses. The sled is still worth knowing because you will see it in legacy samples and in CTF shellcode.

**Answers:**

- In `mov eax, ebx`, which register is the destination operand? **[REDACTED]**
- What instruction performs no action? **[REDACTED]**

---

## Task 4: Flags

The EFLAGS register is a collection of single-bit indicators that reflect the result of the last operation. Conditional jumps read these flags to decide whether to branch.

The flags covered in this task:

| Flag | Abbreviation | Triggers When |
|---|---|---|
| Carry | CF | Result overflows the destination, or a borrow occurs |
| Parity | PF | Least significant byte has an even number of 1 bits |
| Auxiliary | AF | Carry from bit 3 to bit 4 (BCD arithmetic) |
| Zero | ZF | Result is zero |
| Sign | SF | Result is negative (most significant bit is 1) |
| Overflow | OF | Signed arithmetic overflow |
| Direction | DF | Controls string instruction direction (0 = forward) |
| Interrupt Enable | IF | Enables or disables hardware interrupts |

ZF and CF do most of the work in malware analysis. ZF fires on equality checks and null tests. CF fires on unsigned comparisons and overflow.

**Answers:**

- Which flag sets when the result is zero? **[REDACTED]**
- Which flag sets when the result is negative? **[REDACTED]**

---

## Task 5: Arithmetic and Logical Instructions

**ADD** and **SUB** add or subtract a value from the destination and store the result in the destination. SUB sets ZF when the result is zero (both operands were equal) and CF when the destination was smaller than the subtracted value (borrow occurred).

**MUL** multiplies the value operand by EAX and stores the 64-bit result across EDX:EAX. The lower 32 bits land in EAX, the upper 32 bits in EDX. This is why EDX needs to be zeroed before a multiplication if you only care about a 32-bit result. **DIV** reverses this: it divides the 64-bit value in EDX:EAX by the operand, stores the quotient in EAX and the remainder in EDX.

**INC** and **DEC** add or subtract 1 from the operand. You see these in loop counters.

**AND**, **OR**, **NOT**, and **XOR** operate bitwise.

### Technical Deep Dive: XOR eax, eax

`XOR EAX, EAX` appears in nearly every binary, malware or otherwise. It is so standard that `MOV EAX, 0` reads as unusual. Compilers prefer XOR to zero a register because it is a shorter instruction and does not require encoding a 32-bit immediate value.

In shellcode, the preference is not just about size. As covered in Task 2, `MOV EAX, 0` encodes as `b8 00 00 00 00`, four null bytes that kill a string-based exploit. `XOR EAX, EAX` encodes as `31 c0`. Two bytes, no nulls.

The room asks whether `xor eax, eax` and `mov eax, 0` produce the same result. They do. The difference is entirely in the encoding and in what that encoding means when the bytes land in a string buffer.

**Answers:**

- In a subtraction, which flag sets when the destination is smaller than the subtracted value? **[REDACTED]**
- Which instruction increases a register's value? **[REDACTED]**
- Do `xor eax, eax` and `mov eax, 0` have the same result? yea/nay **[REDACTED]**

---

## Task 6: Conditionals and Branching

**TEST** performs a bitwise AND and discards the result, setting only the flags. ZF is set if the result is zero. The most common use: `TEST EAX, EAX`. If EAX is zero, ZF sets. If EAX is non-zero, ZF clears. No other operation needed.

**CMP** subtracts the source from the destination and discards the result, setting flags based on the difference. ZF sets if they are equal. CF sets if the destination is less than the source. Neither flag sets if the destination is greater.

**JMP** moves the Instruction Pointer to a specified address unconditionally.

Conditional jumps check flag state and branch if the condition is met. The common ones:

| Instruction | Condition |
|---|---|
| JZ / JE | Jump if ZF=1 (zero / equal) |
| JNZ / JNE | Jump if ZF=0 (not zero / not equal) |
| JG | Jump if destination greater (signed) |
| JL | Jump if destination less (signed) |
| JA | Jump if above (unsigned) |
| JB | Jump if below (unsigned) |

### Technical Deep Dive: CMP vs TEST and the Mistake Analysts Make

CMP is what you see when code is comparing two values: is this counter greater than this limit, is this index equal to this boundary. TEST is what you see for a specific, narrower question: is this value zero, or did this function return an error.

`TEST EAX, EAX` is the standard way to check if a function returned NULL or zero after a call. In C, the pattern is: call a function, check the return value, branch on failure. In assembly, that check is almost always `TEST EAX, EAX` followed by `JZ` or `JNZ`.

The mistake beginners make is treating TEST as a complex bitwise operation and hunting for what two different values are being ANDed together. When both operands are the same register, the AND with itself returns the value unchanged, and the only thing that matters is whether ZF fired. The programmer was asking one question: is this zero? That is all.

The second common mistake is applying the same logic to `TEST EAX, 1` or similar. That one is checking a specific bit, not just zero. The AND isolates the bit in question and ZF reflects whether it was set. Read what is in the second operand before assuming the operation is just a null check.

**Answers:**

- Which flag sets as a result of TEST being zero? **[REDACTED]**
- Which operation uses subtraction to test two values — 1 (CMP) or 2 (TEST)? **[REDACTED]**
- Which flag decides a JZ or JNZ branch? **[REDACTED]**

---

## Task 7: Stack and Function Calls

**PUSH** puts a value on top of the stack and decrements ESP to point to the new top. **POP** retrieves the top value into the destination register and increments ESP. The stack is LIFO: last pushed, first popped.

**PUSHA** pushes all 16-bit general-purpose registers at once. **PUSHAD** pushes all 32-bit general-purpose registers: EAX, EBX, ECX, EDX, ESI, EDI, ESP, EBP. **POPA** and **POPAD** reverse the operation in a specific order.

**CALL** pushes the return address onto the stack and jumps to the target function. When the function finishes and executes RET, the return address is popped back into EIP and execution resumes where it left off.

### Technical Deep Dive: PUSHAD and the Unpacking Stub

The room notes that PUSHA and PUSHAD are "often a sign of someone manually injecting assembly instructions." That is accurate but understates what it usually means in practice.

When malware is packed or encrypted, the binary on disk is not the real malware. It is a wrapper. The wrapper contains a small piece of code called an unpacking stub whose only job is to decrypt or decompress the actual payload into memory and then hand execution over to it.

The unpacking stub almost always starts with PUSHAD. Saving every register at once, before doing anything, means the stub can trash every register during the unpacking work and then restore them cleanly at the end with POPAD. The payload then starts execution with the registers in the state it expects.

In practice, seeing PUSHAD near the beginning of a binary is one of the clearest indicators that you are looking at packed malware. The real code has not run yet. The stub is about to do its work. Analysts watching for this in a debugger will set a breakpoint on POPAD, let the stub run, and then examine what has been written into memory. That new region is the real payload.

**Answers:**

- Which instruction performs a function call? **[REDACTED]**
- Which instruction pushes all registers to the stack? **[REDACTED]**

---

## Task 8: Practice Time

The emulator runs actual assembly code and shows register values, memory, flags, and stack updating in real time. Four code sections: Arithmetic, MOV Instructions, Stack, and CMP/TEST Instructions, plus a LEA section.

The thing that clicks in an emulator that reading does not deliver is watching the Instruction Pointer move. EIP is not an abstract concept once you see it. It is a finger pointing at the current line. Every instruction it touches executes. If you change where that finger points, the program's reality changes. That is the whole game in exploitation: control EIP and you control what the CPU does next.

Run through each section and watch the registers change color as values move. The MOV section has a trap: the sixth instruction attempts a memory-to-memory move, which x86 does not allow. Either both operands reference registers, or one is a register. Two memory references in a single instruction is not valid.

**Answers:**

- After the 4th MOV instruction, what is the value of `[eax]`? (hex) **[REDACTED]**
- What error appears after the 6th MOV instruction? **[REDACTED]**
- After the 9th stack instruction, what is the value of EAX? (hex) **[REDACTED]**
- After the 12th stack instruction, what is the value of EDX? (hex) **[REDACTED]**
- After POP ECX, what value remains at the top of the stack? (hex) **[REDACTED]**
- After the 3rd CMP/TEST instruction, which flags triggered? **[REDACTED]**
- After the 11th CMP/TEST instruction, which flags triggered? **[REDACTED]**
- After the 9th LEA instruction, what is the value of EAX? (hex) **[REDACTED]**
- What is the final value in ECX? (hex) **[REDACTED]**

---

## Task 9: Conclusion

The room covers opcodes and operands, the MOV/LEA/NOP/shift/rotate instruction set, arithmetic and logical operations, the full EFLAGS register, conditionals with CMP and TEST, branching with JMP and conditional jumps, and stack operations including PUSH, POP, and CALL.

### Who This Room Is For

If malware analysis is the goal, this room is not optional. Assembly is the language malware speaks. If you cannot read it, you cannot read malware. That is the complete argument.

Exploit development requires the same foundation. Writing a buffer overflow means knowing how to set up registers for a system call, how to avoid null bytes in your payload, and how to control EIP. This room is the alphabet for that work. You cannot write a sentence without it.

Cyber policy analysts and SOC managers will not use any of this. Analysts working exclusively in web application security, broken access control, and injection vulnerabilities will not use it either. If the job is finding XSS and SQL injection in web apps, this level of detail is not relevant to the work.

If you want to be the analyst who takes a malware sample apart instead of forwarding it to a vendor, start here.

---

## Answer Table

| Task | Question | Answer |
|---|---|---|
| Task 2 | What are hex codes denoting assembly operations called? | Opcodes |
| Task 2 | Which operand type is denoted by square brackets? | Memory operand |
| Task 3 | In `mov eax, ebx`, which is the destination? | eax |
| Task 3 | Which instruction performs no action? | nop |
| Task 4 | Which flag sets when the result is zero? | ZF |
| Task 4 | Which flag sets when the result is negative? | SF |
| Task 5 | In subtraction, which flag sets when destination < subtracted value? | Carry Flag |
| Task 5 | Which instruction increases a register's value? | inc |
| Task 5 | Do `xor eax, eax` and `mov eax, 0` produce the same result? | yea |
| Task 6 | Which flag sets when TEST result is zero? | Zero Flag |
| Task 6 | Which uses subtraction to test two values — 1 (CMP) or 2 (TEST)? | 1 |
| Task 6 | Which flag decides JZ/JNZ? | Zero Flag |
| Task 7 | Which instruction performs a function call? | call |
| Task 7 | Which instruction pushes all registers to the stack? | pusha |
| Task 8 | Value of `[eax]` after 4th MOV instruction (hex)? | 0x00000040 |
| Task 8 | Error after 6th MOV instruction? | Memory to memory data movement is not allowed. |
| Task 8 | Value of EAX after 9th stack instruction (hex)? | 0x00000025 |
| Task 8 | Value of EDX after 12th stack instruction (hex)? | 0x00000010 |
| Task 8 | Value at top of stack after POP ECX (hex)? | 0x00000010 |
| Task 8 | Flags after 3rd CMP/TEST instruction? | PF,ZF |
| Task 8 | Flags after 11th CMP/TEST instruction? | CF,SF |
| Task 8 | Value of EAX after 9th LEA instruction (hex)? | 0x0000004B |
| Task 8 | Final value in ECX (hex)? | 0x00000045 |

---

*Written by Mario Martinez Jr. (ku5e / Gary7) | [TryHackMe Profile](https://tryhackme.com/p/ku5e) | [ku5e.com/blog](https://ku5e.com/blog)*
