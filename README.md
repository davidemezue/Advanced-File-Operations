# Advanced File Operations

## Lab Objective
Learn how to use some advanced file operations for easier file manipulation: creating, reading, writing, truncating, changing permissions and ownership of, moving, and deleting files.

## Lab Purpose
File operations are used in Linux to create, read, write, reposition, delete, and truncate files. Controlling who can access a file is a core skill in system administration and security testing.

## Lab Tool
- Kali Linux

## Lab Topology
You can use Kali Linux in a VM for this lab. Everything is done in a practice folder, so no real system files are touched.

---

## Lab Walkthrough

### Task 1: Setup

Open a terminal in Kali and create a safe practice area.

```bash
mkdir -p ~/lab
cd ~/lab
touch file.bin
ls -l file.bin
```

You should see an empty file (size 0) owned by your user, with permissions like -rw-r--r--.

Quick file operations to try:

```bash
echo "hello lab" > file.bin     # write (overwrites)
echo "second line" >> file.bin  # append
cat file.bin                    # read
truncate -s 0 file.bin          # empty the file
ls -l file.bin                  # size is back to 0
```

### Task 2: chmod (change permissions)

Permissions are set for three groups, always in this order: user (owner), group, others. Each group gets one digit made by adding these values:

| Permission | Value |
|---|---:|
| r (read) | 4 |
| w (write) | 2 |
| x (execute) | 1 |

Examples: 6 = 4+2 = rw-, 4 = r--, 5 = 4+1 = r-x, 7 = rwx, 0 = no access.

**Numeric mode:**

```bash
chmod 645 file.bin
ls -l file.bin      # expect: -rw-r--r-x
```

Here the user gets read and write (6), the group gets read only (4), and others get read and execute (5).

```bash
chmod 777 file.bin
ls -l file.bin      # expect: -rwxrwxrwx
```

777 lets everyone read, write, and execute. Use it only for practice, never on real files.

**Symbolic mode** (u = user, g = group, o = others, a = all; + adds, - removes, = sets exactly):

```bash
chmod a-rwx file.bin
ls -l file.bin      # expect: ----------

chmod u+rw file.bin
ls -l file.bin      # expect: -rw-------

chmod g+r,o+r file.bin
ls -l file.bin      # expect: -rw-r--r--
```

**Challenge:** Set the file so only the owner can read and write it.

Answer:

```bash
chmod 600 file.bin
```

### Task 3: chown (change owner and group)

This changes who owns a file. You normally need root privileges, so use sudo.

First confirm the user and group exist:

```bash
id kali
```

Then:

```bash
sudo chown kali:kali file.bin   # set owner and group
ls -l file.bin                  # expect: kali kali

sudo chown :root file.bin       # change only the group (note the colon)
ls -l file.bin                  # expect: kali root

sudo chown kali:kali file.bin   # restore it
```

If your username is not kali, replace it with your own (run whoami to check).

### Task 4: mv (move and rename)

```bash
mv file.bin /tmp/               # move the file to /tmp
ls /tmp/file.bin                # confirm it is there

mv /tmp/file.bin ~/lab/         # move it back
mv file.bin renamed.bin         # rename it
ls -l                           # confirm renamed.bin exists
```

**Note:** if the destination already has a file with the same name, mv overwrites it silently. Use mv -i to be asked first.

### Task 5: rm (remove files and directories)

**Warning:** rm is permanent. There is no recycle bin, and a wrong path can cause irreversible damage. Never run it against system folders such as /usr/share/wordlists.

Create a throwaway directory to practice on:

```bash
mkdir -p ~/lab/wordlists_test
touch ~/lab/wordlists_test/a.txt ~/lab/wordlists_test/b.txt
ls ~/lab/wordlists_test         # check what is inside before deleting
```

Delete it: rm -r wordlists_test

```bash
rm -rfv ~/lab/wordlists_test
```

- -r recurses into directories
- -f forces deletion without prompts
- -v prints each item as it is removed

Delete a single file with a confirmation prompt:

```bash
ls
rm -i renamed.bin
cd ~
```

You’ll see;

```text
rm: remove regular empty file 'renamed.bin'?
```

Type;

```text
Y
```

### Cleanup

```bash
cd ~
rm -rv ~/lab
```

---

## Conclusion

This lab explores advanced Linux file operations, including file manipulation, permission management, ownership changes, moving and renaming files, and deleting files and directories.
