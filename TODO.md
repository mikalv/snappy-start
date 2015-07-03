
# To-do list

## Completeness/correctness

* Whitelist syscalls
   * Create snapshot on first unhandled syscall (e.g. `getpid()`)
* Handle `MAP_ANON` memory that is never made writable (e.g. `PROT_NONE`)
* Fix the buffer size limit in `restore`
* Use `PTRACE_O_EXITKILL` so the tracee process can't escape
* Save/restore more state:
   * Full set of x86 registers
   * Signal handlers and signal mask
   * Opened FDs
   * Futex robust list (not very important)
   * Multiple threads
* Initial stack:
   * Ensure the stack is properly aligned
   * Make the stack bigger
   * Enable the stack guard page
* Check that various inputs are unchanged at `restore` time:
   * File contents
   * Inode numbers from `stat()` and `fstat()`
   * UID, GID, etc.
   * x86 segment selector registers (to be nit-picky)
* Save and replay output to stdout/stderr


## Usability

* Combine the two dump files into a single file
* Add an argument for the snapshot file; stop hard-coding the filename
* Make `ptracer` run `elf_loader` automatically


## Efficiency

* Use `/proc/$PID/mem` to read memory instead of `ptrace()`
* Don't write zero pages into the snapshot file
* De-duplicate filenames in the snapshot file


## Security

* Don't leave the `restore` program mapped at a fixed address


## Features

* Add a tool to examine a snapshot:
   * Print mappings in the format of `/proc/$PID/maps`
   * Show memory consumption
   * Show what triggered producing the snapshot (e.g. the syscall we didn't handle)