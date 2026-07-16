# Phases of translation

The C++ source file is processed by the compiler as if the following phases take
place, in this exact order:

### Phase 1

1) The individual bytes of the source code file are mapped (in
implementation-defined manner) to the characters of the basic source character
set. In particular, OS-dependent end-of-line indicators are replaced by newline
characters.
2) The set of source file characters accepted is implementation-defined(since
C++11). Any source file character that cannot be mapped to a character in the
basic source character set is replaced by its universal character name (escaped
with `\u` or `\U`) or by some implementation-defined form that is handled
equivalently.
3) Trigraph sequences are replaced by corresponding single-character
representations.
*(until C++17)*
*(until C++23)*

3) Trigraph sequences are replaced by corresponding single-character
representations.
*(until C++17)*

Input files that are a sequence of UTF-8 code units (UTF-8 files) are guaranteed
to be supported. The set of other supported kinds of input files is
implementation-defined. If the set is non-empty, the kind of an input file is
determined in an implementation-defined manner that includes a means of
designating input files as UTF-8 files, independent of their content
(recognizing the byte order mark is not sufficient).
- If an input file is determined to be a UTF-8 file, then it shall be a
  well-formed UTF-8 code unit sequence and it is decoded to produce a sequence
  of Unicode scalar values. A sequence of translation character set elements is
  then formed by mapping each Unicode scalar value to the corresponding
  translation character set element. In the resulting sequence, each pair of
  characters in the input sequence consisting of carriage return (U+000D)
  followed by line feed (U+000A), as well as each carriage return (U+000D) not
  immediately followed by a line feed (U+000A), is replaced by a single new-line
  character.
- For any other kind of input file supported by the implementation, characters
  are mapped (in implementation-defined manner) to a sequence of translation
  character set elements. In particular, OS-dependent end-of-line indicators are
  replaced by new-line characters.
*(since C++23)*

### Phase 2

1) If the first translation character is byte order mark (U+FEFF), it is
   deleted. (since C++23)Whenever backslash appears at the end of a line
   (immediately followed by zero or more whitespace characters other than
   new-line followed by(since C++23) the newline character), these characters
   are deleted, combining two physical source lines into one logical source
   line. This is a single-pass operation; a line ending in two backslashes
   followed by an empty line does not combine three lines into one.

2) If a non-empty source file does not end with a newline character after this
   step (whether it had no newline originally, or it ended with a newline
   immediately preceded by a backslash), a terminating newline character is
   added.

### Phase 3

1) The source file is decomposed into comments, sequences of whitespace
   characters (space, horizontal tab, new-line, vertical tab, and form-feed),
   and *preprocessing tokens*, which are the following:

   a) header names such as `<iostream>` or `"myfile.h"`

b) placeholder tokens produced by preprocessing import and module directives
(i.e. `import XXX;` and `module XXX;`)
*(since C++20)*

   c) identifiers

   d) preprocessing numbers

   e) character literals, including user-defined character literals(since C++11)

   f) string literals, including user-defined string literals(since C++11)

   g) operators and punctuators (including alternative tokens), such as `+`,
      `<<=`, `<%`, `##`, or `and`

   h) individual non-whitespace characters that do not fit in any other category
The program is ill-formed if the character matching this category is

- apostrophe (`'`, U+0027),
- quotation mark (`"`, U+0022), or
- a character not in the basic character set.

2) Any transformations performed during phase 1 and(until C++23) phase 2 between
the initial and the final double quote of any raw string literal are reverted.
*(since C++11)*

3) Each comment is replaced by one space character.

Newlines are kept, and it is unspecified whether non-newline whitespace
sequences may be collapsed into single space characters.

As characters from the source file are consumed to form the next preprocessing
token (i.e., not being consumed as part of a comment or other forms of
whitespace), universal character names are recognized and replaced by the
designated element of the translation character set, except when matching a
character sequence in:
a) a character literal (c-char-sequence)
b) a string literal (s-char-sequence and r-char-sequence), excluding delimiters
(d-char-sequence)
c) a file name for inclusion (h-char-sequence and q-char-sequence)
*(since C++23)*

If the input has been parsed into preprocessing tokens up to a given character,
the next preprocessing token is generally taken to be the longest sequence of
characters that could constitute a preprocessing token, even if that would cause
subsequent analysis to fail. This is commonly known as *maximal munch*.

