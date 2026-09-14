# Password Encryption Program

An early Python/Tkinter password manager that saves usernames with passwords encoded by a home-made rotation substitution cipher. This is a learning exercise, not secure password storage.

## What it does

- A login window asks for the program password, which is hardcoded as `12345`. A wrong entry reopens the login window.
- The menu has Add Password, Get Password, See list, Set Key, and Exit.
- **Set Key** takes a rotation number and builds a substitution table. Every letter and digit is shifted along a single cycle of `a-z`, `A-Z`, and `1-9`. The table is written to `key.txt` as a plain-text Python dict.
- **Add Password** takes a username and password (no spaces allowed) and loads the key from `key.txt`. It then appends the username and encoded password to `mypasswords.txt`.
- **Get Password** looks up a username in `mypasswords.txt` and decodes its password by inverting the substitution table.
- **See list** opens a window showing the raw contents of `mypasswords.txt`: usernames and encoded passwords.

## Tech stack

- Python 3
- Tkinter (GUI)
- `functools.reduce`

## Project structure

| File | Purpose |
| --- | --- |
| `main.py` | Tkinter GUI: login window, menu, and the add, get, list, and set-key windows. Also reads and writes the text files. |
| `coding.py` | The cipher. `coding()` returns a closure that responds to the messages `set_key`, `import_key`, `export_key`, `empty_key`, `encoding`, and `decoding`. |

The program creates `key.txt` and `mypasswords.txt` in the current working directory when it runs.

## Running it

The program doesn't start as committed. Details are under Notes.

Once the missing module is resolved, no third-party packages are needed. Everything imported comes from the Python standard library, which needs a Python build with Tk support.

## Notes

- **Missing module:** `main.py` runs `from myClasses import CreateDisplay`, but `myClasses` isn't in the repository, so `python main.py` fails with `ModuleNotFoundError`. `CreateDisplay` is never used anywhere else in `main.py`.
- **Undeclared `sys`:** `main.py` calls `sys.path.append(".")` without importing `sys`. That only works on older Python versions where `from tkinter import *` also exposed `sys`. On newer versions, such as Python 3.11, tkinter's `__all__` excludes modules and the line raises `NameError`.
- **Not secure:**
  - The cipher is a fixed character substitution, so the encoded password keeps the original length and repeated characters.
  - The key table sits in plain text in `key.txt`, next to the password file.
  - The key is loaded back with `eval()`.
  - The login password is hardcoded.
- **Other limitations in the code:**
  - Set Key appends to `key.txt` instead of overwriting it, so after a second key is set the file can no longer be parsed. Delete `key.txt` before setting a new key.
  - Only letters and digits can be encoded. Any other character raises a `KeyError`.
  - The `reverse_word` and `reverse_string` options are stored in the key but do nothing, because their code is commented out.
  - `DeleteList` in `main.py` isn't connected to any button.
- Personal learning project, uploaded to GitHub in September 2021.

Part of my portfolio: https://tk-coding.com/cybersecurity/toolkit/password-encryption
