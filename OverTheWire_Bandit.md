# OverTheWire Bandit — Detailed Walkthrough

A level-by-level walkthrough of the OverTheWire **Bandit** wargame. Each section explains *where* the password is, *why* that location is tricky, and the exact commands I used to retrieve it. This is intentionally verbose — the goal is to show reasoning and command-line problem solving in a way that’s easy for recruiters and engineers to follow.

---

## Level 0 → Level 1

**What to do:** SSH into the Bandit server using the connection string given on the OverTheWire site:

```bash
ssh [username]@[server] -p[portnumber]
```

**Where the password is:** A file named `readme` sits in the home directory.

**How I solved it:** After logging in I listed files and simply read the file with `cat readme`. That printed the password for the next level.

```bash
cat readme
```

---

## Level 1 → Level 2

**What to do:** Read the file named `-`.

**Why it's tricky:** A filename that is just a single dash (`-`) can be interpreted as stdin by many commands, so you must reference it explicitly.

**How I solved it:** Use `./-` to refer to the file in the current directory and `cat` it.

```bash
cat ./-
```

This outputs the password.

---

## Level 2 → Level 3

**What to do:** Read a file whose name contains spaces (`spaces in this filename`).

**Why it's tricky:** Shells split arguments on spaces by default, so you must escape the spaces or quote the filename.

**How I solved it:** I escaped the spaces with backslashes and used `cat`:

```bash
cat spaces\ in\ this\ filename
```

Alternatively, quoting would also work:

```bash
cat "spaces in this filename"
```

---

## Level 3 → Level 4

**What to do:** Find and read a hidden file inside a directory named `inhere`.

**Why it's tricky:** The file is hidden (its name starts with a dot), so normal `ls` won't show it.

**How I solved it:** I changed into the directory and listed all files including hidden ones with `ls -a`. That revealed a hidden filename `...Hiding-From-You`. I then used `cat` to view it.

```bash
cd inhere
ls -a
cat ...Hiding-From-You
```

---

## Level 4 → Level 5

**What to do:** Identify the only human-readable file among several files in `inhere`.

**Why it's tricky:** Many files may contain binary or gibberish; only one has readable text.

**How I solved it:** I changed into `inhere` and concatenated the contents of all files named `-file00` through `-file09` using brace expansion. I inspected the output and found the single human-readable line.

```bash
cd inhere
cat ./-file0{0..9}
```

---

## Level 5 → Level 6

**What to do:** Find a file under `inhere` that meets three conditions: human-readable, 1033 bytes in size, and not executable.

**Why it's tricky:** The file is hidden and nested, so naive listing might miss it. A recursive search that shows hidden files and their sizes is necessary.

**How I solved it:** I ran a recursive `ls` and grepped for the size. First I tried `ls -lR | grep 1033` and got no result, then used `ls -laR | grep 1033` which includes hidden files and revealed a file named `.file2` inside a directory called `maybeinhere07`. I then `cat`ed that file.

```bash
cd inhere
ls -laR | grep 1033
cat maybeinhere07/.file2
```

---

## Level 6 → Level 7

**What to do:** Find a 33-byte file owned by user `bandit7` and group `bandit6`.

**Why it's tricky:** The filesystem is large and permission-denied messages can clutter the output.

**How I solved it:** From the root directory (`cd /`) I used `find` to search for files with the specified owner, group, and size. To avoid `permission denied` errors interfering with results, I redirected stderr to `/dev/null`.

```bash
cd /
find -type f -user bandit7 -group bandit6 -size 33 2>/dev/null
cat [path_returned_by_find]
```

The `find` command returned the path; I used `cat` to read the 33-byte file.

---

## Level 7 → Level 8

**What to do:** Locate the line containing the word "millionth" across several users’ `data.txt` files.

**Why it's tricky:** Multiple `data.txt` files exist in different home directories (bandit7–bandit12), so you need to search across them.

**How I solved it:** From `/` I used `find -name data.txt` to discover all matches (silencing errors to keep the output clean). Then I concatenated the relevant `data.txt` files and piped them to `grep` for the search word.

```bash
find -name data.txt 2>/dev/null
cat /home/bandit{7..12}/data.txt | grep millionth
```

This printed the line containing the required phrase.

---

## Level 8 → Level 9

**What to do:** Extract the only unique (non-duplicated) line from `data.txt`.

**Why it's tricky:** The file contains many repeated lines; the password is the one that appears only once.

**How I solved it:** I sorted the file and used `uniq -u` to print only unique lines.

```bash
cat data.txt | sort | uniq -u
```

---

## Level 9 → Level 10

**What to do:** Get the line that contains an equals sign from a file that includes binary data.

**Why it's tricky:** The file contains binary or non-printable bytes; using `strings` extracts printable sequences that can then be searched.

**How I solved it:** I piped `cat` into `strings` to pull printable text, then used `grep` to find the line with `=`.

```bash
cat data.txt | strings | grep =
```

---

## Level 10 → Level 11

