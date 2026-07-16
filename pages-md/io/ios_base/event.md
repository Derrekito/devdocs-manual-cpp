# std::ios_base::event

```cpp
enum event { erase_event, imbue_event, copyfmt_event };
```

Specifies the event type which is passed to functions registered by
`register_callback()` on specific events. The following constants are defined:

- **`erase_event`** — issued on `~ios_base()` or `basic_ios::copyfmt()` (before
  the copy of members takes place)
- **`imbue_event`** — issued on `imbue()`
- **`copyfmt_event`** — issued on `basic_ios::copyfmt()` (after the copy of
  members takes place, but before the exception settings are copied)

### Example

---
*Source: https://en.cppreference.com/w/cpp/io/ios_base/event*
