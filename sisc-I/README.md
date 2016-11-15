# Simplistic Instruction Set Computing

Simplistic Instruction Set Computing (SISC) is a subset of CISC. Complex instruction set computing (CISC) is a processor design where single instructions can execute several low-level operations (such as a load from memory, an arithmetic operation, and a memory store).

The Simplistic Instruction Set Computing pairs features of CISC (load, arithmetic and store in one instruction) with features of RISC (fewer instructions). A remarkable feature that distiguishes SISC is, for example, that it completely lacks any **JUMP**- or **CALL**-instruction. Instead, a branch is archieved by simply overwriting the *instruction pointer*.

### The Jumps and function calls.

The only way to perform a jump in the SISC architecture is to simply assign a new value to the instruction pointer.

```
loop:
# loop body
mov &loop, %ip
```

The next example is how to call a function:

```
function:
	# The function body:
    ...
    # returning to the caller:
    pop %ip

# calling the Function
push %ip:1
mov &function, %ip
```

### Operands

Operand | Meaning | Writable
--- | --- | ---
`%ip` | Instruction Pointer  | Yes
`%ip:2` | Instruction Pointer + 2 x size-of-an-instruction | No
`%Xs` | Segment register | Yes
`%Xs:0` | Value in memory at `%Xs` | Yes
`%Xs:4` | Value in memory at `(%Xs)+4` | Yes
`%sp` | Stack pointer | Yes
`%sp:4` | Value in memory at `(%sp)+4` | Yes
`%sr` | Status register | Yes
`%sr:0` | The bit 0 in the status register | No
`%cr` | Condition register | Yes
`%cr:7` | Condition register's value is `7` | No

`%Xs` stands for four Segments:
* `%as` - The first "Array Segment"
* `%bs` - The second "Array Segment"
* `%cs` - The Thread Local Segment
* `%ds` - The Stack frame local Segment

Registers:

Reg | Meaning
--- | ---
`%as` | 1st Array Segment
`%bs` | 2nd Array Segment
`%cs` | Thread Local Segment
`%ds` | Stack frame Local Segment
`%ip` | Instruction Pointer
`%sp` | Stack Pointer
`%sr` | Status register
`%cr` | condition register

### Conditional Jumps

As SISC has no **JUMP** instructions, it can't have conditional jump instructions. Instead it implements *conditional assignments*: When a condition is not satisfied, the result will not be assigned to it's destination.

```
gte %ds:0,%ds:44, %cr
mov &branchtarget, %ip  IF()
```

### Load and Store

As a CISC-like architecture, the only way to load and store from and to memory is `mov`.

For example if say we want to load a word from the pointer at `%ds:36` to `%ds:16`:

```
# Assign the pointer to 1st array segment
mov %ds:36, %as
# Assign the value, the pointer points to, to %ds:16
mov %as:0, %ds:16
```

