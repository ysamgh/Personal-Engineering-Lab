# Session 003 — Terminal, Shell, Command Execution

**Level:** Beginner → Intermediate  
**Status:** Planned  
**Estimated time:** 60–90 minutes

## Outcomes

Understand the difference between terminal, shell, command, process, argument, option, environment, working directory and exit status. Read commands rather than memorizing them.

## Mental model

```text
Keyboard / terminal emulator
          │ input text
          ▼
        Shell
   (parses command line)
          │
          ├─ built-in command → shell executes
          └─ external command → shell may create/exec process
                                  │
                                  ▼
                             Program
                                  │
                            stdout / stderr
                                  │
                                  ▼
                         terminal / redirection
```

These concepts are related but not identical: a terminal emulator is the user interface, a shell interprets command language, and a program is executable code. Shells can run built-ins, external programs, pipelines, redirections and scripts.

## 1. Safe discovery commands

Run individually, not as one long chain:

```bash
pwd
whoami
id
uname -a
echo "$SHELL"
printf 'hello\n'
```

- `pwd`: current working directory.
- `whoami`: effective username in common environments.
- `id`: user and group identity.
- `uname -a`: kernel/system information (not full distro identity).
- `$SHELL`: variable containing the configured login shell in many environments; current shell may differ.
- `printf`: predictable formatting; often preferred over echo in scripts.

## 2. Command grammar

Example:

`ls -lah /var/log`

- command: `ls`
- options: `-l`, `-a`, `-h` (combined short options)
- argument/path: `/var/log`

Options and arguments vary by command; don't assume a flag means the same thing across tools.

## 3. Help and manuals

```bash
command --help
man ls
man 5 passwd
```

Manual sections distinguish topics, e.g. commands (1), system calls (2), library functions (3), file formats/configuration (5), administration commands (8). On some systems, `man` may not be installed or pages may be missing.

Search inside a manual: `/` then a term; `n` next match; `q` quit.

## 4. Exit status

A process usually returns an exit status to its parent. In shell, `0` conventionally means success and non-zero indicates failure or a specific condition.

```bash
true
echo "$?"

false
echo "$?"
```

This is an intentionally simple lab. `$?` expands to the last command's exit status. It must be read immediately because the next command overwrites the context.

## 5. Output streams

- stdout (file descriptor 1): ordinary output.
- stderr (file descriptor 2): diagnostic/error output.
- stdin (file descriptor 0): input.

```bash
printf 'normal output\n'
printf 'diagnostic output\n' >&2
```

Redirection can overwrite files. Learn the semantics before using `>`, `>>`, `2>`, pipes or `tee` on important paths.

## 6. Challenge

Explain:
1. Terminal vs shell vs command.
2. Why `pwd` is a program/command but `cd` is generally a shell built-in.
3. What an argument is.
4. What exit code 0 commonly means.
5. Why stderr exists separately from stdout.
6. What can happen when you use `>` instead of `>>`.

## 7. Next labs

- Filesystem and path anatomy
- Relative vs absolute paths
- Permissions and ownership
- Process lifecycle
- Pipes and redirection
- Environment variables
- Shell quoting
- Bash scripts and safe scripting
