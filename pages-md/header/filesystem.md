# Standard library header <filesystem> (C++17)

`<filesystem>` is the filesystem support library — portable path
manipulation, directory traversal, and file operations (copy, rename,
remove, permissions, symlinks, free space) without shelling out or
hand-rolling POSIX/Win32 calls. The whole facility requires C++17;
everything lives in `namespace std::filesystem`, and it's conventional
to alias it (`namespace fs = std::filesystem;`) rather than write the
qualified name at every call site.

Almost every operation below comes in two forms: one that throws
`filesystem_error` on failure, and a twin overload taking a trailing
`std::error_code&` that reports failure by setting the code instead of
throwing. Prefer the `error_code&` form on hot paths or when a missing
file isn't exceptional.

### Includes

- **`<compare>`** (C++20) — three-way comparison operator support

### Classes

- **path** (C++17) — represents a filesystem path
- **filesystem_error** (C++17) — exception thrown on file system errors
- **directory_entry** (C++17) — one entry of a directory
- **directory_iterator** (C++17) — iterates the contents of a directory
- **recursive_directory_iterator** (C++17) — iterates a directory and
  its subdirectories
- **file_status** (C++17) — a file's type and permissions
- **space_info** (C++17) — free and available space on a filesystem
- **file_type** (C++17) — enum: the type of a file
- **perms** (C++17) — enum: filesystem permission bits
- **perm_options** (C++17) — enum: semantics of a permissions operation
- **copy_options** (C++17) — enum: semantics of a copy operation
- **directory_options** (C++17) — enum: options for iterating directory
  contents
- **file_time_type** (C++17) — represents a file's time values
- **std::hash<path>** (C++17) — hash support for `path`

### Path composition and queries

- **absolute** (C++17) — composes an absolute path
- **canonical** / **weakly_canonical** (C++17) — composes a canonical
  path (resolving `.`, `..`, and symlinks)
- **relative** / **proximate** (C++17) — composes a path relative to a
  base path
- **current_path** (C++17) — gets or sets the current working directory
- **temp_directory_path** (C++17) — a directory suitable for temporary
  files
- **u8path** (C++17, deprecated C++20) — creates a `path` from UTF-8
  encoded source

### File and directory operations

- **copy** (C++17) — copies files or directories
- **copy_file** (C++17) — copies file contents
- **copy_symlink** (C++17) — copies a symbolic link
- **create_directory** / **create_directories** (C++17) — creates a
  directory, or a whole chain of them
- **create_hard_link** (C++17) — creates a hard link
- **create_symlink** / **create_directory_symlink** (C++17) — creates a
  symbolic link
- **remove** / **remove_all** (C++17) — removes a file or empty
  directory, or a directory and all its contents recursively
- **rename** (C++17) — moves or renames a file or directory
- **resize_file** (C++17) — truncates or zero-fills a regular file to a
  size
- **read_symlink** (C++17) — the target of a symbolic link

### Metadata queries

- **exists** (C++17) — does a path refer to an existing file system
  object?
- **equivalent** (C++17) — do two paths refer to the same object?
- **file_size** (C++17) — a file's size
- **hard_link_count** (C++17) — the number of hard links to a file
- **last_write_time** (C++17) — gets or sets the last modification time
- **permissions** (C++17) — modifies a file's access permissions
- **space** (C++17) — free space available on the filesystem
- **status** / **symlink_status** (C++17) — a file's attributes,
  following or not following a symlink target
- **status_known** (C++17) — is a `file_status` fully known?
- **is_block_file** / **is_character_file** / **is_directory** /
  **is_fifo** / **is_other** / **is_regular_file** / **is_socket** /
  **is_symlink** (C++17) — does a path (or `file_status`) refer to
  that kind of file?
- **is_empty** (C++17) — is a file or directory empty?

### Forward declarations

- **hash** (C++11) — forward-declared here for the `path` specialization

---
*Source: https://en.cppreference.com/w/cpp/header/filesystem*
