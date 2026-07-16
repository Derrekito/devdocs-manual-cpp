# Filesystem library (since C++17)

The Filesystem library provides facilities for performing operations on file
systems and their components, such as paths, regular files, and directories.

The filesystem library was originally developed as boost.filesystem, was
published as the technical specification ISO/IEC TS 18822:2015, and finally
merged to ISO C++ as of C++17. The boost implementation is currently available
on more compilers and platforms than the C++17 library.

The filesystem library facilities may be unavailable if a hierarchical file
system is not accessible to the implementation, or if it does not provide the
necessary capabilities. Some features may not be available if they are not
supported by the underlying file system (e.g. the FAT filesystem lacks symbolic
links and forbids multiple hardlinks). In those cases, errors must be reported.

The behavior is undefined if the calls to functions in this library introduce a
*file system race*, that is, when multiple threads, processes, or computers
interleave access and modification to the same object in a file system.

#### Library-wide definitions

- *file*: a file system object that holds data, can be written to, read from, or
  both. Files have names, attributes, one of which is file type:
- *file name*: a string of characters that names a file. Permissible characters,
  case sensitivity, maximum length, and the disallowed names are
  implementation-defined. Names `"."` (dot) and `".."` (dot-dot) have special
  meaning at library level.
- *path*: sequence of elements that identifies a file. It begins with an
  optional root-name (e.g. `"C:"` or `"//server"` on Windows), followed by an
  optional root-directory (e.g. `"/"` on Unix), followed by a sequence of zero
  or more file names (all but last of which have to be directories or links to
  directories). The native format (e.g. which characters are used as separators)
  and character encoding of the string representation of a path (the *pathname*)
  is implementation-defined, this library provides portable representation of
  paths.

**Classes**

- **path (C++17)** — represents a path (class)
- **filesystem_error (C++17)** — an exception thrown on file system errors
  (class)
- **directory_entry (C++17)** — a directory entry (class)
- **directory_iterator (C++17)** — an iterator to the contents of the directory
  (class)
- **recursive_directory_iterator (C++17)** — an iterator to the contents of a
  directory and its subdirectories (class)
- **file_status (C++17)** — represents file type and permissions (class)
- **space_info (C++17)** — information about free and available space on the
  filesystem (class)
- **file_type (C++17)** — the type of a file (enum)
- **perms (C++17)** — identifies file system permissions (enum)
- **perm_options (C++17)** — specifies semantics of permissions operations
  (enum)
- **copy_options (C++17)** — specifies semantics of copy operations (enum)
- **directory_options (C++17)** — options for iterating directory contents
  (enum)
- **file_time_type (C++17)** — represents file time values (typedef)

**Non-member functions**

- **absolute (C++17)** — composes an absolute path (function)
- **canonicalweakly_canonical (C++17)** — composes a canonical path (function)
- **relativeproximate (C++17)** — composes a relative path (function)
- **copy (C++17)** — copies files or directories (function)
- **copy_file (C++17)** — copies file contents (function)
- **copy_symlink (C++17)** — copies a symbolic link (function)
- **create_directorycreate_directories (C++17)(C++17)** — creates new directory
  (function)
- **create_hard_link (C++17)** — creates a hard link (function)
- **create_symlinkcreate_directory_symlink (C++17)(C++17)** — creates a symbolic
  link (function)
- **current_path (C++17)** — returns or sets the current working directory
  (function)
- **exists (C++17)** — checks whether path refers to existing file system object
  (function)
- **equivalent (C++17)** — checks whether two paths refer to the same file
  system object (function)
- **file_size (C++17)** — returns the size of a file (function)
- **hard_link_count (C++17)** — returns the number of hard links referring to
  the specific file (function)
- **last_write_time (C++17)** — gets or sets the time of the last data
  modification (function)
- **permissions (C++17)** — modifies file access permissions (function)
- **read_symlink (C++17)** — obtains the target of a symbolic link (function)
- **removeremove_all (C++17)(C++17)** — removes a file or empty directory
  removes a file or directory and all its contents, recursively (function)
- **rename (C++17)** — moves or renames a file or directory (function)
- **resize_file (C++17)** — changes the size of a regular file by truncation or
  zero-fill (function)
- **space (C++17)** — determines available free space on the file system
  (function)
- **statussymlink_status (C++17)(C++17)** — determines file attributes
  determines file attributes, checking the symlink target (function)
- **temp_directory_path (C++17)** — returns a directory suitable for temporary
  files (function)

**File types**

- **is_block_file (C++17)** — checks whether the given path refers to block
  device (function)
- **is_character_file (C++17)** — checks whether the given path refers to a
  character device (function)
- **is_directory (C++17)** — checks whether the given path refers to a directory
  (function)
- **is_empty (C++17)** — checks whether the given path refers to an empty file
  or directory (function)
- **is_fifo (C++17)** — checks whether the given path refers to a named pipe
  (function)
- **is_other (C++17)** — checks whether the argument refers to an *other* file
  (function)
- **is_regular_file (C++17)** — checks whether the argument refers to a regular
  file (function)
- **is_socket (C++17)** — checks whether the argument refers to a named IPC
  socket (function)
- **is_symlink (C++17)** — checks whether the argument refers to a symbolic link
  (function)
- **status_known (C++17)** — checks whether file status is known (function)

### Notes

Using this library may require additional compiler/linker options. GNU
implementation prior to 9.1 requires linking with `-lstdc++fs` and LLVM
implementation prior to LLVM 9.0 requires linking with `-lc++fs`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_filesystem` | 201703L | (C++17) | Filesystem library

### See also

**C++ documentation for File System TS**

---
*Source: https://en.cppreference.com/w/cpp/filesystem*
