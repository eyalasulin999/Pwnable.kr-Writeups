# Pwnable.kr-Writeups

## fd

Each process has its own FDs
Which FDs every process has?

```bash
# ls -l /proc/self/fd
lrwx------ 1 fd fd 64 May 30 19:02 0 -> /dev/pts/59
lrwx------ 1 fd fd 64 May 30 19:02 1 -> /dev/pts/59
lrwx------ 1 fd fd 64 May 30 19:02 2 -> /dev/pts/59
lr-x------ 1 fd fd 64 May 30 19:02 3 -> /proc/3560182/fd
```

```yaml
0: stdin
1: stdout
2: stderr
```

Just read from stdin fd

```bash
echo LETMEWIN | ./fd $((0x1234))
# Mama! Now_I_understand_what_file_descriptors_are!
```
