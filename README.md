# PYLT

PYLT (Press You Long Time) is a little utility to detect long presses on Linux. It lets you specify the delay after which is considered a long press, and a command to run when a long press event is triggered.

## NOTES

Sorry, I'm using this README for notes for now.

### Disable key repeat

So that we can long press, you might want to disable key repeat. 

We could use`xset r off` to disable all keys. However, we still want some keys (like arrows) to repeat. Therefore--and like https://unix.stackexchange.com/a/307649/262190 is saying, we use the following to individually disable all keys (8 is the first keycode, 255 the last one):

```bash
seq 8 255 | xargs -n 1 xset -r
```

Then, we need to parse `xmodmap -pk` to find the correct keycodes for our specific system for all keys that we want to repeat. Then, key by key we run (`xxx` is the keycode):

```bash
xset r xxx
```

### Detect long presses

This guy: https://unix.stackexchange.com/a/320474/262190 seems to be the only on the internet who knows what's going on with this.

It worked great, so I started from his script (which itself is a fork of `evtest` if I understood correctly).

You can find the source code in pylt-long-pressed.c, which you can compile with:

```bash
gcc pylt-long-pressed.c -o pylt-long-pressed
```

You need GCC to compile, as well as `evtest` installed. You can install `evtest` on Ubuntu with:

```bash
sudo apt install evtest
```

Run as sudo, with (change "firefox" into the command to execute, 0.75 into your desired delay in seconds):

```bash
sudo ./pylt-long-pressed `ls -la /dev/input/by-id/* | grep kbd | echo "/dev/input/""$(awk -F'/' '{print $NF}')"` 0.75 "sudo -H -u $(whoami) firefox"
```

