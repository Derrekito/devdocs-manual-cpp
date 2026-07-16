# std::filesystem::directory_entry

```cpp
class directory_entry;  // (since C++17)
```

Represents a directory entry. The object stores a `path` as a member and may
also store additional file attributes (hard link count, status, symlink status,
file size, and last write time) during directory iteration.

### Member functions

- **(constructor)** — constructs a directory entry (public member function)
- **(destructor)** — default destructor (public member function)

**Modifiers**

- **operator=** — assigns contents (public member function)
- **assign** — assigns contents (public member function)
- **replace_filename** — sets the filename (public member function)
- **refresh** — updates the cached file attributes (public member function)

**Observers**

- **pathoperator const path&** — returns the path the entry refers to (public
  member function)
- **exists** — checks whether directory entry refers to existing file system
  object (public member function)
- **is_block_file** — checks whether the directory entry refers to block device
  (public member function)
- **is_character_file** — checks whether the directory entry refers to a
  character device (public member function)
- **is_directory** — checks whether the directory entry refers to a directory
  (public member function)
- **is_fifo** — checks whether the directory entry refers to a named pipe
  (public member function)
- **is_other** — checks whether the directory entry refers to an *other* file
  (public member function)
- **is_regular_file** — checks whether the directory entry refers to a regular
  file (public member function)
- **is_socket** — checks whether the directory entry refers to a named IPC
  socket (public member function)
- **is_symlink** — checks whether the directory entry refers to a symbolic link
  (public member function)
- **file_size** — returns the size of the file to which the directory entry
  refers (public member function)
- **hard_link_count** — returns the number of hard links referring to the file
  to which the directory entry refers (public member function)
- **last_write_time** — gets the time of the last data modification of the file
  to which the directory entry refers (public member function)
- **statussymlink_status** — status of the file designated by this directory
  entry; status of the file/symlink designated by this directory entry (public
  member function)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — compares two directory entries (public
  member function)

### Non-member functions

- **operator<<** — performs stream output on a directory entry (function)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3171 | C++17 | `directory_entry` couldn't be inserted by `operator<<`
      because of LWG2989 | output enabled again

---
*Source: https://en.cppreference.com/w/cpp/filesystem/directory_entry*
