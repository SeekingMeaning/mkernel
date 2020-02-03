mkernel-msys2-mingw64
=====================

A fork of [arjun024/mkernel](https://github.com/arjun024/mkernel) for MSYS2 MinGW 64-bit

This is a minimalist kernel which prints "`my first kernel`" on the screen and then hangs.

* The kernel is multi-boot compliant and loads with GRUB.


#### Blog post ####

[Kernel 101 – Let’s write a Kernel](http://arjunsreedharan.org/post/82710718100/kernel-101-lets-write-a-kernel)

#### Prerequisites ####

1. Install [MSYS2](https://www.msys2.org/) x86_64
2. Open MSYS2 MinGW 64-bit terminal
3. Install dependencies (nasm, gcc, binutils, qemu):

```
pacman -S mingw-w64-x86_64-nasm mingw-w64-x86_64-gcc mingw-w64-x86_64-binutils mingw-w64-x86_64-qemu
```

(Note: You may or may not have to install other missing dependencies.
In addition to the ones listed above, I also have `base-devel` and
`mingw-w64-x86_64-toolchain` installed.)

#### Build commands ####
```
nasm -f elf32 kernel.asm -o kasm.o
```
```
gcc -m32 -c kernel.c -o kc.o
```
```
ld -m i386pe -T link.ld -o kernel kasm.o kc.o
```
```
objcopy -O elf32-i386 kernel kernel.elf
```

(Note: The commands `ld` and `qemu-system-i386` slightly differ from
those of the original mkernel. [The standard MinGW/64 LD linker doesn't
output ELF binaries.](https://stackoverflow.com/a/34761095) Because of
this, the `objcopy` command is added to convert from PEI-I386 to ELF.)

#### Test on emulator ####
```
qemu-system-i386 -kernel kernel.elf
```

#### Get to boot ####
GRUB requires your kernel executable to be of the pattern `kernel-<version>`.

So, rename the kernel:

```
mv kernel.elf kernel-701
```

Copy it to your boot partition (assuming you are superuser):

```
cp kernel-701 /boot/kernel-701
```

Configure your grub/grub2 similar to what is given in `_grub_grub2_config` folder.

Reboot.

Voila!

![kernel screenshot](http://static.tumblr.com/gltvynn/yOdn443dr/mkernel.png "Screenshot")

#### The next step ####
See [mkeykernel repo](//github.com/arjun024/mkeykernel)
