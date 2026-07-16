# asm declaration

*asm-declaration* gives the ability to embed assembly language source code
within a C++ program. This declaration is conditionally-supported and
implementation defined, meaning that it may not be present and, even when
provided by the implementation, it does not have a fixed meaning.

### Syntax

```text
attr ﻿(optional) asm ( balanced-token-seq ) ;
```

- **attr** — (since C++11) any number of attributes
- **balanced-token-seq** — a sequence of tokens where parentheses, brackets and
  braces are balanced; any restrictions on the balanced-token-seq and its
  meaning are implementation-defined

### Explanation

The balanced-token-seq is typically a string literal that represents a short
program written in assembly language, which is executed whenever this
declaration is executed. Different C++ compilers have wildly varying rules for
asm-declarations, and different conventions for the interaction with the
surrounding C++ code.

As other block declarations, this declaration can appear inside a block (a
function body or another compound statement), and, as all other declarations,
this declaration can also appear outside a block.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_constexpr` | 201907L | (C++20) | Trivial default initialization and
      asm-declaration in `constexpr` functions

### Example

Demonstrates two kinds of inline assembly syntax offered by the GCC/Clang
compilers. This program works correctly only on the x86_64 platform under Linux.

```cpp
#include <iostream>

extern "C" int func(int x);
// the definition of func is written in assembly language
// raw string literal could be very useful
asm(R"(
.globl func
    .type func, @function
    func:
    .cfi_startproc
    movl %edi, %eax /* x is in RDI, see x86-64 calling convention */
    addl $1, %eax
    ret
    .cfi_endproc
)");

int main()
{
    int n = func(0110);
    // extended inline assembly
    asm ("leal (%0,%0,4),%0"
         : "=r" (n)
         : "0" (n));
    std::cout << "73*5 = " << n << std::endl; // flush is intentional

    // standard inline assembly
    asm ("movq $60, %rax\n" // the exit syscall number on Linux
         "movq $2,  %rdi\n" // this program returns 2
         "syscall");
}
```

Output:

```text
73*5 = 365
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 195 | C++98 | it was required to support all asm declarations | made
      conditionally-supported
  CWG 2262 | C++11 | attributes could not be applied to asm declarations |
      allowed

### See also

- C++ ABIs

**C documentation for Inline assembly**

### External links

  1. | GCC Inline Assembly HOWTO
  2. | GCC Inline ASM — Locklessinc.com
  3. | IBM XL C/C++ Inline Assembly
  4. | Intel C++ Inline Assembly
  5. | Visual Studio Inline Assembler
  6. | Sun Studio 12 Asm Statements
  7. | Inline assembly for Itanium-based HP-UX
  8. | X86 calling conventions — Wikipedia

---
*Source: https://en.cppreference.com/w/cpp/language/asm*
