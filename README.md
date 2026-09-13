# ForgeOS

An x86_64 operating system, built by hand. Custom bootloader, custom kernel, custom shell — no Linux underneath.

```
forge:~$ fastfetch
   ______
  /|_||_\`.__
 (   _    _ _\
 =`-(_)--(_)-'
   F O R G E

OS:       ForgeOS x86_64
Kernel:   forge-kernel (custom, C + asm)
Shell:    forge-sh (builtin)
Mascot:   Rivet the anvil-bot
Arch:     x86_64 (long mode)
Filesys:  ramfs (volatile)
Pkg Mgr:  fpkg
```

## What this is

Every layer is written from scratch:

- **Bootloader** — a 512-byte boot sector that loads the kernel via BIOS `INT 13h` and enters 32-bit protected mode.
- **Kernel** — hand-rolled page tables carry it into 64-bit long mode, where a small C kernel takes over.
- **Drivers** — VGA text-mode output and a polling PS/2 keyboard driver, written directly against hardware ports.
- **Shell** — a command interpreter with a real (if temporary) in-RAM filesystem: `ls`, `cat`, `write`, `rm`, `fastfetch`, and more.
- **fpkg** — a package manager designed around static file hosting, so publishing a package never needs a server.

## What's missing (honestly)

No GUI, no persistent disk filesystem, no network stack. `fpkg install` is a stub until networking lands. See [Roadmap](#roadmap).

## Building from source

Requires `nasm`, `gcc`, and `xorriso`.

```bash
git clone https://github.com/forge0s/forge-packages.git
cd forgeos
bash build/build.sh
```

Produces `build/forgeos.iso`. Test it:

```bash
qemu-system-x86_64 -cdrom build/forgeos.iso
```

Or flash it to a USB drive and boot real hardware.

## Repo layout

```
boot/
  stage1.asm    16-bit boot sector, loads kernel, enters protected mode
  kentry.asm    32-bit to 64-bit long mode transition
kernel/
  kernel.c      shell + command dispatcher
  vga.c/.h      VGA text-mode driver
  keyboard.c/.h PS/2 keyboard driver (polling, scancode set 1)
  kstring.c/.h  freestanding string helpers (no libc)
  ramfs.c/.h    in-memory filesystem
  link.ld       linker script, loads kernel at 0x8000
build/
  build.sh      assembles, compiles, links, masters the ISO
packages/       fpkg packages (see PACKAGING.md)
packages.json   package index read by fpkg
```

## Packaging

See [PACKAGING.md](PACKAGING.md) for the manifest format and how to publish a package.

## Roadmap

In order of payoff:

1. Boot-test on real hardware and in QEMU/VirtualBox.
2. A disk driver (ATA/IDE PIO) plus a real filesystem, so state survives a reboot.
3. A NIC driver (RTL8139 is the usual first target) and a minimal TCP/IP stack.
4. Wire `fpkg install` to actually fetch from a live `packages.json`.
5. A userspace and ELF loader, so packages stop needing to be kernel-builtins.
6. A graphics-mode framebuffer, then a compositor, then a desktop environment.

## Mascot

Rivet — an anvil with legs. Building an OS is closer to blacksmithing than software engineering: you heat the same piece of metal a hundred times before it holds its shape.