```cpp
int foo = 1;
int bar = 0xE+foo;   // error, invalid preprocessing number 0xE+foo
int baz = 0xE + foo; // OK

int quux = bar+++++baz; // error: bar++ ++ +baz, not bar++ + ++baz.
```

The sole exceptions to the maximal munch rule are:

- If the next character begins a sequence of characters that could be the prefix
  and initial double quote of a raw string literal, the next preprocessing token
  shall be a raw string literal. The literal consists of the shortest sequence
  of characters that matches the raw-string pattern.
```cpp
#define R "x"
const char* s = R"y"; // ill-formed raw string literal, not "x" "y"
const char* s2 = R"(a)" "b)"; // a raw string literal followed by a normal string literal
```
- If the next three characters are `<::` and the subsequent character is neither
  `:` nor `>`, the `<` is treated as a preprocessing token by itself (and not as
  the first character of the alternative token `<:`).
```cpp
struct Foo { static const int v = 1; };
std::vector<::Foo> x; // OK, <: not taken as the alternative token for [
extern int y<::>;     // OK, same as extern int y[].
int z<:::Foo::value:>; // OK, int z[::Foo::value];
```
*(since C++11)*

- Header name preprocessing tokens are only formed within a `#include` or
  `import`(since C++20) directive or in a `__has_include` expression(since
  C++17).

```cpp
std::vector<int> x; // OK, <int> not a header-name
```

### Phase 4

1) The preprocessor is executed.

2) Each file introduced with the `#include` directive goes through phases 1
   through 4, recursively.

3) At the end of this phase, all preprocessor directives are removed from the
   source.

### Phase 5

1) All characters in character literals and string literals are converted from
the source character set to the encoding (which may be a multibyte character
encoding such as UTF-8, as long as the 96 characters of the basic character set
have single-byte representations).
2) Escape sequences and universal character names in character literals and
non-raw string literals are expanded and converted to the literal encoding. If
the character specified by a universal character name cannot be encoded as a
single code point in the corresponding literal encoding, the result is
implementation-defined, but is guaranteed not to be a null (wide) character.
Note: the conversion performed at this stage can be controlled by command line
options in some implementations: gcc and clang use `-finput-charset` to specify
the encoding of the source character set, `-fexec-charset` and
`-fwide-exec-charset` to specify the ordinary and wide literal encodings
respectively, while Visual Studio 2015 Update 2 and later uses `/source-charset`
and `/execution-charset` to specify the source character set and literal
encoding respectively.
*(until C++23)*

For a sequence of two or more adjacent string literal tokens, a common encoding
prefix is determined as described here. Each such string literal token is then
considered to have that common encoding prefix. (Character conversion is moved
to phase 3)
*(since C++23)*

### Phase 6

Adjacent string literals are concatenated.

### Phase 7

Compilation takes place: each preprocessing token is converted to a token. The
tokens are syntactically and semantically analyzed and translated as a
translation unit.

### Phase 8

Each translation unit is examined to produce a list of required template
instantiations, including the ones requested by explicit instantiations. The
definitions of the templates are located, and the required instantiations are
performed to produce *instantiation units*.

### Phase 9

Translation units, instantiation units, and library components needed to satisfy
external references are collected into a program image which contains
information needed for execution in its execution environment.

### Notes

Some compilers do not implement instantiation units (also known as template
repositories or template registries) and simply compile each template
instantiation at phase 7, storing the code in the object file where it is
implicitly or explicitly requested, and then the linker collapses these compiled
instantiations into one at phase 9.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 787 | C++98 | the behavior was undefined if a non-empty source file does
      not end with a newline character at the end of phase 2 | add a terminating
      newline character in this case
  CWG 1775 | C++11 | forming a universal character name inside a raw string
      literal in phase 2 resulted in undefined behavior | made well-defined
  P2621R2 | C++98 | universal character names were not allowed to be formed by
      line splicing or token concatenation | allowed

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):
- C++03 standard (ISO/IEC 14882:2003):
- C++98 standard (ISO/IEC 14882:1998):

### See also

**C documentation for Phases of translation**

---
*Source: https://en.cppreference.com/w/cpp/language/translation_phases*
