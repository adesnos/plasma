Reverse
=======

Reverse engineering for x86/ARM/MIPS binaries. Generate a more readable code
(pseudo-C) with colored syntax.

Supported formats : `ELF`, `PE`, `RAW`.


The `Makefile` is used only for checking tests.


## Requirements

    python >= 3.4
    capstone + python bindings (see requirements.sh)
    python-pyelftools
    https://github.com/simonzack/pefile-py3k
    terminal with 256 colors (if not use the option `--nocolor`)

For Python binding of [Capstone engine](http://www.capstone-engine.org), you 
can install it from PyPi, like followings: 

    sudo pip3 install capstone

You can also run `requirements.sh` which will retrieve all requirements.


## Pseudo-decompilation of functions

The option `-x main` is optional because the binary contains the symbol main.

    $ ./reverse.py tests/server.bin

![reverse](/images/screenshot.png?raw=true)


## Interactive mode (`-i`)

More commands are available in this mode (`da`, `db`, ...). See `help`
for a full list.


## Analyze shellcodes

For every `int 0x80`, the tool try to detect syscalls with parameters.

    $ ./reverse.py --raw x86 tests/shellcode.bin
    function 0x0 {
        0x0: eax = 0 # xor eax, eax
        0x2: al = '\x0b' # mov al, 0xb
        0x4: cdq
        0x5: push edx
        0x6: push 1752379246 "n/sh"
        0xb: push 1768042287 "//bi"
        0x10: ebx = esp # mov ebx, esp
        0x12: push edx
        0x13: push ebx
        0x14: ecx = esp # mov ecx, esp
        0x16: int 128 ; execve(ebx, ecx, edx) # int 0x80
    }


## Edit with vim

    $ ./reverse tests/dowhile1.bin --vim
    You can now run : vim dowhile1.bin.rev -S dowhile1.bin.vim


## Custom colors

At the first run, `reverse.py` creates a new file `custom_colors.py` with
default values. Here you can set your own colors.
