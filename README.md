# PYLT

PYLT (Press You Long Time) is a little program to implement long-press to show special characters (like your cell phone or macOS) in Linux.

## NOTES

Sorry, I'm using this README for notes for now.

### Disable key repeat

So that we can long-press, we have to disable key repeat. We could use`xset r off` to disable all keys. However, we still want some keys (like arrows) to repeat.

Therefore--and like https://unix.stackexchange.com/a/307649/262190 is saying, we use the following to individually disable all keys (8 is the first keycode, 255 the last one):

```bash
seq 8 255 | xargs -n 1 xset -r
```

Then, we need to parse `xmodmap -pk` to find the correct keycodes for our specific system for all keys that we want to repeat. Then, key by key we run (`xxx` is the keycode):

```bash
xset r xxx
```

### Detect long-press

This guy: https://unix.stackexchange.com/a/320474/262190 seems to be the only on the internet who knows what's going on with this. For now we can just use his script, and launch our program instead of the one he launches.

