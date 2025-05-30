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

## collision

`char` is 1 byte, `int` is 4 bytes

So, every 4 chars "merged" into 1 integer

```
goal is 0x21DD09EC = 568134124

568134124 / 5 = 113626824.8

0.8 * 5 = 4

4x 113626825 
1x 113626824

hex(113626825) = 6C5CEC9
hex(113626824) = 6C5CEC8
```

We want to pass that hex as argument - 6C5CEC96C5CEC96C5CEC96C5CEC96C5CEC8

Notice endianness - Little!

```bash
./col $(echo -en '\xC9\xCE\xC5\x06\xC9\xCE\xC5\x06\xC9\xCE\xC5\x06\xC9\xCE\xC5\x06\xC8\xCE\xC5\x06')
# Two_hash_collision_Nicely
```