**What to do:** Decode a base64-encoded password.

**Why it's tricky:** The file looks like base64, so you have to decode it to see the password.

**How I solved it:** I used `base64 -d` to decode the contents and printed the result.

```bash
cat data.txt | base64 -d
```

---

## Level 11 → Level 12

**What to do:** Decode a ROT13-encoded string.

**Why it's tricky:** The file is encoded with a letter-substitution cipher (ROT13). `tr` is a small but perfect tool for this.

**How I solved it:** I used `tr` to translate letters and reveal the original text.

```bash
cat data.txt | tr 'a-zA-Z' 'n-za-mN-ZA-M'
```

---

## Level 12 → Level 13

**What to do:** Extract the password from a file that’s wrapped in multiple archive/compression formats and formats.

**Why it's tricky:** The content needs sequential unpacking and possibly binary-to-text conversion. You may need to identify the format at each step and apply the correct tool (gzip, bzip2, tar, xxd, etc.).

**How I solved it:** I iteratively inspected the file (using `file`, `hexdump`/`xxd` when necessary), then applied the appropriate decompress/untar commands (`gzip -d`, `bunzip2`, `tar -xvf`, `xxd -r`, etc.), repeating until the password was recovered.

---

## Level 13 → Level 14

**What to do:** Use an SSH private key found in the home directory to log in as `bandit14`.

**Why it's tricky:** Instead of a password, the level provides a private key file which must be used with SSH’s `-i` option.

**How I solved it:** I saved the private key (ensure it has correct permissions) and used it to SSH into the target account.

```bash
chmod 600 privatekey
ssh -i privatekey bandit14@[host]
```

Once logged in, I opened the password file for the next level.

---

## Level 14 → Level 15

**What to do:** Connect to a service on port 30000 using `nc` (netcat) and send the password found in the provided file.

**Why it's tricky:** This level uses a network service that expects a password via a plain TCP connection, not an SSH session.

**How I solved it:** I launched a netcat connection to the specified host and port, then pasted the password when prompted.

```bash
nc [host] 30000
# paste the password when prompted
```

The service returned the next password.

---

## Level 15 → Level 16

**What to do:** Use OpenSSL’s `s_client` to connect to an SSL/TLS service and interact with it.

**Why it's tricky:** The service requires an SSL/TLS connection rather than plain TCP. `openssl s_client` is a handy tool for this.

**How I solved it:** I connected with `openssl s_client -connect [host]:[port]`, then used the password obtained earlier to get the next piece of data.

```bash
openssl s_client -connect [host]:[port]
# interact and provide the password when requested
```

---

## Level 16 → Level 17

**What to do:** Scan a port range to find an unusual service, connect with OpenSSL, and extract a private key.

**Why it's tricky:** You must discover the service by scanning a port range, then use `openssl s_client` again to interact and retrieve sensitive data (a private key) from the service.

**How I solved it:** I ran `nmap` over the port range to find open/interesting ports. I then used `openssl s_client` to connect to the target port and supplied the password. The service returned a private key blob; I saved that blob to a file and used it for the final SSH step to the next level.

```bash
nmap -p31000-32000 [host]
openssl s_client -connect [host]:[port]
# save the returned private key to a file and set permissions
chmod 600 keyfile
ssh -i keyfile bandit17@[host]
```

---

## Level 17 → Level 18

**What to do:** Compare two files and identify which changed line contains the password.

**Why it's tricky:** `diff` marks lines that differ with `<` and `>`. You must interpret which direction corresponds to the newest or intended content.

**How I solved it:** I ran `diff file1 file2`. The password appeared on a line prefixed by `>` which indicates the newer or second file’s differing line — that is the password to use.

```bash
diff file1 file2
# password is on the line starting with '>'
```

---

## Level 18 → Level 19

**What to do:** SSH in but force a pseudo-tty and run `/bin/sh` directly.

**Why it's tricky:** The remote login does not behave like a normal interactive shell; you must force a TTY and execute a shell program directly to interact.

**How I solved it:** I used the `-t` flag to force allocation of a pseudo-tty and specified `/bin/sh` as the remote command.

```bash
ssh -t [user]@[host] /bin/sh
```

After spawning the shell, I was able to run commands and retrieve the password.

---

## Level 19 → Level 20

**What to do:** Use a program in the home directory that allows you to act as another user to read that user’s password file.

**Why it's tricky:** The helper program is set up to run with elevated privileges or as a different user (setuid or configured to allow switching). You must use it correctly to access a file you normally cannot.

**How I solved it:** I executed the helper program as instructed by the level, then used the privileges it granted to read the target password file (for example, `cat /home/bandit20/…`). The output contained the final password.

```bash
./helper_program
cat /home/bandit20/[password_file]
```

---

### Closing notes

* This walkthrough emphasizes the commands I used and the reasoning behind each step. It demonstrates practical comfort with shell syntax, `find`, `grep`, `diff`, compression tools, `openssl`, `nmap`, `nc`, and basic SSH key handling.

