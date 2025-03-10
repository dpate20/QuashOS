# Quash (Quite A Shell)

Quash is a lightweight custom shell written in C for EECS 678 (Operating Systems). It replicates many of the features you'd expect from a standard shell like `bash` or `csh`, including support for command execution, pipes, I/O redirection, environment variables, background jobs, and built-in commands.

This project is meant to build familiarity with UNIX system calls, the process model, and command-line interfaces.

---

## 💾 Installation

To build Quash:
```bash
make
```

To generate documentation in HTML/LaTeX:
```bash
make doc
```

To clean up build files:
```bash
make clean
```

---

## 🚀 Usage

To launch the shell:
```bash
./quash
```

Or to run tests:
```bash
make test
```

---

## ✅ Features

Quash implements the following:

- 🧠 Execution of binaries with arguments
- 🔎 PATH searching if no path prefix (`/`, `./`, `../`) is provided
- ⏱️ Foreground & background job support (using `&`)
- 🔃 Input & output redirection:
  - `<` for input
  - `>` for output (truncate)
  - `>>` for output (append)
- 🔗 Piping with `|`
- 💬 Environment variable expansion (e.g. `$HOME`, `$PWD`)
- 🧩 Built-in commands:
  - `echo`
  - `cd`
  - `export`
  - `pwd`
  - `jobs`
  - `quit` / `exit`

---

## 📚 Examples

### Run a background job:
```bash
[QUASH]$ sleep 5 &
Background job started: [1]    1234    sleep 5 &
```

### Redirection:
```bash
[QUASH]$ echo Hello > hello.txt
[QUASH]$ cat < hello.txt >> output.txt
```

### Pipe commands:
```bash
[QUASH]$ cat file.txt | grep "keyword" | sort
```

### Built-ins:
```bash
[QUASH]$ echo $HOME
/home/ellia
[QUASH]$ cd ..
[QUASH]$ export PATH=/usr/bin:/bin
[QUASH]$ pwd
/home
[QUASH]$ jobs
[1]  1234  sleep 5 &
```

---

## 🛠️ Dev Notes

### Primary File
- Modify: `src/execute.c`
- Avoid changing `src/parsing/` except for `destroy_parser()` if needed

### Important Functions
- `run_script()` - Entry point for executing commands
- `create_process()` - Handles forking, pipes, and redirection
- `run_<type>()` functions - For each command type

### Deque Utilities (`src/deque.h`)
Helpful for managing job/process lists.

- Use `IMPLEMENT_DEQUE_STRUCT()`, `IMPLEMENT_DEQUE()` to make custom lists
- Example: `new_Example()`, `push_back_Example()`, `destroy_Example()` etc.

### Helpful Shell Functions
- `get_command_string()`
- `lookup_env()`
- `realpath()`

---

## 📖 Useful System Calls

- `fork()`, `execvp()`, `waitpid()`, `kill()`
- `open()`, `close()`, `dup2()`, `pipe()`
- `getenv()`, `setenv()`, `chdir()`, `get_current_dir_name()`
- `atexit()` for cleanup

⚠️ **DO NOT** use `system()` — it’s banned.

---

## 🧪 Testing

Run all tests with:
```bash
make test
```

Or manually:
```bash
./run_tests.bash [-cdptsv]
```

### Test Flags
- `-c` → clean test files  
- `-d` → print diffs on fail  
- `-p` → run tests in parallel  
- `-s`, `-t` → debug: keep test dirs  
- `-v` → verbose test output  

---

## 🎯 Grading Breakdown

| Tier | Features | Points |
|------|----------|--------|
| 0    | Compiles                    | 10%   |
| 1    | Simple commands, `pwd`, `echo` | 30% |
| 2    | Args, env vars, `cd`, `export` | 30% |
| 3    | `jobs`, background procs, I/O redirect, pipes | 30% |
| 4    | (Extra Credit) Mixed pipes + redirect | 10% |

### Memory Violations (-5% per tier):
- Definitely/indirectly lost memory
- Invalid read/write
- Uninitialized values

Use **Valgrind** to avoid these.

---

## 📦 Submission

Submit using:
```bash
make submit
```

Double check before turning it in:
```bash
make unsubmit
```

🔁 That will repackage your code to simulate TA extraction. Everything should build + run post-unzip.

⚠️ If you change the `submit/unsubmit` targets, make sure file extensions are converted to `.txt` and unpack correctly.

---

## 🔥 Final Tips
- Stick to C — no shortcuts like `system()`
- Keep your logic clean and functions modular
- Use the automated tests to your advantage
- Print debug info smartly if you need it
